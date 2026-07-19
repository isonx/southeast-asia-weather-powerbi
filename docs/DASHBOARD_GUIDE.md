# Dashboard Guide

## Report page

The PBIX contains one page named **Weather Intelligence**, designed at 1280 × 720 pixels and configured to fit the page.

## Visual inventory

The current report definition contains 42 visual objects:

| Visual type | Count | Use |
|---|---:|---|
| Card | 17 | Current conditions, timestamp, AQI category, astronomy, pollutants, and supporting KPIs. |
| Shape | 12 | Containers, panels, and section backgrounds. |
| Text box | 9 | Main heading, section titles, labels, and pollutant abbreviations. |
| Slicer | 1 | Single-city selection. |
| Line chart | 1 | Seven-day maximum/minimum temperature trend. |
| Clustered column chart | 1 | Daily maximum rain probability. |
| Gauge | 1 | US AQI against a 0–300 display range and target 100. |

## Interaction behavior

### City slicer

The slicer uses `Locations[City]`. It is intended for single selection. All analytical tables are filtered through the location relationships.

### Seven-day temperature chart

- Axis: `DailyForecast[Date]`
- Values: `Daily Max Temperature`, `Daily Min Temperature`
- Purpose: Compare expected daily high and low temperatures.

### Daily rain probability chart

- Axis: `DailyForecast[Date]`
- Value: `Rain Probability`
- Purpose: Show the highest expected probability of precipitation on each forecast date.

### AQI gauge

- Value: `US AQI`
- Target: `AQI Target` = 100
- Maximum: `AQI Maximum` = 300
- Category card: `AQI Category`
- Conditional color source: `AQI Category Color`

The target line indicates the upper boundary of the Moderate category. The color measure can be applied to supported text or callout properties through **Format style → Field value**.

### Pollutant cards

The dashboard shows current concentrations for:

- PM2.5
- PM10
- Ozone (O₃)
- Nitrogen dioxide (NO₂)
- Sulphur dioxide (SO₂)
- Carbon monoxide (CO)

## Design system

The report uses:

- Dark gray outer canvas and header.
- White content panels.
- Orange section headers.
- Blue chart series and UI accents.
- High-contrast dark text for KPI values.
- Custom AQI colors for conditional formatting.

## Static preview versus interactive report

`assets/dashboard-preview.png` is a static image for GitHub and portfolio pages. It cannot respond to slicers. The interactive version is the PBIX file in `powerbi/`.
