# Aircraft Crashes Dashboard — Power BI Build Guide

**File to import:** `aircrashes_cleaned_for_powerbi.csv`

## A note on data quality first
The original `Operator`, `Location`, and `Country/Region` columns have scrambled word order
(e.g. the same airline shows up as "Airlines Alaska", "Alaska Air Fuel", "Inc. Airlines Alaska Wein"
in different rows). That makes them unreliable for grouping, mapping, or ranking — so this guide
skips visuals built on those fields. Everything below uses only the clean columns: dates,
Aircraft Manufacturer, and the casualty numbers.

If you want a real map or operator leaderboard later, that column needs manual re-parsing
(regex/NLP token reordering) — a separate task from the dashboard itself.

## Step 1 — Import
Power BI Desktop → **Get Data → Text/CSV** → select `aircrashes_cleaned_for_powerbi.csv` → Load.

## Step 2 — Data model / column types
In Power Query, set:
- `Year`, `Decade`, `MonthNum`, `Day`, `Ground`, `Aboard`, `Fatalities (air)`, `TotalFatalities`, `Survivors` → Whole Number
- `SurvivalRate`, `FatalityRate` → Decimal Number
- `Quarter`, `Month`, `Aircraft Manufacturer`, `Aircraft` → Text
- Optionally build a proper Date column: `Date = Date.FromText(Text.Combine({Text.From([Year]), Text.From([MonthNum]), Text.From([Day])}, "-"))` (some Day values may be invalid for the month — wrap in `try...otherwise null`)

## Step 3 — DAX measures to create
```
Total Crashes = COUNTROWS(aircrashes)
Total Fatalities = SUM(aircrashes[Fatalities (air)])
Total Ground Fatalities = SUM(aircrashes[Ground])
Total Onboard = SUM(aircrashes[Aboard])
Avg Fatality Rate = AVERAGE(aircrashes[FatalityRate])
Avg Survival Rate = AVERAGE(aircrashes[SurvivalRate])
Fatalities per Crash = DIVIDE([Total Fatalities], [Total Crashes])
Deadliest Crash = MAX(aircrashes[Fatalities (air)])
```

## Step 4 — Page layout (3 pages)

### Page 1: Overview / Safety Trend
```
┌─────────────┬─────────────┬─────────────┬─────────────┐
│Total Crashes│Total Deaths │Avg per Crash│ Deadliest Yr │  ← KPI cards
├─────────────┴─────────────┴─────────────┴─────────────┤
│  Line chart: Crashes & Fatalities by Year (1908-2024)  │  ← dual-axis line
├─────────────────────────────┬───────────────────────────┤
│ Bar: Fatalities by Decade   │ Bar: Crashes by Decade     │
└─────────────────────────────┴───────────────────────────┘
```
- Line chart: X = Year, Y1 = Total Crashes (count), Y2 = Total Fatalities — this carries the
  main story (peak deaths in the 1970s, steep decline since, despite far more flights today).
- Add a decade slicer.

### Page 2: Manufacturer & Aircraft
```
┌───────────────────────────┬─────────────────────────────┐
│ Bar: Fatalities by        │ Bar: Crash count by         │
│ Manufacturer (top 10)     │ Manufacturer (top 10)       │
├───────────────────────────┴─────────────────────────────┤
│ Table: Top 15 deadliest single crashes                   │
│ (Year, Aircraft, Fatalities, Aboard, FatalityRate)       │
└───────────────────────────────────────────────────────────┘
```
- Use two side-by-side bars (not one combo) since "most fatalities" and "most crashes" tell
  different stories — Boeing/Douglas lead partly because they built the most planes historically.
- Add a manufacturer slicer that cross-filters both.

### Page 3: Seasonality & Severity
```
┌─────────────────────────────┬───────────────────────────┐
│ Column: Crashes by Quarter  │ Column: Crashes by Month   │
├─────────────────────────────┴───────────────────────────┤
│ Scatter: Aboard (x) vs Fatalities (y), sized by Ground   │
│ — shows which crashes were most/least survivable         │
└─────────────────────────────────────────────────────────┘
```

## Step 5 — Formatting
- Theme: dark or muted background works well for this subject — avoid a cheerful palette.
- Use a single accent color for "fatalities" visuals and a neutral gray for "crash count" so the
  eye separates severity from frequency at a glance.
- Add a Year range slider at the top of every page (sync slicers across pages: View → Sync Slicers).

## Step 6 — Optional next step
If you want the Operator/Country columns fixed for a real map/leaderboard, let me know — I can
write a script to attempt reordering the scrambled tokens against a reference list of known
airline and country names, though it won't be 100% accurate given how mangled the text is.
