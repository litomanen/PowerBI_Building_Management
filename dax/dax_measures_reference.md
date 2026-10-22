# DAX Measures Reference — Infrastructure Management Analytics

This document provides a technical catalog of the Data Analysis Expressions (DAX) measures developed for the **PowerBI_Building_Management** analytics engine.

---

## 1. Portfolio Overview & Asset Metrics

### Total Building Count (`Qtd Edifícios`)
Calculates the total number of building records within the active filter context.

```dax
Qtd Edifícios = COUNTROWS('building_data')
```

* **Logic**: Uses `COUNTROWS` on the primary dimension table `building_data` for optimal engine performance.
* **Business Value**: Provides baseline volume metrics across regions, portfolio segments, and property types.

---

## 2. Energy Efficiency & Sustainability (ESG)

### Inefficient Buildings Ratio (`% Edificios Ineficientes (D-E)`)
Calculates the percentage of unique buildings operating with sub-optimal energy efficiency ratings (`D` and `E`).

```dax
% Edificios Ineficientes (D-E) = 
VAR Total = DISTINCTCOUNT(building_data[building_id])
VAR Poluidores = 
    CALCULATE(
        DISTINCTCOUNT(building_data[building_id]), 
        building_data[energy_rating] IN {"D", "E"}
    )
RETURN 
    DIVIDE(Poluidores, Total, 0)
```

* **Logic**: Counts distinct buildings filtered by rating `D` or `E` against the total distinct building count under the current filter context.
* **Business Value**: Highlights portfolio ESG compliance exposure and pinpoints priority targets for energy retrofits.

---

### Buildings Share by Energy Category (`% Edifícios por Categoria`)
Calculates the proportion of buildings in a specific energy rating category relative to the entire portfolio.

```dax
% Edifícios por Categoria = 
DIVIDE(
    DISTINCTCOUNT(building_data[building_id]), 
    CALCULATE(
        DISTINCTCOUNT(building_data[building_id]), 
        ALL(building_data[energy_rating])
    )
)
```

* **Logic**: Removes filters on `energy_rating` in the denominator using `ALL()`, allowing percentage-of-total distribution analysis.
* **Business Value**: Facilitates benchmark comparative charts (e.g., stacked bar charts) across rating tiers.

---

### Carbon Intensity per Square Meter (`CO2 por m2`)
Measures environmental footprint intensity by computing annual CO2 emissions per unit of surface area.

```dax
CO2 por m2 = 
DIVIDE(
    SUM(building_data[co2_kg]), 
    SUM(building_data[area_sqm]), 
    0
)
```

* **Logic**: Normalizes raw carbon output (`co2_kg`) against structural size (`area_sqm`) using safe division.
* **Business Value**: Allows fair sustainability comparisons between small facilities and large complex properties.

---

### Energy Consumption Variance vs Global Average (`Diferença Consumo %`)
Evaluates how the selected subset's average energy consumption compares against the global portfolio benchmark.

```dax
Diferença Consumo % = 
VAR MediaGlobal = CALCULATE(AVERAGE(building_data[energy_consumption_kwh]), ALL(building_data))
VAR MediaSelecao = AVERAGE(building_data[energy_consumption_kwh])
RETURN
    DIVIDE(MediaSelecao - MediaGlobal, MediaGlobal, 0)
```

* **Logic**: Computes the relative percentage deviation of the current selection's average kWh against the unfiltered portfolio average (`ALL(building_data)`).
* **Business Value**: Instantly flags regional or structural outliers consuming significantly above portfolio baseline.

---

## 3. Maintenance, Risk & Asset Life Cycle

### Pre-2000 Renovation Need Ratio (`% Renovação Pré-2000`)
Identifies aging infrastructure built prior to the year 2000 that requires structural or operational renovation.

```dax
% Renovação Pré-2000 = 
VAR TotalEdificiosPre2000 = 
    CALCULATE(
        DISTINCTCOUNT(building_data[building_id]), 
        building_data[year_built] < 2000
    )
VAR EdificiosPre2000ComRenovacao = 
    CALCULATE(
        DISTINCTCOUNT(building_data[building_id]), 
        building_data[year_built] < 2000, 
        building_data[renovation_needed] = "Yes"
    )
RETURN 
    DIVIDE(EdificiosPre2000ComRenovacao, TotalEdificiosPre2000, 0)
```

* **Logic**: Filters older properties (`year_built < 2000`) and determines the proportion flagged with `renovation_needed = "Yes"`.
* **Business Value**: Supports capital expenditure (CapEx) planning and long-term asset renewal budgeting.

---

### Incident Rate per Building (`Taxa Incidencia`)
Calculates the average number of reported incidents over the last 5 years per building.

```dax
Taxa Incidencia = 
DIVIDE(
    SUM(building_data[incidents_last_5y]), 
    DISTINCTCOUNT(building_data[building_id]), 
    0
)
```

* **Logic**: Sums total 5-year incident logs and divides by unique property count.
* **Business Value**: Quantifies operational risk exposure for insurance evaluation and preventive maintenance planning.