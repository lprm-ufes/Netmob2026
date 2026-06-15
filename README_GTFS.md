# GTFS & Mobility Data 


This dataset pairs [**Bus Mobility Data** ](README_Mobility.md) with the **scheduled GTFS feeds**
of the two transport companies operating in the region. This document explains what is published,
how the feeds relate to the mobility data, the coverage gaps, and the rules for combining the
feeds correctly.

> **What is GTFS?** The *General Transit Feed Specification* is an open, standard format for
> publishing public-transport schedules and their geography. A feed is a set of linked CSV
> files describing the **agencies**, **routes**, **trips**, **stops**,
> **stop times**, service **calendars**, and route **shapes** of a transit network. Each `trip`
> is one scheduled run of a vehicle along a route; the same `trip_id`s appear in the observed
> mobility data, which is what lets us join the two.

### What is new

A **static spatial layer** was already provided in `auxiliar_data/`. The GTFS feeds add the
**scheduled (temporal) dimension** and a complete, id-linked operational inventory that the
static layer does not have.

| Aspect | `auxiliar_data/` (already provided) | GTFS feeds (new) |
|--------|-------------------------------------|------------------|
| Stops | `stops.json` — 257 curated stops (Points; address, neighborhood; coded `A001…`) | `stops.txt` — 576 (TransNit) + 623 (TransOceânico) operational stops, keyed by `stop_id` |
| Lines / routes | `line_routes.json` — 43 line geometries (MultiLineString, by name, e.g. `L03_Circular`) | `routes.txt` + `shapes.txt` — 29 + 26 routes with `route_id` and per-trip shapes |
| Integration terminals | `stops_integration_city.json`, `stops_integration_metropolitan.json` | — (not in GTFS) |
| Weather | `meteorological_data.csv` | — (not in GTFS) |
| **Timetable** | — (none) | **`trips.txt`, `stop_times.txt`, `calendar*.txt`, `frequencies.txt`** |
| **Fares** | — | `fare_attributes.txt`, `fare_rules.txt` |
| **Link to mobility data** | not id-matched | `trip_id` ↔ mobility `tripId` (see §2) |

In short: the static layer describes **where** a subset of stops and lines are; GTFS adds
**when** every scheduled trip runs, the per-company route/stop inventory, and the `trip_id`s
that tie the schedule to the observed mobility.

---

## 1. What is published

The published feeds live in **`GTFS_data/`** as three folders containing **three GTFS
snapshots** in total.  Each snapshot is a full GTFS feed in its own subfolder, named by  the company and  export date.



| folder | Company | Snapshots inside | Covers |
|---------|---------|------------------|--------|
| `GTFS_data/TransNit_20260320/` | **TransNit**  | `20260320184600` | whole month (feed is stable) |
| `GTFS_data/TransOceanico_20260315/` | **TransOceânico**  | `20260315060028` | early/mid month |
| `GTFS_data/TransOceanico_20260329/` | **TransOceânico**  | `20260329060040` | late month |

Each snapshot subfolder is a standard GTFS feed made of these files:

| File | Content |
|------|---------|
| `agency.txt` | Transit agencies (operating companies) in the feed — name, URL, timezone. |
| `routes.txt` | Routes (bus lines) — `route_id`, short/long name, type. |
| `trips.txt` | Individual scheduled trips per route — `trip_id`, `route_id`, `service_id`, direction, `shape_id`. |
| `stops.txt` | Stops/stations — `stop_id`, name, latitude/longitude. |
| `stop_times.txt` | Arrival/departure times of each trip at each stop (the bulk of the feed). |
| `calendar.txt` | Service patterns — which weekdays each `service_id` runs, with start/end dates. |
| `shapes.txt` | Geographic paths (point sequences) that trips follow, for map drawing. |


| Company | published snapshots | stops | routes | agencies in `agency.txt` |
|---------|--------------------:|------:|-------:|--------------------------|
| **TransNit** | 1 | 576 | 29 | VIAÇÃO ARAÇATUBA · AUTO LOTAÇÃO INGÁ – MUNICIPAL · AUTO ÔNIBUS BRASÍLIA |
| **TransOceânico**  | 2 | 623 | 26 | CONSÓRCIO TRANSOCEÂNICO |

> Note: The operator made available several snapshots for March 2026. Since no single snapshot covers the whole period, we evaluated the combination of every pair of snapshots against the mobility data and selected the combination with the largest intersection. Combining the three snapshots described in this file leads to a coverage of **99.7 %** of the trips in the Mobility Data (see Section 3).


