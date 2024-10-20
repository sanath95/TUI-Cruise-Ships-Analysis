# TUI Cruise Ships Analysis

This project analyzes the provided dataset for two cruise ships and develops a narrative explaining performance trends.

The [data](./data/data.csv) used for this project can be found here.

For further details about the dataset, refer to the [Data Schema](./data/schema.pdf).

---

# Vessel 1

## Data Overview

- **Total Rows:** 105,120  
  - Each measurement is taken at 5-minute intervals.
  - There are 541 missing (NA) values.

- **Total Columns:** 44  
  - Numeric: 41  
  - Categorical: 1  
  - Time: 2  

---

## Vessel 1 Route

![Vessel 1 Route](./assets/vessel1_route.png)

---

## Exploratory Data Analysis

### Feature Selection
1. **Dropped Columns:**
   - `Vessel Name` and `Bow Thruster 1 Power (MW)` had only one unique value, so they were removed.
   - `Depth (m)` had 27,756 missing values (26.4% of the data), so it was dropped.
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
1. For columns with only one missing value, the gaps were filled using the mean of the values before and after.
2. Other columns (with up to 171 missing values) were backfilled (future scope: regression techniques could be applied to fill these missing values).

---

### Correlation Analysis

![Correlation Matrix](./assets/correlation_matrix.jpg)

| Feature 1                          | Feature 2                        | Pearson Correlation |
|-----------------------------------|----------------------------------|---------------------|
| Speed Through Water (knots)       | Speed Over Ground (knots)        | 0.99                |
| Relative Wind Angle (Degrees)     | True Wind Angle (Degrees)        | 0.94                |
| Relative Wind Speed (knots)       | True Wind Speed (knots)          | 0.77                |
| Relative Wind Direction (Degrees) | True Wind Direction (Degrees)    | 0.66                |

---

### Inferences

1. **Diesel Generator Power (MW) vs. Propulsion Power (MW) (1.00 correlation):** The high positive correlation suggests that the diesel generator is primarily dedicated to supporting propulsion power demands, with non-propulsion electrical loads contributing only a small portion to total power consumption.
2. **Propulsion Power (MW) vs. Speed Through Water (knots) (0.91 correlation):** This strong correlation implies that propulsion power consumption can be accurately estimated based on the vessel's speed through the water.
3. **HVAC Chiller Power (MW) vs. Sea Temperature (°C) (0.91 correlation):** The high positive correlation indicates a consistent pattern where the chiller workload increases with rising temperatures due to greater cooling requirements.
4. **Scrubber Power (MW) vs. Propulsion Power (MW) (0.85 correlation):** This suggests that optimizing propulsion efficiency could also reduce the scrubber system's power consumption.
5. **Diesel Generator Power (MW) vs. Main Engine Fuel Flow Rate (kg/h) (0.78 correlation):** The correlation highlights the interdependence between mechanical and electrical power needs, suggesting opportunities for optimizing overall energy consumption.
6. **Diesel Generator Power (MW) vs. Boiler Fuel Flow Rate (L/h) (-0.66 correlation):** The negative correlation indicates a trade-off or complementary usage between the Boiler and the Diesel Generator in meeting the ship's energy demands.
7. **Trim (m) vs. Speed Through Water (knots) (-0.53 correlation):** This suggests that optimizing trim could help improve speed through the water and enhance fuel efficiency.
8. **Speed Through Water (knots) vs. Speed Over Ground (knots) (0.99 correlation):** The high correlation implies minimal or stable water currents or tidal effects affecting the vessel.
9. **Relative Wind Angle (Degrees) vs. True Wind Angle (Degrees) (0.94 correlation):** This suggests consistent alignment between the vessel's speed and course relative to wind direction.
10. **Relative Wind Speed (knots) vs. True Wind Speed (knots) (weak correlations 0.77) and Relative Wind Direction (Degrees) vs. True Wind Direction (Degrees) (weak correlation 0.66)** Indicates variability in the vessel's speed or course, affecting how the wind is experienced onboard.

> These inferences are based on linear correlations only and do not imply causation.

---

### Conclusions

1. **Efficiency Optimization:** Efforts to reduce speed or optimize routes can significantly impact overall power generation and fuel consumption, leading to potential savings.
2. **Performance Consistency:** There are no significant variations in performance attributable to fouling or mechanical issues.
3. **Energy Use Based on Temperature:** There is an opportunity to optimize energy consumption by using sea temperature forecasts to anticipate HVAC demand.
4. **Fuel Efficiency and Environmental Compliance:** Reducing propulsion power can lower both fuel consumption and the demand for exhaust treatment.
5. **Factors Influencing Generator Output:** The Diesel Generator's power output is influenced by more than just the Main Engine's fuel flow, including non-propulsion electrical loads (e.g., HVAC), sea conditions, or operational practices.
6. **Load Shifting Strategy:** Implementing load-shifting strategies where the Boiler and Diesel Generator usage are adjusted based on demand could improve energy efficiency.
7. **Trim Optimization:** Lower trim values could reduce hydrodynamic resistance, allowing for higher speeds at the same propulsion power. Proper cargo balancing and ballast adjustments could maximize speed and minimize fuel consumption.

---