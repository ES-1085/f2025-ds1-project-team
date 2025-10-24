# Data

This folder includes all the datasets for the project.

_Last updated: 2025-10-24_

## Dataset 1 — NYC School Energy Consumption (`dataset.json`)


| Field | Type | Description | Units / Examples | Notes |
|---|---|---|---|---|
| `school_id` | string | Unique school/building identifier | `"M015"`, `"K321"` | Use to join to Enrollment |
| `school_name` | string | School/Building name | `"P.S. 015 Roberto Clemente"` | May duplicate address |
| `building_address` | string | Street address | `"111 8 AVENUE"` | Free text |
| `borough` | categorical | NYC borough | `MANHATTAN`, `BROOKLYN`, `QUEENS`, `BRONX`, `STATEN ISLAND` | Standardize case |
| `postcode` | string | ZIP code | `"10011"` | Use as geo proxy if NTA missing |
| `latitude`, `longitude` | numeric | Geographic coordinates | `40.741`, `-74.000` | For map joins/spatial |
| `measurement` | categorical | Energy/cost metric label | `"Electricity Usage (KWH)"`, `"Gas (Therms)"`, `"Steam (MLBS)"`, `"Total Usage (MMBTU)"`, `"Total Cost ($)"` | Drives units for monthly cols |
| `Aug_08` … `Apr_12` | numeric | Monthly readings per `measurement` | e.g., `kWh`, `therms`, `MLBS`, `kW`, `MMBTU`, `$` | Wide months; pivot to long for tidy |
| `FY 2008` … `FY 2012` | numeric | Fiscal-year totals per `measurement` | matches `measurement` units | Optional summary fields |

**_Note that not all columns are being used_**
**Rows: 1,372 Columns: 9**

**Unit notes:**  
- Electricity usage: `kWh`; demand: `kW` (peak).  
- Natural gas: `therms` (1 therm ≈ 0.1 MMBtu ≈ 29.3071 kWh).  
- Steam: often reported as `MLBS` (thousand pounds) or already converted `MMBTU`.  
- “Total Usage (MMBTU)” aggregates across fuels.

---

## Dataset 2 — School Enrollment (`ENROLL2024_20241105.mdb`)


| Field | Type | Description | Units / Examples | Notes |
|---|---|---|---|---|
| `School_ID` | string | Unique school identifier | `"M015"` | Join to `school_id` in Energy |
| `School_Name` | string | Official school name | `"P.S. 015 Roberto Clemente"` |  |
| `District` | integer | NYC school district | `2` |  |
| `Academic_Year` | integer | Year label | `2023` | Use most recent year |
| `Enrollment_Total` | numeric | Total students | `482` | Used for per-student normalization |
| `Grade_Level` | string | Grade span | `"K-5"`, `"9-12"` | Optional stratifier |
| `Building_Code` | string | DOE building code | `"M015"` | Often equals `School_ID` |
| `Address`, `Zip_Code`, `Borough` | string | Location info | `"333 E 4th St"`, `"10009"`, `MANHATTAN` | For geographic joins |

**_This dataset is only being used to find the total enrollment of each school. Most other fields are not being used._**
**Rows: 1,850 Columns: 9**

## Dataset 3 — Neighborhood Financial Health (`Neighborhood_Financial_Health_Digital_Mapping_and_Data_Tool.csv`)


| Field | Type | Description | Units / Examples | Notes |
|---|---|---|---|---|
| `GeoID` | string | Neighborhood/tract/area code | `"BK123"` | Use as stable geo key |
| `Neighborhood_Name` | string | Official neighborhood label | `"Bedford-Stuyvesant"` | Harmonize with school `nta` |
| `Borough` | categorical | NYC borough | `BROOKLYN` |  |
| `Median_Income` | numeric | Median household income | `$55,000` |  |
| `Poverty_Rate` | numeric | Percent below poverty line | `0.21` (21%) | Convert to % if needed |
| `Unemployment_Rate` | numeric | Percent unemployed | `0.07` (7%) |  |
| `Debt_Delinquency_Rate` | numeric | Percent with delinquent debt | `0.18` |  |
| `Financial_Health_Index_Score` | numeric | Composite score (higher = healthier) | `7.5` |  |
| `Index_Rank` | integer | Rank among neighborhoods | `35` | Lower = better |
| `Latitude`, `Longitude` | numeric | Area centroid coordinates | `40.689`, `-73.942` | For mapping |

**Rows: 332 Columns: 10**







