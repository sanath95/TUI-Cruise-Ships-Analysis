# TUI Cruise Ships Analysis

This project analyzes the provided dataset for two cruise ships and develops a narrative explaining performance trends.

The [data](./data/data.csv) used for this project can be found here.

For further details about the dataset, refer to the [Data Schema](./data/schema.pdf).

---

# Vessel 1

## Data Overview

- **Total Rows:** 105,120  
  - Each measurement is taken at 5-minute intervals.
  - There are 541 missing values in vessel 1 data (excluding Depth (m) column).
  - There are 4786 missing values in vessel 2 data (excluding Depth (m) column).

- **Total Columns:** 44  
  - Numeric: 41  
  - Categorical: 1  
  - Time: 2  

---

## Route

### Vessel 1

![vessel 1 route](./assets/vessel1_route.png)

### Vessel 2

![vessel 2 route](./assets/vessel2_route.png)
---

## Exploratory Data Analysis

### Feature Selection
1. **Dropped Columns:**
   - `Vessel Name` and `Bow Thruster 1 Power (MW)` had only one unique value, so they were removed.
   - `Depth (m)` had 27,756 missing values (26.4% of the data) in vessel 1 and 29,738 missing values (28.28% of the data) in vessel 2, so it was dropped.
   - `Propulsion Power (MW)` is the sum of `Port Side Propulsion Power (MW)` and `Starboard Side Propulsion Power (MW)`, so the individual propulsion columns were dropped.
   - `Start Time`, `End Time`, and `Local Time (h)` were dropped because measurements occur at consistent 5-minute intervals.
   - `Longitude (Degrees)` and `Latitude (Degrees)` were excluded from the analysis.

2. **Speed Features:**
   - `Speed Through Water (knots)` was retained because it directly relates to propulsion efficiency and helps isolate the effects of water conditions. `Speed Over Ground (knots)` was dropped.

3. **Wind Features:**
   - `Relative Wind Angle (Degrees)`, `Relative Wind Speed (knots)`, and `Relative Wind Direction (Degrees)` were kept as they reflect how wind forces interact with the vessel, influencing aerodynamic drag and resistance. The corresponding true wind measurements (`True Wind Angle`, `True Wind Speed`, and `True Wind Direction`) were dropped.

4. **Aggregated Features:**
   - The values for the following components were aggregated:
     - Power Galley (MW)
     - HVAC Chiller Power (MW)
     - Boiler Fuel Flow Rate (L/h)
     - Diesel Generator Power (MW)
     - Bow Thruster Power (MW)
     - Stern Thruster Power (MW)
     - Main Engine Fuel Flow Rate (kg/h)

---

### Handling Missing Values

**Vessel 1:**

1. For columns with only one missing value, the gaps were filled using the mean of the values before and after.
2. Other columns (with up to 171 missing values) were backfilled (future scope: regression techniques could be applied to fill these missing values).

**Vessel 2:**

1. All columns (with up to 972 missing values) were backfilled (future scope: regression techniques could be applied to fill these missing values).

---

### Correlation Analysis

**Vessel 1**

![vessel 1 correlation matrix](./assets/vessel1_correlation_matrix.png)

**Vessel 2**

![vessel 2 correlation matrix](./assets/vessel2_correlation_matrix.png)

---

### Observations

| Feature 1                           | Feature 2                         | Vessel 1 Correlation | Vessel 2 Correlation |
| ----------------------------------- | --------------------------------- | -------------------- | -------------------- |
| Diesel Generator Power (MW)         | Propulsion Power (MW)             | 1.00                 | 0.99                 |
| Diesel Generator Power (MW)         | Main Engine Fuel Flow Rate (kg/h) | 0.99                 | 1.00                 |
| Speed Through Water (knots)         | Propulsion Power (MW)             | 0.91                 | 0.90                 |
| HVAC Chiller Power (MW)             | Sea Temperature (Celsius)         | 0.91                 | 0.89                 |
| Main Engine Fuel Flow Rate (kg/h)   | Boiler Fuel Flow Rate (L/h)       | -0.66                | -0.67                |
| Diesel Generator Power (MW)         | Scrubber Power (MW)               | 0.86                 | 0.49                 |
| Speed Through Water (knots)         | Trim (m)                          | -0.53                | -0.19                |

---

### Conclusions

1. The vessels show high operational efficiency by aligning power generation, propulsion, and fuel usage.
2. Both vessels efficiently convert propulsion power into forward motion, reflecting good propulsion system performance.
3. Environmental factors like sea temperature significantly impact energy consumption, particularly in auxiliary systems like HVAC.
4. The trade-off between main engine and boiler fuel usage indicates efforts to minimize overall fuel consumption, possibly through energy recovery techniques.
5. Variability in scrubber usage and trim adjustments between the vessels may reflect different operational practices, vessel designs, or regulatory requirements.

> These inferences are based on linear correlations only and do not imply causation.

---