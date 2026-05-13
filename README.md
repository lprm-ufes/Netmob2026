<!-- ![NetMob 2026 Logo](images/logo_2026.png) -->
<img src="image/logo_2026.png" alt="NetMob 2026 Logo" width="400" />


# NetMob 2026: Public Transport & Passenger Demand in Niterói, RJ, Brazil

Welcome to the official repository of the **NetMob 2026 Data Challenge**, organized in collaboration with UFF, UFES, PUC Minas and research partners from across Brazil. This repository contains the dataset for the **NetMob 2026 Challenge**, focusing on public transport mobility and passenger demand in the city of Niterói, Rio de Janeiro, Brazil.

Researchers are invited to explore the dataset and contribute original findings on topics such as real time vehicle tracking, bus stop arriving prediction, social-aware trip analysis, and more.


## Overview

The dataset provides a comprehensive view of the urban mobility ecosystem in Niterói, combining three main sources of information:

1.  **Mobility Data:** Real-time GPS telemetry from the public bus fleet.
2.  **Ticket Data:** Passenger transaction records (boardings) across the bus system.
3.  **Meteorological Data:**  - Auxiliar data -  Weather conditions in the city during the collection period.
4.  **Social and economic Data:** -  Auxiliar data - Official data containing economic and social information about the city. 

**Collection Period:** March 2026.

---

## Dataset Structure

> Interested researchers must request the dataset.
### 1. Mobility Data (GPS Telemetry)
Contains high-frequency positional data for buses operating in Niterói.
- **Location:** `mobility_data/` 
- **Details:** See [**README_Mobility.md**](README_Mobility.md) for schema and spatial coverage.

### 2. Ticket Data (Passenger Transactions)
Contains logs of every passenger boarding, including fare types and card categories.
- **Location:** `ticket_data/`
- **Details:** See [**README_Ticket.md**](README_Ticket.md) for transaction types and categorical mappings.

### 3. Environment Data 
Static reference files and environmental data located in `auxiliar_data/`.

| File | Format | Description |
| :--- | :--- | :--- |
| `line_routes.json` | GeoJSON | Complete geometric shapes of all bus routes. |
| `stops.json` | GeoJSON | Official bus stops with street-level location. |
| `stops_integration_city.json` | JSON | Real-time snapshots near city integration terminals. |
| `stops_integration_metropolitan.json` | GeoJSON | Locations of major metropolitan interchange hubs. |
| `meteorological_data_cleaned.csv` | CSV | Hourly weather data (Temp, Rain, Wind) for March 2026. |

---

## Getting Started: Notebooks

We provide six reference notebooks to help participants explore the data:

- [**Bus Mobility Data Characterization**](Notebooks/bus_mobility_data_characterization.ipynb): Exploration of GPS traces and vehicle patterns.
- [**Ticket Transactions Analysis**](Notebooks/Ticket_Transactions.ipynb): Analysis of passenger demand and fare patterns.
- [**Meteorological Analysis**](Notebooks/meteorological-analysis.ipynb): Investigation of weather patterns during the study period.
- [**Demand Mapping**](Notebooks/Cross_Dataset_Integration_Demand_Mapping.ipynb): Overlays ticket boarding counts onto GeoJSON route geometries to identify high-demand corridors.
- [**Headway & Frequency Analysis**](Notebooks/Operational_Data_Headway_and_Frequency_Analysis.ipynb): Computes wait times between consecutive buses and hourly service frequency from GPS telemetry data.
- [**Fare Revenue & Transaction Analysis**](Notebooks/Ticket_Data_Fare_Revenue_and_Transaction_Analysis.ipynb): Analyses passenger demographics, system revenue, and peak travel demand from ticket boarding data.

---

## Potential Use Cases

### Operational Data (GPS Telemetry)

- 🗺️ **Real-time vehicle tracking** on a map (Leaflet, Folium, Kepler.gl)
- ⏱️ **Headway and frequency analysis** — time between consecutive buses on the same route
- 📍 **Stop inference** — detecting deceleration patterns near known stops
- 🔀 **Route deviation detection** — comparing GPS trace against official GTFS shapes
- 🤖 **Arrival time prediction** — ML models using lat/lng/angle/timestamp sequences
- 📊 **Fleet utilization analysis** — active vehicles per route over time

### Ticket Data (Fare Transactions)

- 💰 **Fare revenue analysis** — total fares collected by date, route, operator, and fare type
- 👥 **Passenger volume estimation** — transaction counts as proxy for ridership and demand patterns
- 💳 **Card adoption rates** — proportion of registered vs. unregistered/cash payment transactions
- 🏃 **Peak hour analysis** — fare transaction distribution and demand variation by time period
- 🚌 **Route popularity & demand** — which routes generate highest transaction volume
- 🔀 **Fare integration analysis** — impact of multi-operator integration on ridership and revenue
- 📈 **Temporal trends** — ridership patterns, seasonality, and growth across multiple days
- 🎯 **Passenger segmentation** — analysis of fare types and subsidized ridership patterns
- 🔗 **Mode comparison** — combining GPS traces with transactions to validate vehicle occupancy estimates

### Cross-Dataset Integration

- 🗺️ **Demand mapping** — overlaying fare transactions with GPS routes to identify high-demand corridors
- ⏳ **Service efficiency** — correlating operational metrics (headway, deviation) with passenger revenue
- 📊 **Origin-destination analysis** — using auxiliary stops data and transaction records together
- 🎆 **Multi-modal flow** — analyzing metropolitan integration terminals with transaction patterns


## Requesting Access

To request access to the dataset, please visit the Data Challenge portal:
👉 [https://netmob.org/www26/datachallenge](https://netmob.org/www26/datachallenge)

Access is granted upon agreement with the Terms and Conditions of the challenge.

##  License

This dataset is shared for non-commercial academic use only. Please refer to the Terms and Conditions upon access request.

---

## Data Source & Credits

- **Niterói City**
- **Mobnit API:** Real-time bus data.
- **INMET:** Meteorological data from the Niterói station (A001).
