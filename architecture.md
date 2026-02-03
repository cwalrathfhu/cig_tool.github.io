# Small Starts CIG Evaluation Tool
## Project Architecture, Module Index, and Data Dictionary

---

## 1. Project Purpose

This project is a **modular evaluation framework** for estimating likely **FTA Small Starts Project Justification ratings**.

Goals:
- Rapid screening of project concepts
- Early identification of strengths and weaknesses
- Transparent, reproducible methods aligned with FTA logic
- Minimal reliance on black-box modeling

The tool is intentionally simplified and designed for **internal planning and pre-FTA validation**.

---

## 2. System Structure (High Level)

The system has four layers:
1. **User Inputs** – geometry and key assumptions
2. **Analytical Modules** – GIS, spreadsheets, and structured qualitative reviews
3. **Rating Logic** – explicit FTA-aligned thresholds
4. **Outputs** – tables, maps, and documentation

Modules are independent but share common geometry and assumptions.

---

## 3. Core Geometry (Authoritative)

All spatial analysis is anchored to:
- **Stations** (point features; user-selected)
- **Station Areas** (½-mile buffers)
- **Project Corridor** (optional polyline)

Station areas are the authoritative unit of analysis unless otherwise noted.

---

## 4. Tools and Dependencies

### GIS Platform
- ArcGIS Pro
- Standard geoprocessing tools only:
  - Buffer
  - Intersect
  - Spatial Join
  - Attribute Join
  - Summary Statistics

### Scripting
- ArcPy (Python) for automation and repeatability
- Scripts reference layers already loaded in the ArcGIS Pro map

### Non-GIS Tools
- Excel for cost-effectiveness and simple calculators

### External Services
- ArcGIS Online / Enterprise feature layers
- No direct REST or live web API calls

---

## 5. External Data Sources

- **Population & Employment:** Census Block Groups
- **Community Risk (Equity):** Census Tracts
- **Housing Units:** County-level Census data
- **LBAR / LIHTC Housing:** User-provided table with latitude/longitude
- **Essential Services:** Point datasets (local GIS, ESRI POI, or curated lists)

All external datasets are treated as **read-only inputs**.

---

## 6. Analytical Modules (Index)

### M01 – Core Geometry & Setup
Creates station buffers and validates spatial reference.

### M02 – Population (Land Use)
Area-weights Census Block Group population into station areas.

### M03 – Employment (Land Use / Mobility)
Area-weights or aggregates employment into station areas.

### M04 – Essential Services (Land Use)
Counts essential service destinations within station buffers.

### M05 – LBAR Housing Inventory
Geocodes LIHTC points from lat/lon and calculates LBAR ratios using county housing totals.

### M06 – Community Risk (Equity)
Area-weights tract-level high-risk population into station areas.

### M07 – Cost-Effectiveness
Spreadsheet-based cost per hour of user benefit.

### M08 – Mobility Improvements
Ridership and user benefit aggregation.

### M09 – Congestion Relief
VMT and mode-shift metrics.

### M10 – Environmental Benefits
Emissions and energy impact calculations.

### M11 – Economic Development (Qualitative)
Structured review of plans, zoning, and TOD readiness.

### M12 – Rating & Synthesis Engine
Applies thresholds and aggregates station results to corridor-level ratings.

### M13 – Reporting & Documentation
Produces tables, maps, and FTA-ready narratives.

---

## 7. Core Analytical Concepts

### Area-Weighted Allocation
Used when census geographies do not match station areas:
```
Allocated Value = Source Value × (Overlap Area ÷ Source Area)
```

### Direct Spatial Assignment
Used for point data (housing, services):
```
Assign to station if point falls within buffer
```

### Threshold-Based Ratings
All numeric metrics are converted to categorical ratings:
- High
- Medium-High
- Medium
- Medium-Low
- Low

Thresholds are explicit and stored outside of code when possible.

---

## 8. Data Dictionary (Authoritative)

### Geometry Fields
| Field | Description | Units |
|------|------------|-------|
| Station_ID | Unique station identifier | Text |
| Route_ID | Unique corridor identifier | Text |
| Buffer_Dist | Station buffer radius | Miles |

### Population & Employment
| Field | Description | Units |
|------|------------|-------|
| Total_Pop | Total population (Census) | Persons |
| Employment | Total jobs | Jobs |
| Pop_Density | Population density | Persons / sq mi |

### Housing (LBAR)
| Field | Description | Units |
|------|------------|-------|
| LIHTC_Units | Income-restricted housing units | Units |
| Total_Housing | County-level housing units | Units |
| LBAR_Ratio | Affordable ÷ total housing | Ratio |

### Community Risk
| Field | Description | Units |
|------|------------|-------|
| High_Risk_Pop | Population meeting CRE criteria | Persons |
| High_Risk_Pct | High-risk population share | Percent |

### Essential Services
| Field | Description | Units |
|------|------------|-------|
| Service_Count | Services within station area | Count |

### Ratings
| Field | Description | Units |
|------|------------|-------|
| Rating | FTA-style categorical score | Text |

---

## 9. Outputs

### Quantitative
- Station-level metric tables
- Corridor-level summaries

### Visual
- Validation maps
- Workflow diagrams

### Narrative
- Methodology appendices
- FTA-style writeups

---

## 10. Design Philosophy

- Transparency over optimization
- Reproducibility over complexity
- FTA alignment over academic precision

Every result must be explainable and auditable using standard GIS tools.

---

## 11. Use for Future Chats / Codex

This document is the **single source of truth** for the project.

Future chats should:
- Reference modules by ID (e.g., M04)
- Assume station areas as the base geography
- Avoid redefining core assumptions unless explicitly requested

---

**End of Architecture Document**

