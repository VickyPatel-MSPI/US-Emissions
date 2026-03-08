# United States GHG Emissions Breakdown Dashboard

[![Databricks](https://img.shields.io/badge/Databricks-SQL%20%26%20Spark-FF3621?style=flat&logo=databricks&logoColor=white)](https://databricks.com/)


Interactive dashboard and SQL-based analysis exploring **county-level greenhouse gas (GHG) emissions** across the United States — identifying emission hotspots, top contributors, per-capita intensity, and geographic patterns.

## 🎯 Project Goals

Understand **who emits what and where** to support data-driven climate policy and sustainability decisions.

- Identify highest-emitting **counties** and **states**
- Calculate **emissions per person** (intensity)
- Visualize **geographic distribution** of emissions
- Analyze **population vs. emissions** relationship
- Highlight concentration — how few states/counties drive most emissions


## 🖼️ Dashboard Highlights

- 🗺️ **Interactive Emission Map** (county-level, powered by Mapbox)
- 📊 **Emission vs Population** scatter plot (per person metric)
- 🏆 **Top 10 Counties** by total emissions
- 🇺🇸 **Top 10 States** contribution pie/bar chart (% share)

*(Add screenshot(s) here — recommended: full dashboard view + zoomed map + top-N visuals)*

<!-- You can upload images to the repo and link them like this: -->
<!-- ![Dashboard Overview](assets/dashboard-full.png) -->
<!-- ![Emission Map Detail](assets/emission-map.png) -->

## 🛠️ Tech Stack

| Area                | Technology/Tools                     |
|---------------------|---------------------------------------|
| Data Processing     | Databricks SQL, Apache Spark SQL     |
| Query & Analysis    | SQL                                   |
| Visualization       |  Databricks Dashboard                 |
| Geospatial          | Mapbox                                |
| Data Prep/Exploration | Excel                               |
| Data Source         | EPA GHG Inventory (county-level, 2023) |

## 📊 Key Datasets

**Table**: `emissions.default.emissions_data`

| Column                     | Description                                      | Type     |
|----------------------------|--------------------------------------------------|----------|
| `county_state_name`        | County and state name                            | STRING   |
| `state_abbr`               | State abbreviation                               | STRING   |
| `latitude`                 | Latitude                                         | DOUBLE   |
| `longitude`                | Longitude                                        | DOUBLE   |
| `population`               | County population                                | BIGINT   |
| `GHG emissions mtons CO2e` | Total GHG emissions (million metric tons CO₂e)   | STRING*  |

\* Note: emission values stored as string with commas → cleaned in queries

## 📈 Important SQL Analyses

### 1. Geographic Map Data

```sql
SELECT 
    latitude,
    longitude,
    CAST(REPLACE(`GHG emissions mtons CO2e`, ',', '') AS DOUBLE) AS Emissions
FROM emissions.default.emissions_data
```

---

**2. Emissions Per Person (Intensity)**
```sql
SELECT 
    county_state_name,
    population,
    CAST(REPLACE(`GHG emissions mtons CO2e`, ',', '') AS DOUBLE) / 
    CAST(population AS DOUBLE) AS Emissions_per_person
FROM emissions_data
WHERE population > 0
ORDER BY Emissions_per_person DESC
```

---

**3. Total Emissions by State (Top 10)**
```sql
SELECT 
    state_abbr,
    ROUND(SUM(CAST(REPLACE(`GHG emissions mtons CO2e`, ',', '') AS DOUBLE)), 2) AS Total_Emission
FROM emissions_data
GROUP BY state_abbr
ORDER BY Total_Emission DESC
LIMIT 10
```

---

**4. Top 10 Counties by Absolute Emissions**
```sql
SELECT 
    county_state_name,
    population,
    CAST(REPLACE(`GHG emissions mtons CO2e`, ',', '') AS DOUBLE) AS Total_Emission
FROM emissions_data
ORDER BY Total_Emission DESC
LIMIT 10
```



**Conclusion 🎯**

---

This project demonstrates how data analytics and visualization can help interpret environmental impact data at scale.

Key insights include:

* A small number of states contribute over 50% of total emissions

* High population does not always mean higher emission per person

* Certain counties act as major emission hotspots

* Geographic visualizations help quickly identify regional emission clusters

The analysis highlights the importance of data-driven environmental monitoring, enabling policymakers and organizations to focus sustainability efforts where they are most needed.

---

**Contributions**

Explore the Databricks dashboard for interactive visualizations : [United States Emission]()
