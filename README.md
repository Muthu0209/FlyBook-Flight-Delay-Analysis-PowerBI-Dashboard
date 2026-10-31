# FlyBook-Flight-Delay-Analysis-PowerBI-Dashboard
An interactive Power BI dashboard analyzing 2023 US flight delays, cancellations, and diversions to identify the main causes of delays and help FlyBook users make better-informed travel decisions.

##  Business Context

FlyBook is an online flight booking platform. Its Chief Product Officer is concerned about rising flight delays and their effect on customer experience and booking decisions. As the Senior Data Analyst, the goal is to:

- Identify the **primary causes** of flight delays
- Find which **airlines, airports, aircraft manufacturers, days, and months** perform worst
- Provide **reliability insights** that FlyBook can surface to travelers



## Dashboard Preview

### Page 1: Delay Overview (KPIs, Airline Performance & Trends)
![Delay Overview](screenshots/01-delay-overview-kpis-trends.jpg)

### Page 2: Delay Insights (Airports, Manufacturers & Map)
![Delay Insights](screenshots/02-delay-insights-airports-manufacturer-map.jpg)


## 🗂️ Data Model

| Table | Description |
|---|---|
| `US_flights_2023` | Flight-level data: delays, airline, aircraft, airports |
| `Cancelled_Diverted_2023` | Cancelled and diverted flight records |
| `airports_geolocation` | Airport names, cities, coordinates (IATA code) |
| `weather_meteo_by_airport` | Daily weather observations per airport |
| `DateTable` | Calendar table built with DAX |
| `DelayTypeTable` | Helper table for delay cause breakdown |

**Relationships**
- `US_flights_2023[Dep_Airport]` → `airports_geolocation[IATA_CODE]`
- `weather_meteo_by_airport[airport_id]` → `airports_geolocation[IATA_CODE]`
- `DateTable[Date]` → `US_flights_2023[FlightDate]`
- `DateTable[Date]` → `Cancelled_Diverted_2023[FlightDate]`

---

##  Key DAX Measures

```DAX
DateTable = CALENDAR(MIN(Cancelled_Diverted_2023[FlightDate]), MAX(US_flights_2023[FlightDate]))

Total delay = SUM(US_flights_2023[Dep_Delay])
Total Diverted = SUM(Cancelled_Diverted_2023[Diverted])
Total_Delay_Carrier = SUM(US_flights_2023[Delay_Carrier])
Total_Delay_Weather = SUM(US_flights_2023[Delay_Weather])
Total_Delay_NAS = SUM(US_flights_2023[Delay_NAS])
Total_Delay_Security = SUM(US_flights_2023[Delay_Security])
Total_Delay_LastAircraft = SUM(US_flights_2023[Delay_LastAircraft])

Avg_Delay_By_Day = AVERAGE(US_flights_2023[Dep_Delay])

DelayTypeTable =
UNION(
    ROW("DelayType", "Carrier", "TotalDelay", [Total_Delay_Carrier]),
    ROW("DelayType", "Weather", "TotalDelay", [Total_Delay_Weather]),
    ROW("DelayType", "NAS", "TotalDelay", [Total_Delay_NAS]),
    ROW("DelayType", "Security", "TotalDelay", [Total_Delay_Security]),
    ROW("DelayType", "LastAircraft", "TotalDelay", [Total_Delay_LastAircraft])
)
```

Also included: month-over-month **% change** measures for delays, cancellations, diversions, and flight count (using `DATEADD` with a divide-by-zero guard), plus arrow and color-coding measures for KPI cards.

---

##  Key Insights

- **Airline performance:** JetBlue, Frontier, and Spirit show the highest average departure delays.
- **Aircraft manufacturer:** Diamond Aircraft has the highest average departure delay, well above Airbus, Boeing, Canadair, and Embraer.
- **Day of week:** Sunday (7) has the highest average delay; Tuesday (2) the lowest.
- **Seasonality:** Delays peak in the summer (July) and are lowest in the autumn (November).
- **Worst airports:** SMX, COD, PPG, HTS, and USA for departures; PVU, SMX, PSE, BQN, and SFB for arrivals.
- **Delay causes:** Carrier and NAS delays contribute more to average delay than weather and security.

---

## Tools & Skills

- **Power BI Desktop**: data modeling, visuals, slicers, edit interactions
- **DAX**: calculated tables, time-intelligence and KPI measures
- **Power Query**: data loading and transformation
- Data visualization and dashboard design

---

##  Repository Structure

```
FlyBook-Flight-Delay-Analysis-PowerBI/
├── FlyBook-Flight-Delay-Analysis-PowerBI.pbix
├── screenshots/
│   ├── 01-delay-overview-kpis-trends.jpg
│   └── 02-delay-insights-airports-manufacturer-map.jpg
└── README.md
```

---

## How to Use

1. Download the `.pbix` file.
2. Open it in **Power BI Desktop** (free from Microsoft).
3. Use the month and airline slicers to explore the data.
4. If prompted, point the data sources to your local copies of the CSV files.


⭐ If you found this project useful, please star the repo!ttps://github.com/Muthu0209
