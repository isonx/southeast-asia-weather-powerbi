# Data Dictionary

## `Locations`

| Column | Type | Visibility | Description |
|---|---|---|---|
| `CityKey` | Whole number | Hidden | Stable surrogate key used in all relationships. |
| `City` | Text | Visible | City name used by the report slicer and city card. |
| `Country` | Text | Visible | Country name associated with the city. |
| `Latitude` | Decimal number | Hidden | WGS84 latitude passed to the APIs. |
| `Longitude` | Decimal number | Hidden | WGS84 longitude passed to the APIs. |
| `Timezone` | Text | Hidden | IANA timezone identifier retained as location metadata. |

## `CurrentWeather`

| Column | Type | Visibility | Description |
|---|---|---|---|
| `CityKey` | Whole number | Hidden | Foreign key to `Locations`. |
| `UpdatedAt` | Date/time | Visible | Current-weather timestamp returned by Open-Meteo. |
| `Temperature` | Decimal number | Hidden | Current 2-metre air temperature in °C. |
| `RelativeHumidity` | Decimal number | Hidden | Relative humidity percentage returned by the API. |
| `ApparentTemperature` | Decimal number | Hidden | Perceived or apparent temperature in °C. |
| `IsDay` | Whole number | Hidden | Day/night flag used by the icon mapping. |
| `Precipitation` | Decimal number | Hidden | Current precipitation in millimetres. |
| `WeatherCode` | Whole number | Hidden | WMO weather interpretation code. |
| `WeatherCondition` | Text | Visible | Human-readable condition derived from `WeatherCode`. |
| `WeatherIcon` | Text | Visible | Unicode icon derived from weather code and day/night flag. |
| `SurfacePressure` | Decimal number | Hidden | Surface pressure in hPa. |
| `WindSpeed` | Decimal number | Hidden | Wind speed at 10 metres in km/h. |
| `Visibility` | Decimal number | Hidden | Visibility in metres. |

## `DailyForecast`

| Column | Type | Visibility | Description |
|---|---|---|---|
| `Date` | Date | Visible | Forecast date used on chart axes. |
| `WeatherCode` | Whole number | Hidden | Daily WMO weather interpretation code. |
| `MaxTemperature` | Decimal number | Hidden | Forecast daily maximum temperature in °C. |
| `MinTemperature` | Decimal number | Hidden | Forecast daily minimum temperature in °C. |
| `PrecipProbabilityMax` | Decimal number | Hidden | Maximum daily precipitation probability in percent. |
| `PrecipitationSum` | Decimal number | Hidden | Forecast daily total precipitation in millimetres. |
| `Sunrise` | Date/time | Hidden | Forecast sunrise time. |
| `Sunset` | Date/time | Hidden | Forecast sunset time. |
| `WindSpeedMax` | Decimal number | Hidden | Forecast maximum 10-metre wind speed in km/h. |
| `CityKey` | Whole number | Hidden | Foreign key to `Locations`. |
| `WeatherCondition` | Text | Visible | Readable daily condition derived from weather code. |
| `DayName` | Text | Visible | Abbreviated English weekday name. |

## `AirQuality`

| Column | Type | Visibility | Description |
|---|---|---|---|
| `CityKey` | Whole number | Hidden | Foreign key to `Locations`. |
| `UpdatedAt` | Date/time | Visible | Air-quality timestamp returned by the API. |
| `USAQI` | Decimal number | Hidden | Consolidated US Air Quality Index. |
| `EuropeanAQI` | Decimal number | Hidden | Consolidated European Air Quality Index. |
| `PM10` | Decimal number | Hidden | PM10 concentration in µg/m³. |
| `PM2_5` | Decimal number | Hidden | PM2.5 concentration in µg/m³. |
| `CarbonMonoxide` | Decimal number | Hidden | Carbon monoxide concentration in µg/m³. |
| `NitrogenDioxide` | Decimal number | Hidden | Nitrogen dioxide concentration in µg/m³. |
| `SulphurDioxide` | Decimal number | Hidden | Sulphur dioxide concentration in µg/m³. |
| `Ozone` | Decimal number | Hidden | Ozone concentration in µg/m³. |

## `_Measures`

| Column | Type | Visibility | Description |
|---|---|---|---|
| `Key` | Whole number | Hidden | Dummy value used only to create a dedicated measure table. |

All business-facing calculations are DAX measures documented in `DAX_MEASURES.md`.
