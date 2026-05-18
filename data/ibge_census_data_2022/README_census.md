# IBGE Census Data 2022

## About the IBGE Census

The **IBGE Census** (*Censo Demográfico*) is the official national population and housing survey of Brazil, conducted by the **Instituto Brasileiro de Geografia e Estatística (IBGE)** — Brazil's federal statistics agency. Carried out roughly every ten years, it is the country's most comprehensive socioeconomic data collection effort, covering every household across all municipalities. The Census gathers information on population size and composition, age, sex, race/ethnicity, literacy, education, household characteristics, sanitation, water supply, waste disposal, and the urban environment surrounding dwellings. Data are aggregated and released at multiple spatial scales,  making it a key reference for public policy, urban planning, and academic research.

The 2022 edition is the most recent round and is the source of all indicators in this folder.

## Dataset Overview

This dataset contains spatial boundaries and census indicators for Niterói-RJ, derived from the 2022 IBGE Census.  This dataset was processed by the Niterói City Hall to group data by spatial boundaries of Niterói neighborhoods.

Folder and file names have been standardized in English for international audiences.

## Directory Structure

| Filename | Description |
|---|---|
| `niteroi_census_borders.gpkg` | GeoPackage containing spatial boundaries (sectors/neighborhoods). |
| `favelas_indicators/` | Statistics on favelas (or urban communities) from Niteroi. |
| `census_tracts_indicators/` | Statistics data at the Census Tract level. |

## `favelas_indicators/`

| Filename | Description |
|---|---|
| `dict_favelas_census.csv` | Metadata dictionary defining codes/labels for absolute counts. |
| `dict_favelas_census_pct.csv` | Metadata dictionary defining codes/labels for percentage tables. |
| `household_indicators_2022.csv` | Absolute counts of households (e.g., water supply, waste disposal, sanitation). |
| `household_indicators_pct_2022.csv` | Percentage breakdown of household infrastructure characteristics. |
| `population_indicators_2022.csv` | Demographic data including total population counts by age groups and sex. |
| `population_indicators_pct_2022.csv` | Percentage breakdown of demographic cohorts (age/sex distribution). |
| `literacy_indicators_2022.csv` | Absolute number of literate vs. illiterate individuals (usually aged 15+). |
| `literacy_indicators_pct_2022.csv` | Literacy rates across the mapped favela communities. |
| `household_heads_indicators_2022.csv` | Absolute counts of household heads categorized by sex, race, and age. |
| `household_heads_indicators_pct_2022.csv` | Percentage breakdown of household heads' demographics. |
| `household_heads_disaggregated_pct_2022.csv` | Highly granular cross-tabulations (e.g., female heads of household with children). |
| `neighborhood_environment_indicators.csv` | Absolute counts of urban surroundings features aggregated by neighborhood. |
| `neighborhood_environment_indicators_pct.csv` | Percentages of public infrastructure coverage (paving, lighting, sidewalks) by neighborhood. |

## `census_tracts_indicators/`

| Filename | Description |
|---|---|
| `dict_census_tracts_2022.csv` | Metadata dictionary defining codes and labels for the census tract variables. |
| `density_indicators_2022.csv` | Population density indicators (e.g., residents per hectare, average residents per household). |
| `household_indicators_2022.csv` | Absolute counts of household infrastructure and characteristics per tract. |
| `population_indicators_2022.csv` | Demographic profile (age, sex distribution) aggregated by census tract. |
| `literacy_indicators_2022.csv` | Absolute literacy and illiteracy counts for population segments within each tract. |
| `household_heads_indicators_2022.csv` | Demographic characteristics of the designated heads of households per tract. |
| `block_face_environment_indicators.csv` | Absolute counts of urban infrastructure elements observed on individual street/block faces. |
| `block_face_environment_indicators_pct.csv` | Percentage coverage of urban infrastructure features (e.g., % of faces with sidewalks, ramps, lighting). |

## Common Header Translations (IBGE Terms)

When translating the content from dictionaries (`dict_*.csv`), here is a quick reference for the standard IBGE-to-English terminology:

- **Setor Censitário**: Census Tract
- **Domicílios**: Households
- **Entorno**: Urban Surroundings / Built Environment
- **Alfabetização**: Literacy
- **Responsável pelo domicílio**: Head of household
- **Favelas e Comunidades Urbanas (FCU)**: Favelas and Urban Communities (formerly Subnormal Agglomerations)
- **Face**: Block face / Street segment

> **Attention:** Also see the dictionary's content to be aware of the names of indicators (they are still in Portuguese for consistency, but with descriptions in English).
