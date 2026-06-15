# Mobility Data: Real-Time Bus Tracking


## Updates

- **2026-06-15:** Days **2026-03-27**, **2026-03-28**, **2026-03-30**, and **2026-03-31** were replaced with new versions containing more readings:

| Date | Old readings | New readings |
| :-- | --: | --: |
| 2026-03-27 | 1,054,078 | 1,067,407 |
| 2026-03-28 | 126,023 | 328,916 |
| 2026-03-30 | 766,665 | 847,024 |
| 2026-03-31 | 894,162 | 895,242 |


## Overview

This dataset contains **real-time GPS telemetry of public buses** operating in Niterói, RJ. The data is collected via the Mobnit API and follows the **GTFS (General Transit Feed Specification)** standard, extended with real-time positional attributes.

After cleaning the data, all buses trips were saved in CSV file. 

---

## Data Directories

### `mobility_data/` — Processed Vehicle Telemetry
One CSV file per day. Each row is a single vehicle observation.
- **Period:** From March 11th to March 31st, 2026.
- **Frequency:** Approximately every 15-30 seconds per vehicle.
- **Format:** CSV (one file per day, named `YYYY-MM-DD.csv`)



---

## Schema

| Column | Type | Description |
| :-- | :-- | :-- |
| `id` | `integer` | Unique vehicle identifier (hardware/transponder ID) |
| `timestamp` | `datetime` | Observation time in **BRT (GMT-3)**, format `YYYY-MM-DD HH:MM:SS` |
| `tripId` | `string` | GTFS trip identifier — encodes schedule and direction |
| `lat` | `float` | Vehicle latitude in **WGS84** decimal degrees |
| `lng` | `float` | Vehicle longitude in **WGS84** decimal degrees |
| `heading` | `float` | Vehicle heading/bearing in degrees (`0`–`360`, clockwise from North) |
| `lineId` | `string` | Public-facing **route number** (e.g. `24`, `49.1`) |
| `lineName` | `string` | Full route name (Origin X Destination) |
| `headsign` | `string` | Destination sign shown on the bus front |
| `direction` | `integer` | Trip direction — `0` (outbound / *Ida*) or `1` (inbound / *Volta*) |

---

## Spatial & Temporal Coverage

- **Bounding Box:** Approximately Lat [-22.95, -22.75], Lng [-43.20, -42.95].
- **Timezone:** BRT (UTC-3).
- **Key Hubs:** Barcas Terminal, Icaraí, Centro, Piratininga.

---

## Data Quality Notes

- **Gaps:** Occasional telemetry gaps may occur due to connectivity issues in specific areas. Known Gaps:
    - 2026-03-18 / 2026-03-19 - telemetry system out of service (these days are **not** included in this dataset).
    - 2026-03-20 - reduced volume of readings, as it is the day after the outage above.
    - 2026-03-22 - interval greater than the expected 15s between each reading.
    - 2026-03-28 - fewer readings than expected.
   

- **Out of Service:** Vehicles with `null`/empty fields in `lineId` (and related route columns) are likely moving between depots or are out of service.
- **Precision:**  coordinates are high-precision floats; rounding may be appropriate for visualization.

---




[**Back to Main README**](README.md)
