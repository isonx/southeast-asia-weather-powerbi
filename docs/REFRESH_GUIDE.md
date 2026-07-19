# Download, Open, and Refresh Guide

## Download

Download the report from:

```text
powerbi/SEAWeatherIntelligence.pbix
```

On GitHub, open the file and select **Download raw file**, or use the direct download link in the main README.

## Software requirement

Use Microsoft Power BI Desktop. The report was finalized in a June 2026 Power BI Desktop environment. A current or newer compatible version is recommended.

## Manual refresh in Power BI Desktop

1. Open `SEAWeatherIntelligence.pbix`.
2. Select **Home → Refresh**.
3. If prompted for credentials, configure:
   - `https://api.open-meteo.com` — Anonymous, Public.
   - `https://air-quality-api.open-meteo.com` — Anonymous, Public.
4. Wait for the load dialog to finish.
5. Verify that the **Updated** card displays a recent API timestamp.
6. Test at least two cities in the slicer.
7. Save the file to retain the newly imported data.

## What refresh updates

A refresh reloads:

- Current temperature and weather condition.
- Apparent temperature, humidity, wind, pressure, visibility, and precipitation.
- Seven-day forecast dates and temperatures.
- Daily precipitation probabilities and totals.
- Sunrise and sunset.
- US and European AQI.
- PM2.5, PM10, O₃, NO₂, SO₂, and CO.
- The API timestamp shown in the header.

## What the slicer does

The city slicer does not call the API. It filters all six cities already imported during the last refresh. This makes interaction fast and prevents a web request every time the selection changes.

## Power BI Service

After publishing, configure the semantic model's web-source credentials as Anonymous. Scheduled refresh availability depends on the account, workspace, tenant settings, and licence configuration.

## GitHub limitation

GitHub cannot execute or automatically refresh a PBIX file. The repository provides source, documentation, a preview image, and a downloadable report. Interactive exploration requires Power BI Desktop or an authorized Power BI web deployment.