---

## 2. How mobility `tripId` maps to GTFS `trip_id`

In the mobility data, the collumn  `tripId` is the GTFS `trip_id` **without its trailing `_<direction>` suffix**:

```
mobility tripId : 1476582_U_37
GTFS trip_id    : 1476582_U_37_0   (direction 0)
                  1476582_U_37_1   (direction 1)
```

Raw string equality yields **0 matches**.  You must strip the direction suffix before joining. After stripping, a mobility trip is "covered" by a feed when its base id is present in that feed's `trip_id` set.

The two companies use **disjoint `trip_id` namespaces** (0 shared trip_ids), so every mobility
trip belongs to exactly one company; the two feeds never compete for the same trip.

---

## 3. Coverage gaps — why one snapshot per company is not enough

The schedule **changed mid-month**, so no single snapshot covers the whole period:

- **TransNit** is near-saturated: almost any snapshot covers ~100 % of its trips.
- **TransOceânico** is the constraint: the best single snapshot reaches only **87 %** mean
  daily coverage, because early-month trips (Mar 11–13) and late-month trips (Mar 30–31) live
  in *different* snapshots.

**Solution — snapshots per company, used as a union.** Selecting two snapshots per company
lifts combined coverage from **92.8 %** (best single-file pair) to **99.7 %**:

```
TransNit       : 20260320184600
TransOceânico  : 20260315060028  +  20260329060040
```

With this selection,  **99.75 %** of trips in the Mobility data are covered  and the union leaves only **66**
day-trip observations uncovered across the whole month.

---

## 4. The 5 remaining gaps (covered by neither company)

After the union above, **5 distinct `tripId`s** are present in the mobility data but exist in
**no GTFS snapshot of either company**:

| tripId | Notes |
|--------|-------|
| `T75279` | recurs on several days |
| `T75280` | recurs on several days |
| `T75281` | recurs on several days |
| `T75282` | recurs on several days |
| `T76935` | recurs on several days |

These ids use a different convention (a `T`-prefix instead of the usual
`<number>_<U/S>_<n>` pattern) and are absent from every `trips.txt`. They are most likely
special / unscheduled service that was never published in any GTFS export. They account for the
66 uncovered day-trip entries and the residual ~0.3 % gap. 

---

## 5. Combining the feeds — how auxiliary data overwrites

`trips.txt` references **routes** (`route_id`) and **stops** (via `stop_times.txt` → `stop_id`),
so to use the trips you must also load `routes.txt`, `stops.txt`, `shapes.txt`, `calendar*.txt`.
When you merge feeds, ID collisions in these auxiliary tables must be resolved by **overwriting
duplicates** (last-writer-wins on the primary key), not by blind concatenation:

- **Two snapshots of the same company** (e.g. the two TransOceânico files): `stops.txt` and
  `routes.txt` are **identical** across snapshots (623 stops / 26 routes, fully shared). Only
  `trips.txt`, `stop_times.txt` and `calendar*.txt` differ. → De-duplicate stops and routes by
  `stop_id` / `route_id`; the auxiliary tables collapse to one copy, while trips are the union.

- **The two companies together**: `route_id` namespaces are **disjoint** (0 shared) → routes
  simply concatenate. But the companies **share 112 `stop_id`s** (the same physical bus stops).
  When merged, those rows collide and one feed's definition **overwrites** the other. Pick a
  deterministic winner (e.g. keep the first feed loaded, or the one with the more complete
  fields) so the merged `stops.txt` has exactly one row per `stop_id`.

**Practical merge order:** load TransNit, then TransOceânico; for `stops.txt` keep one row per
`stop_id` (overwrite duplicates); for `routes.txt` and `trips.txt` take the union (no key
conflicts). Within a company, deduplicate stops/routes across its two snapshots and union the
trips.

---

## 6. Why the feeds must be used together

1. **Trips are partitioned across companies** - each mobility trip exists in only one
   company's feed (0 shared trip_ids). Loading only TransNit *or* only TransOceânico leaves
   roughly half the observed trips unexplained. Both companies are required for full mobility
   coverage.
2. **Trips are partitioned across snapshots within a company** -TransOceânico's mid-month
   schedule change means one snapshot misses either the early or the late trips. Both of its
   snapshots are required.
3. **Auxiliary tables resolve the references** - a trip is only usable when its `route_id` and
   `stop_id`s resolve, so the merged `routes.txt`/`stops.txt` (deduplicated as in §5) must be
   loaded alongside the unioned trips.


