# API Integration

## Overview

The dashboard retrieves weather and air-quality data from Open-Meteo through two HTTPS GET requests that return JSON. The implementation does not embed an API key.

## API 1: Weather Forecast API

**Base URL**

```text
https://api.open-meteo.com/v1/forecast
```

**Purpose**

- Current weather conditions for each city.
- Seven-day daily forecast.
- Sunrise and sunset.
- Daily precipitation probability.

**Request design**

Power Query reads the six rows in `LocationsBase`, converts the latitudes and longitudes into comma-separated strings, and sends one multi-location request.

Conceptual request:

```text
GET /v1/forecast
    ?latitude=<comma-separated latitudes>
    &longitude=<comma-separated longitudes>
    &current=temperature_2m,relative_humidity_2m,apparent_temperature,is_day,precipitation,weather_code,surface_pressure,wind_speed_10m,visibility
    &daily=weather_code,temperature_2m_max,temperature_2m_min,precipitation_probability_max,precipitation_sum,sunrise,sunset,wind_speed_10m_max
    &timezone=auto
    &forecast_days=7
    &temperature_unit=celsius
    &wind_speed_unit=kmh
    &precipitation_unit=mm
```

`timezone=auto` allows each returned location to use its applicable local timezone.

## API 2: Air Quality API

**Base URL**

```text
https://air-quality-api.open-meteo.com/v1/air-quality
```

**Purpose**

- US Air Quality Index.
- European Air Quality Index.
- Particulate matter and gaseous pollutant concentrations.

Conceptual request:

```text
GET /v1/air-quality
    ?latitude=<comma-separated latitudes>
    &longitude=<comma-separated longitudes>
    &current=us_aqi,european_aqi,pm10,pm2_5,carbon_monoxide,nitrogen_dioxide,sulphur_dioxide,ozone
    &timezone=auto
```

## Cities and coordinates

| CityKey | City | Country | Latitude | Longitude | Timezone |
|---:|---|---|---:|---:|---|
| 1 | Bangkok | Thailand | 13.7563 | 100.5018 | Asia/Bangkok |
| 2 | Jakarta | Indonesia | -6.2088 | 106.8456 | Asia/Jakarta |
| 3 | Singapore | Singapore | 1.3521 | 103.8198 | Asia/Singapore |
| 4 | Kuala Lumpur | Malaysia | 3.1390 | 101.6869 | Asia/Kuala_Lumpur |
| 5 | Manila | Philippines | 14.5995 | 120.9842 | Asia/Manila |
| 6 | Ho Chi Minh City | Vietnam | 10.8231 | 106.6297 | Asia/Ho_Chi_Minh |

Fixed coordinates are used instead of city-name geocoding. This prevents ambiguous city matches and makes the model deterministic.

## Response handling

The APIs return one JSON object per coordinate. When multiple coordinates are requested, Power Query expects a list of location objects in the same order as the input coordinate lists.

The implementation adds several reliability controls:

```powerquery
Headers = [
    Accept = "application/json",
    #"Accept-Encoding" = "identity"
],
Timeout = #duration(0, 0, 2, 0)
```

The response is buffered, explicitly decoded as UTF-8, and then parsed:

```powerquery
ResponseBinary = Binary.Buffer(Web.Contents(...)),
ResponseText = Text.FromBinary(ResponseBinary, TextEncoding.Utf8),
Source = Json.Document(ResponseText),
Result = if Value.Is(Source, type list) then Source else {Source}
```

This design was selected to avoid compressed-response decoding issues encountered in some Power BI Desktop environments.

## API-to-model mapping

| API response section | Power BI table | Grain |
|---|---|---|
| Forecast `current` | `CurrentWeather` | One row per city per refresh |
| Forecast `daily` | `DailyForecast` | One row per city and forecast date |
| Air quality `current` | `AirQuality` | One row per city per refresh |
| Static coordinates | `Locations` | One row per city |

## Authentication and privacy

On first refresh, configure both domains as:

- Authentication: **Anonymous**
- Privacy level: **Public**

No password, token, or API key is committed to this repository.

## Refresh behavior

This is an Import-mode model. The API is contacted only when Power BI refreshes the model. Changing the city slicer does not make a new API request; it filters the already imported rows.

## Attribution and commercial use

Open-Meteo API data are offered under CC BY 4.0 and require attribution. The provider's current free-service terms restrict the free endpoint to non-commercial use and specified request limits. For production or paid commercial deployment, review the latest terms and consider a paid endpoint or self-hosted deployment.

Official references:

- https://open-meteo.com/en/docs
- https://open-meteo.com/en/docs/air-quality-api
- https://open-meteo.com/en/licence
- https://open-meteo.com/en/terms
