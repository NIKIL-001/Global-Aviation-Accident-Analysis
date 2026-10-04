# DAX Measures — Global Aviation Accident Analysis

Table name used below: `Air_Crash_Data` (replace with your actual table name if different).

## Core totals

```DAX
Total_Crashes = COUNTROWS(Air_Crash_Data)
```

```DAX
Total_Onboard = SUM(Air_Crash_Data[TotalPeople])
```

```DAX
Total_Survivors = SUM(Air_Crash_Data[Survivors])
```

```DAX
Total_Fatality = SUM(Air_Crash_Data[TotalFatalities])
```

## Rates

```DAX
Avg_Survival_Rate = AVERAGE(Air_Crash_Data[SurvivalRate])
```

```DAX
Avg_Fatality_Rate = AVERAGE(Air_Crash_Data[FatalityRate])
```

```DAX
Fatalities_per_Crash = DIVIDE([Total_Fatality], [Total_Crashes])
```

## Extremes

```DAX
Darkest_Year = 
CALCULATE(
    MAX(Air_Crash_Data[Year]),
    FILTER(
        Air_Crash_Data,
        Air_Crash_Data[TotalFatalities] = MAX(Air_Crash_Data[TotalFatalities])
    )
)
```

```DAX
Darkest_Location = 
CALCULATE(
    SELECTEDVALUE(Air_Crash_Data[Location]),
    FILTER(
        Air_Crash_Data,
        Air_Crash_Data[TotalFatalities] = MAX(Air_Crash_Data[TotalFatalities])
    )
)
```


## Notes
- `Darkest_Year` / `Darkest_Location` return the year/location of the single highest-fatality
  crash record. If multiple rows share the max value, these may need a tie-break (e.g. wrap in
  `TOPN(1, ..., ORDER BY Year ASC)`).
- Apply **Format visual → Callout value → Display units → None** on any card showing Year, so
  it doesn't get abbreviated (e.g. "2K" instead of "2001").
- If `Avg_Fatality_Rate` or `Avg_Survival_Rate` shows NaN/blank, check for `TotalPeople = 0` rows
  causing division errors in the source column — guard with an `IF` in Power Query:
  `= if [TotalPeople] = 0 then 0 else [TotalFatalities] / [TotalPeople] * 100`
