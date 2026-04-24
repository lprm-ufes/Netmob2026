# Ticket Data: Passenger Boarding Transactions

## Overview

This dataset contains passenger transaction records from the public bus system in Niterói, RJ. Each record represents a single boarding event.

**Collection Period:** March 2026.
**Total Records:** ~7.2 million transactions.
 >**Free Travels:** 1,969,370  

 >**Paid with Card**: 4,365,789 (including cards used to validate payments with money)

 > **Paid in Cash**: 912,801

**Distinct users:** 377,755 distinguishible transactions (by card_id)

---

## Directory Structure

```
ticket_data/
 ├── 2026-03-01.csv 
 ├── 2026-03-02.csv 
 └── ...
 └── 2026-03-31.csv
```

---

## Schema Description

| Column | Type | Description |
| :-- | :-- | :-- |
| `transaction_date` | `datetime` | Timestamp of boarding (BRT, UTC-3) |
| `anon_user_id` | `int` | Anonymized user identifier (0 = cash/no card) |
| `fare_type` | `int` | Payment modality (see Mappings) |
| `vehicle_number` | `int` | Unique bus ID |
| `company_number` | `int` | Bus operator/company ID |
| `route_detail_id` | `int` | Internal route variant identifier |
| `integration_flag` | `int` | Indicates if the trip was a transfer |
| `card_type` | `int` | Passenger category (see Mappings) |
| `debited_amount` | `float` | Fare amount in BRL |
| `view_type` | `str` | Full route name |
| `route_name` | `str` | Short route line code (e.g. `44`) |
| `day_type` | `int` | Weekday, weekend, or holiday |
| `day_period` | `int` | Time of day (Dawn, Morning, etc.) |

---

## Categorical Mappings

### Fare Types
1. **Free:** Free pass (student, senior, etc.)
2. **Electronic:** Paid via card
3. **Cash:** Paid in cash

### Card Type
- `-1`: No card (Cash)
- `1`: Standard fare card
- `2`: Student free pass
- `3`: Working/Labor pass
- `4`: Senior free pass

### Integration Flag
- `1`: No integration (Direct trip)
- `2`: Integration (Transfer trip)

---

## Analysis Use Cases

- **Demand Heatmaps:** Identifying high-demand boarding points.
- **Ridership Patterns:** Peak vs. off-peak demand variations.
- **Integration Efficiency:** Studying transfer volumes at major hubs.
- **Revenue Estimation:** Analyzing fare collection across different operators and routes.

---

## Data Quality Notes

- **Anon User ID 0**: Corresponds to cash transactions or unregistered cards. Note that `card_chip_id` has been anonymized to `anon_user_id`.
- **Debited Amount 0.0:** Indicates a free ride (subsidized) or an integration transfer.
- **Timezone:** All timestamps are in BRT (UTC-3).

[**Back to Main README**](README.md)
