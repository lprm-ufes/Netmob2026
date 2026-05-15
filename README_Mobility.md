# Mobility Data: Real-Time Bus Tracking

## Overview

This dataset contains **real-time GPS telemetry of public buses** operating in Niterói, RJ. The data is collected via the Mobnit API and follows the **GTFS (General Transit Feed Specification)** standard, extended with real-time positional attributes.

After cleaning the data, all buses trips were saved in CSV file. 

---

## Data Directories

### `mobility_data/` — Processed Vehicle Telemetry
CSV files exported every 24h. Each row is a single vehicle observation.
- **Period:** From March 11th to March 30th, 2026. 
- **Frequency:** Approximately every 15-30 seconds per vehicle.
- **Format:** CSV



---

## Schema

| Column | Type | Description |
| :-- | :-- | :-- |
| `id` | `integer` | Unique vehicle identifier (hardware/transponder ID) |
| `timestamp` | `datetime` | Observation time in **BRT (GMT-3)**, format `YYYY-MM-DD HH:MM:SS` |
| `tripId` | `string` | GTFS trip identifier — encodes schedule and direction |
| `routeId` | `integer` | GTFS route identifier |
| `lat` | `float` | Vehicle latitude in **WGS84** decimal degrees |
| `lng` | `float` | Vehicle longitude in **WGS84** decimal degrees |
| `angle` | `float` | Heading in **degrees** (0° = North, 90° = East) |
| `linha` | `string` | Public-facing **route number** (e.g. `31`, `49.1`) |
| `nomeLinha` | `string` | Full route name (Origin X Destination) |
| `headsign` | `string` | Destination sign shown on the bus front |
| `sentido` | `string` | Trip direction — `Ida` (outbound) or `Volta` (inbound) |

---

## Spatial & Temporal Coverage

- **Bounding Box:** Approximately Lat [-22.95, -22.75], Lng [-43.20, -42.95].
- **Timezone:** BRT (UTC-3).
- **Key Hubs:** Barcas Terminal, Icaraí, Centro, Piratininga.

---

## Data Quality Notes

- **Gaps:** Occasional telemetry gaps may occur due to connectivity issues in specific areas.
- **Out of Service:** Vehicles with `null` fields in `linha` or `routeId` are likely moving between depots or are out of service.
- **Precision:** `angle` and coordinates are high-precision floats; rounding may be appropriate for visualization.

[**Back to Main README**](README.md)
