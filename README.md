# ⚡ EV Charging Station Analytics

An interactive 5-page **Power BI** dashboard that analyzes EV charging sessions across 20 stations: demand, queues, pricing, renewable energy share and vehicle charging behavior. It ends with a page of data-backed insights and recommendations.

---

## 📌 Project Overview

| Item | Detail |
|---|---|
| **Tool** | Microsoft Power BI Desktop (DAX, data modeling) |
| **Data** | 8,354 charging sessions, 1 Jan 2025 – 29 Mar 2025 |
| **Scope** | 20 stations, 10 chargers, 3 vehicle types (Car, Bus, Two-Wheeler) |
| **Goal** | Understand when and why charging demand peaks, and how to ease it |

---

## 🗂️ Repository Structure

```
EV-Charging-Station-Analytics/
│
├── README.md
├── EV_Charging_Analytics.pbix
├── dataset/
│   └── ev_charging_dataset.csv
└── screenshots/
    ├── executive_overview.png
    ├── period_performance.png
    ├── station_operations.png
    ├── vehicle_battery_behavior.png
    └── key_insights.png
```

---

## 📊 Dashboard Pages

### 1. Executive Overview
High-level KPIs: total charging sessions, total energy consumed, session trend, average station load, average renewable energy share, waiting time and charging duration.

([Executive_Overview.png](https://github.com/dheanarasan22/EV-Charging-Station-Analytics/blob/main/01_executive_overview.png))

### 2. Period & Performance Comparison
Week-over-week session comparison, average charging duration and waiting time by day of week, and average charging demand by time slot (Peak vs Off-Peak).

([period_performance.png](https://github.com/dheanarasan22/EV-Charging-Station-Analytics/blob/main/02_period_performance.png))

### 3. Station Operations & Queue Analysis
Queue length by station, sessions by charging priority, plus optimization reward by traffic density, charging power by vehicle type and electricity price by time slot.

([Station_Operations.png](https://github.com/dheanarasan22/EV-Charging-Station-Analytics/blob/main/03_station_operations.png))

### 4. Vehicle & Battery Charging Behavior
State of charge (SOC) at arrival and departure, SOC gained, battery capacity and energy per session, including how arrival SOC bands drive energy use and charging time.

([vehicle_battery_behavior.png](https://github.com/dheanarasan22/EV-Charging-Station-Analytics/blob/main/04_vehicle_battery_behavior.png))

### 5. Key Insights & Recommendations
Live, measure-driven insight cards comparing Peak and Off-Peak, plus recommended actions.

([key_insights.png](https://github.com/dheanarasan22/EV-Charging-Station-Analytics/blob/main/05_key_insights.png))

---

## 🔍 Key Findings

1. **Peak hours are the pressure point.** Peak carries about 37% of sessions but runs stations at about **84% load** vs about 35% off-peak, roughly **2.4x the demand**.
2. **Price does not shift demand.** Average electricity price is nearly identical in peak (9.94) and off-peak (9.97).
3. **Peak charging is less green.** Renewable share is about **29% in peak** vs about 39% off-peak.
4. **Optimization reward drops in peak.** The average reward is about -19.8 in peak vs about -7.1 off-peak.
5. **Low arrival SOC means longer, heavier sessions.** Vehicles arriving at 10–20% SOC draw about 52 kWh per session vs about 27 kWh at 50–60%.
6. **Site and weather matter little.** Station load stays within about 51–56%, and location type and weather barely change the results. Time of day is the main lever.

---

## 💡 Recommendations

- Introduce **time-of-use pricing** so peak hours cost more.
- **Shift flexible charging** to off-peak hours, when renewable share is higher.
- **Review scheduling logic** for the peak window, where optimization reward is weakest.
- Add **capacity or smart queuing** to handle peak demand.

---

## 🧮 Data Model

| Table | Purpose |
|---|---|
| `ev_charging_dataset` | Fact table with one row per charging session |
| `DateTable` | Date dimension for time intelligence |
| `measure_heading` | Holds all DAX measures and page titles |

**Notable modeling choices**
- `Sessions SPLY` compares against the **previous 7 days** (`DATEADD(..., -7, DAY)`), because the data covers under three months and a year-over-year comparison would be blank.
- `day_of_week` is sorted Monday → Sunday using a `WEEKDAY`-based sort column.
- `Initial SOC Band` groups arrival SOC into 10-point bands, with a hidden sort column.
- Chart measures such as `Avg Charging Duration (min)` are **numeric**. The text versions used in cards are kept separate.

### Example DAX

```DAX
Sessions SPLY =
CALCULATE ( [total_charging_session], DATEADD ( 'DateTable'[Date], -7, DAY ) )

Peak Demand Multiple =
DIVIDE (
    CALCULATE ( [Average Charging Demand], 'ev_charging_dataset'[time_slot] = "Peak" ),
    CALCULATE ( [Average Charging Demand], 'ev_charging_dataset'[time_slot] = "Off-Peak" )
)

Avg SOC Gained =
AVERAGEX (
    'ev_charging_dataset',
    'ev_charging_dataset'[final_soc] - 'ev_charging_dataset'[initial_soc]
)
```

---

## 📁 Dataset

`dataset/ev_charging_dataset.csv` contains one row per charging session, with fields covering:

- **Time:** charging date, day of week, time slot (Peak / Off-Peak)
- **Vehicle and battery:** vehicle type, battery capacity (kWh), initial and final SOC
- **Station and operations:** station ID, assigned charger, location type, queue length, charging priority
- **Performance:** energy consumed (kWh), charging duration, waiting time, charging power (kW), charging demand
- **Context:** weather condition, traffic density, electricity price, renewable energy ratio, optimization reward

---

## 🚀 How to Use

1. Clone or download this repository.
2. Open `EV_Charging_Analytics.pbix` in **Power BI Desktop**.
3. If prompted, point the data source to `dataset/ev_charging_dataset.csv` (**Home → Transform data → Data source settings**).
4. Use the slicers on each page (date, vehicle type, location type, weather) to explore.

---

## 🛠️ Skills Demonstrated

Power BI report design · DAX measures · data modeling · calculated columns and sort-by-column · time intelligence · KPI design · data storytelling

---

## 👤 Author

**SALAI DHEAN ARASAN S**
[LinkedIn](https://www.linkedin.com/) · [GitHub](https://github.com/)
