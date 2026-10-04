# ✈️ Global Aviation Accident Analysis — Power BI Dashboard

## 📖 Overview
A 3-page Power BI dashboard analyzing 5,035+ global aviation accident records from 1908–2024,
covering crash frequency, fatalities, survival rates, manufacturers, aircraft, locations, and
time-based trends.

## 🗂️ Data Source
- 📄 File: `aircrashes_cleaned_for_powerbi.csv`
- 🧱 Table name in model: `Air_Crash_Data`
- 📊 Original columns: Year, Quarter, Month, Day, Country/Region, Aircraft Manufacturer, Aircraft,
  Location, Operator, Ground, Aboard, TotalFatalities
- ➕ Added columns: Decade, MonthNum, TotalPeople, Survivors, SurvivalRate (%), FatalityRate (%)

> ⚠️ **Known data quality note:** `Operator`, `Location`, and `Country/Region` text fields have
> scrambled word order in the source data (inconsistent across rows for the same entity), so
> visuals built on exact-match grouping of these fields (e.g. operator rankings) should be treated
> as indicative, not fully accurate.

## 🗺️ Pages

### 🏠 1. Home
First-view/summary page.
- 🎯 KPI card strip: Total_Crashes, Total_Onboard, Total_Survivors, Total_Fatality, Darkest_Year,
  Darkest_Location
- ✈️ Top 10 Aircraft (bar chart)
- 🏢 Top 5 Operators (bar chart)
- 📍 Top 10 Locations (bar chart)
- 🌍 World map of crash locations (bubble = frequency)

### 🎛️ 2. Cockpit
Trend and seasonality view, styled with gauge/instrument-panel feel.
- 🔍 Filter panel: Aircraft, Country slicers
- 🥧 Quarter distribution (donut/pie chart)
- 📈 Survivors vs Total Onboard by Year (line/area chart)
- 💀 Fatalities by Year (area chart)
- 📍 Crash by Location (bar chart, top entries)
- 📅 Crash by Month (column chart)

### 🗃️ 3. Black Box
Detailed record-level table for drill-down.
- 🎚️ Slicers: Year, Aircraft, Quarter, Country (with Clear All button)
- 📋 Full data table: Year, Month, Aircraft, Location, Aircraft Manufacturer, Operator, Country,
  Quarter, Total Fatalities, Survivor

## 🎨 Design
- 🌑 Theme: black/dark background with gold/yellow accents, subtle star-field texture
- 🛩️ Aviation motifs: plane silhouettes, radar/cockpit visual language
- 🧭 Navigation: left-side icon buttons (Home, Cockpit, Black Box) with bookmark-based page links

## 🛠️ How to Rebuild / Extend
1. 📥 Import `aircrashes_cleaned_for_powerbi.csv` via Get Data → Text/CSV
2. ⚙️ Apply Power Query steps (type conversions, TotalPeople/SurvivalRate/FatalityRate columns)
3. 🧮 Load the DAX measures from `DAX_Measures.md`
4. 🏗️ Build pages per the layout above
5. 📘 See `PowerBI_Build_Guide.md` for the original detailed build walkthrough

## 📁 Files in this project
- `aircrashes_cleaned_for_powerbi.csv` — cleaned dataset
- `PowerBI_Build_Guide.md` — original page-by-page build guide
- `README.md` — this file
- `DAX_Measures.md` — all DAX measures used in the dashboard
- `dashboard_background.png` — corner-silhouette background image

- ## 👨‍💻 Author

**Nikil Raja**
- B.E. Computer Science Engineering (Data Science Specialization)
- Email- nikilraja31@gmail.com
- Linkedin- www.linkedin.com/in/nikilraja-r31
- Chennai, India
---
