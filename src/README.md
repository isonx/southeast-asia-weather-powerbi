# Power BI Project Source

This folder contains a Git-friendly Power BI Project (`.pbip`) companion for the finalized PBIX:

- `SEAWeatherIntelligence.pbip`
- `SEAWeatherIntelligence.Report/` — report definitions and theme resources extracted from the uploaded PBIX.
- `SEAWeatherIntelligence.SemanticModel/` — documented TMDL semantic model, relationships, Power Query expressions, and DAX measures corresponding to the project, including the current custom `AQI Category Color` measure.

The exact finalized deliverable uploaded by the project owner is preserved separately in `../powerbi/SEAWeatherIntelligence.pbix`. Use that PBIX when an exact copy of the finished report is required. Use this source folder for Git version control, code review, learning, and model documentation.

Open `SEAWeatherIntelligence.pbip` with a compatible Power BI Desktop version and refresh the public web sources. Machine-local `.pbi` cache files are intentionally excluded.
