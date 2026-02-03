# Small Starts CIG Web App
## Architecture Overview (Authoritative for LLM Sessions)

---

## 1. What This Application Is

This project is a **client-side, browser-based prototype web app** for screening **FTA Small Starts – Project Justification (Land Use)** metrics.

Key characteristics:
- Runs entirely in the browser (no backend)
- Uses live federal APIs + user-uploaded files
- Intended for **planning-grade evaluation**, not official FTA submittals
- Optimized for transparency, explainability, and rapid iteration

The app is currently implemented as a **single-page HTML + JavaScript application**.

---

## 2. High-Level Architecture

### Execution Model
- **Frontend-only** (HTML, CSS, JavaScript)
- No server-side logic
- No database
- All state lives in memory during the session

### Core Libraries
- **MapLibre GL JS** – interactive map and visualization
- **Turf.js** – all spatial geometry operations
- **PapaParse** – robust CSV parsing
- **pako** – gzip decompression (LODES files)

### External Data Sources (Live)
- Census TIGERweb (geometry)
- Census ACS API (tabular data)
- LEHD LODES (file-based, user downloaded)

---

## 3. Core User Workflow

1. User clicks on map to place **station points**
2. App creates **0.5-mile radial buffers** around each station
3. Buffers are **dissolved into a union polygon** to avoid double counting
4. App computes Land Use–related metrics inside the union
5. Metrics are translated into **FTA Small Starts breakpoint ratings**

All calculations update dynamically as stations or inputs change.

---

## 4. Authoritative Geometry Concepts

### Stations
- User-defined point features (lat/lon)
- Stored in-memory as GeoJSON

### Station Buffers
- Fixed radius: **0.5 miles**
- Generated using Turf.js

### Station-Area Union
- Dissolved union of all buffers
- Used for all aggregate calculations
- Prevents double counting across overlapping stations

This union polygon is the **primary spatial mask** for analysis.

---

## 5. Data Domains and How They Are Computed

### 5.1 Population & ACS Variables

**Source:** Census ACS 5-year API

**Process:**
- Fetch intersecting census geographies (tracts or block groups) via TIGERweb
- Fetch ACS values by GEOID
- Area-apportion values based on polygon overlap

**Aggregation Rules:**
- Additive variables (population, households): area-weighted sum
- Non-additive variables (medians): area-weighted average estimate (explicitly flagged)

---

### 5.2 Employment (LODES)

**Source:** LEHD LODES WAC files (user-uploaded `.csv.gz`)

**Process:**
- User downloads official statewide file
- File is parsed client-side
- Census block internal points are fetched via TIGERweb
- Jobs are summed for blocks whose internal point falls within station-area union

**Notes:**
- Screening-grade method
- Memory-intensive for large states

---

### 5.3 Community Risk (CRE)

**Source:** User-uploaded CSV (Census tract level)

**Process:**
- User maps CSV columns (GEOID, total pop, high-risk pop)
- Tract GEOIDs normalized to 11-digit format
- Tracts intersecting station-area union are fetched via TIGERweb
- High-risk and total population are area-apportioned

**Output:**
- Percent high-risk population in station areas

---

### 5.4 Essential Services

**Source:** User-uploaded GeoJSON or CSV point data

**Process:**
- Points loaded into memory
- For each station, a **1-mile buffer** is generated
- Points within each buffer are counted
- Average count across stations is computed

**Output:**
- Average essential services per station area

---

### 5.5 LBAR Housing Inventory

**Source:** User-uploaded GeoJSON or CSV (lat/lon + units)

**Process:**
- LBAR sites counted if they fall inside station-area union
- Total housing units in station area fetched from ACS
- County-level housing totals fetched from ACS (user-specified counties)
- LBAR ratio = (station-area share ÷ county share)

**Special Rule:**
- If county LBAR share > 5%, rating is boosted by one level

---

## 6. Rating Engine (FTA Small Starts – Land Use)

The app implements **explicit FTA breakpoint tables** for:
- Average population density
- Employment served
- LBAR ratio
- Community Risk (high-risk %)
- Essential services (avg per station)

### Rating Scale
- High
- Medium-High
- Medium
- Medium-Low
- Low

Ratings are purely rule-based and deterministic.

---

## 7. State Management

All application state is held in-memory:
- Station points
- Buffer geometries
- Uploaded datasets (CRE, LBAR, services, LODES)
- Computed summaries and ratings

There is no persistence across page reloads.

---

## 8. UI Organization

### Sidebar
- Station controls
- Variable selection
- Upload panels
- Metric summaries
- Rating panels

### Map Panel
- Basemap (Carto light raster)
- Stations
- Buffers
- Optional overlays (census geographies, LBAR points)

UI is intentionally utilitarian and analysis-focused.

---

## 9. What This App Does *Not* Do

- No backend processing
- No user authentication
- No persistent storage
- No cost-effectiveness, mobility, congestion, or environmental scoring (yet)
- No official FTA validation

---

## 10. Design Philosophy

- Client-side transparency over performance
- Explicit approximations over hidden models
- Reproducibility via public APIs and user-provided data
- Planning-grade screening, not compliance modeling

---

## 11. Guidance for Future LLM Sessions

When modifying this repo:
- Preserve the frontend-only architecture
- Do not introduce backend dependencies without explicit instruction
- Treat the station-area union as the authoritative geography
- Do not silently change rating thresholds or aggregation rules
- Prefer incremental changes over refactors


---

**End of Architecture Document**

