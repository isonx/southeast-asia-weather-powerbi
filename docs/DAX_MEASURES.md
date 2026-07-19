# DAX Measures Reference

The measures are stored in the `_Measures` table. Most calculations respond to the city filter propagated from `Locations`.

## Context

### `Selected City`

```DAX
Selected City =
SELECTEDVALUE(Locations[City], "Bangkok")
```

Returns the selected city name. If no single city is selected, Bangkok is used as the fallback label.

### `Selected Country`

```DAX
Selected Country =
SELECTEDVALUE(Locations[Country], "Thailand")
```

Returns the selected country's name, with Thailand as the fallback.

### `Last Updated`

```DAX
Last Updated =
VAR T = MAX(CurrentWeather[UpdatedAt])
RETURN
    IF(
        ISBLANK(T),
        "Refresh required",
        "Updated " & FORMAT(T, "dd MMM yyyy HH:mm")
    )
```

Displays the latest current-weather timestamp within the selected city context. It reports the API data timestamp, not the computer's current clock time.

## Current Weather

### `Current Temperature`

```DAX
Current Temperature = MAX(CurrentWeather[Temperature])
```

Returns current 2-metre temperature for the selected city. Format: `0.0 °C`.

### `Feels Like`

```DAX
Feels Like = MAX(CurrentWeather[ApparentTemperature])
```

Returns apparent temperature. Format: `0.0 °C`.

### `Humidity`

```DAX
Humidity =
DIVIDE(MAX(CurrentWeather[RelativeHumidity]), 100)
```

Converts the API's percentage value into a decimal so Power BI can display it using the `0%` format.

### `Wind Speed`

```DAX
Wind Speed = MAX(CurrentWeather[WindSpeed])
```

Returns current wind speed at 10 metres. Format: `0.0 km/h`.

### `Surface Pressure`

```DAX
Surface Pressure = MAX(CurrentWeather[SurfacePressure])
```

Returns current surface pressure. Format: `0 hPa`.

### `Visibility Km`

```DAX
Visibility Km =
DIVIDE(MAX(CurrentWeather[Visibility]), 1000)
```

Converts visibility from metres to kilometres. Format: `0.0 km`.

### `Current Precipitation`

```DAX
Current Precipitation = MAX(CurrentWeather[Precipitation])
```

Returns the current precipitation value. Format: `0.0 mm`.

### `Current Weather`

```DAX
Current Weather =
SELECTEDVALUE(CurrentWeather[WeatherCondition], "Unknown")
```

Returns the readable condition produced by the Power Query weather-code function.

### `Weather Icon`

```DAX
Weather Icon =
SELECTEDVALUE(CurrentWeather[WeatherIcon], "•")
```

Returns the Unicode weather icon corresponding to current conditions and day/night status.

## Astronomy

### `Sunrise`

```DAX
Sunrise =
VAR T = MIN(DailyForecast[Sunrise])
RETURN IF(ISBLANK(T), "—", FORMAT(T, "HH:mm"))
```

Returns the earliest sunrise in the currently filtered forecast context. On the dashboard, the visual context resolves this to the first displayed forecast day.

### `Sunset`

```DAX
Sunset =
VAR T = MIN(DailyForecast[Sunset])
RETURN IF(ISBLANK(T), "—", FORMAT(T, "HH:mm"))
```

Returns the earliest sunset in the current forecast context, formatted as local time.

## Air Quality

### `US AQI`

```DAX
US AQI = MAX(AirQuality[USAQI])
```

Returns the selected city's consolidated US Air Quality Index.

### `European AQI`

```DAX
European AQI = MAX(AirQuality[EuropeanAQI])
```

Returns the selected city's consolidated European Air Quality Index.

### `AQI Maximum`

```DAX
AQI Maximum = 300
```

Defines the displayed maximum of the gauge. Values above 300 still belong to the hazardous category, but the current gauge scale ends at 300.

### `AQI Target`

```DAX
AQI Target = 100
```

Provides the gauge reference line. The line marks the upper boundary of the Moderate US AQI category.

### `AQI Category`

```DAX
AQI Category =
VAR A = [US AQI]
RETURN
    SWITCH(
        TRUE(),
        ISBLANK(A), "Refresh required",
        A <= 50, "Good",
        A <= 100, "Moderate",
        A <= 150, "Unhealthy for sensitive groups",
        A <= 200, "Unhealthy",
        A <= 300, "Very unhealthy",
        "Hazardous"
    )
```

Converts the numeric US AQI into a readable category.

### `AQI Category Color`

```DAX
AQI Category Color =
VAR A = [US AQI]
RETURN
    SWITCH(
        TRUE(),
        ISBLANK(A), "#BDBDBD",
        A <= 50, "#00B050",
        A <= 100, "#1E88E5",
        A <= 150, "#FF7B7B",
        A <= 200, "#800000",
        A <= 300, "#FF0000",
        "#9B0000"
    )
```

Returns a hexadecimal color string for conditional formatting. The project uses a custom palette requested for the dashboard; these are not the official US AQI colors.

## Pollutants

### `PM2.5`

```DAX
PM2.5 = MAX(AirQuality[PM2_5])
```

Returns PM2.5 concentration. Format: `0.0 µg/m³`.

### `PM10`

```DAX
PM10 = MAX(AirQuality[PM10])
```

Returns PM10 concentration. Format: `0.0 µg/m³`.

### `Ozone`

```DAX
Ozone = MAX(AirQuality[Ozone])
```

Returns ozone concentration. Format: `0.0 µg/m³`.

### `Nitrogen Dioxide`

```DAX
Nitrogen Dioxide = MAX(AirQuality[NitrogenDioxide])
```

Returns nitrogen dioxide concentration. Format: `0.0 µg/m³`.

### `Sulphur Dioxide`

```DAX
Sulphur Dioxide = MAX(AirQuality[SulphurDioxide])
```

Returns sulphur dioxide concentration. Format: `0.0 µg/m³`.

### `Carbon Monoxide`

```DAX
Carbon Monoxide = MAX(AirQuality[CarbonMonoxide])
```

Returns carbon monoxide concentration. Format: `0.0 µg/m³`.

## Forecast

### `Daily Max Temperature`

```DAX
Daily Max Temperature = MAX(DailyForecast[MaxTemperature])
```

Returns maximum forecast temperature for each date point. Format: `0.0 °C`.

### `Daily Min Temperature`

```DAX
Daily Min Temperature = MIN(DailyForecast[MinTemperature])
```

Returns minimum forecast temperature for each date point. Format: `0.0 °C`.

### `Rain Probability`

```DAX
Rain Probability =
DIVIDE(MAX(DailyForecast[PrecipProbabilityMax]), 100)
```

Converts the daily maximum precipitation probability from percentage points to a decimal. Format: `0%`.

### `Daily Rainfall`

```DAX
Daily Rainfall = MAX(DailyForecast[PrecipitationSum])
```

Returns forecast daily precipitation sum. Format: `0.0 mm`.

## Measure count

| Folder | Measures |
|---|---:|
| Context | 3 |
| Current Weather | 9 |
| Astronomy | 2 |
| Air Quality | 6 |
| Pollutants | 6 |
| Forecast | 4 |
| **Total** | **30** |
