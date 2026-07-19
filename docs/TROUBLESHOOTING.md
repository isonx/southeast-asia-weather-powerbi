# Troubleshooting

## `Found invalid data while decoding`

The current source project requests uncompressed JSON and explicitly decodes the response as UTF-8. Confirm that you are using the PBIX supplied in this repository. Then:

1. Open **File → Options and settings → Data source settings**.
2. Clear permissions for both Open-Meteo domains.
3. Refresh again.
4. Choose Anonymous authentication and Public privacy.

## Blank cards or charts

The PBIX may contain an older cached snapshot or an interrupted refresh. Select **Home → Refresh** and wait for all three API tables to finish loading.

## API credential prompt repeats

Check both of these domains separately:

```text
https://api.open-meteo.com
https://air-quality-api.open-meteo.com
```

Set each one to Anonymous and Public.

## City slicer does not update all visuals

Verify the relationships in Model view:

- `Locations[CityKey]` → `CurrentWeather[CityKey]`
- `Locations[CityKey]` → `DailyForecast[CityKey]`
- `Locations[CityKey]` → `AirQuality[CityKey]`

The `Locations` side must be unique and the filter direction should flow from `Locations` to the three data tables.

## Weather icon displays as a square

The icons are Unicode characters. Rendering depends on the installed font and Power BI environment. Replace the icon font or use image URLs if a device does not support the characters.

## AQI color measure cannot be selected

In the conditional-formatting dialog, change **Format style** from Gradient or Rules to **Field value**, then choose `_Measures[AQI Category Color]`.

## AQI gauge arc does not change color

Some properties of Power BI's native gauge do not support field-value conditional formatting. The color measure can still control supported callout, text, label, or card properties.

## Report shows old dates

The model is Import mode. Select **Home → Refresh**. Opening or changing the slicer alone does not retrieve new data.

## API temporarily unavailable

Wait and retry. The project uses a two-minute timeout per API request. Availability and accuracy are not guaranteed by the data provider.
