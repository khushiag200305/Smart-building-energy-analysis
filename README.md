# Energy Consumption Analysis in Smart Buildings

Unsupervised analysis of hourly energy data from **1,449 buildings (~20M meter readings)** to identify consumption patterns and group buildings by behavior, with recommendations for energy optimization.

**Tech:** Python · Pandas · NumPy · Scikit-Learn · Matplotlib · Seaborn

## Dataset
[ASHRAE – Great Energy Predictor III](https://www.kaggle.com/competitions/ashrae-energy-prediction) (Kaggle): hourly meter readings, building metadata, and site-level weather data.
The data is not included in this repo. Download it from Kaggle and place the CSV files in the same folder as the notebook.

## Approach
1. **Data integration:** merged meter readings with building metadata and weather data; interpolated weather gaps per site.
2. **Feature engineering:** built 10 behavioral features per building: mean/peak/std load, load factor, day/night ratio, weekend/weekday ratio, and more. Applied a log transform to correct heavy skew.
3. **Weather analysis:** measured each building's temperature sensitivity from daily load vs. temperature correlation.
4. **PCA:** reduced 10 features to 3 components retaining **96% of the variance**.
5. **K-Means clustering:** chose k = 4 using the elbow method and silhouette score (0.34).
6. **Cluster profiling:** compared clusters on load shape, schedule dependence, size, and weather sensitivity.

## Key Findings
- Weekend consumption is **~17% lower** than weekday across all buildings.
- **53% of buildings are strongly temperature-dependent:** 532 cooling-driven and 226 heating-driven.

| Cluster | Buildings | Profile | Optimization opportunity |
|---|---|---|---|
| 0 | 345 | Largest, peaky (load factor 0.16), most weather-sensitive | Peak shaving, weather-based HVAC control |
| 1 | 640 | Mid-size, cooling-dominant | Pre-cooling, cooling efficiency upgrades |
| 2 | 316 | Small, flat base load (load factor 0.37) | Equipment efficiency, base-load reduction |
| 3 | 148 | Schedule-driven: day load 2.35× night, ~48% weekend drop | Night/weekend HVAC and lighting setback |

## Limitations
- All meter types (electricity, chilled water, steam, hot water) are combined into one reading.
- A silhouette score of 0.34 indicates moderate overlap between clusters.

## How to Run
pip install -r requirements.txt
jupyter notebook energy_consumption_analysis.ipynb

## View the notebook with outputs on Kaggle: https://www.kaggle.com/code/khushiag200305/notebooke633265425
