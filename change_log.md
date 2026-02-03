# Small Starts CIG Evaluation Tool
## Project Change Log (LLM-Oriented)

---

### Purpose of This File
This change log records **major conceptual, architectural, and methodological updates** to the Small Starts CIG Evaluation Tool. It is intended to allow a new LLM or collaborator to quickly understand how and why the project evolved, without re-deriving prior decisions.

Only **material changes** are recorded; minor wording or formatting edits are omitted.

---

## v0.1 – Initial Concept Framing
**Status:** Superseded

**Key Characteristics:**
- Focus on Small Starts Project Justification criteria
- Early emphasis on Land Use (population density)
- Ad hoc GIS scripting approach

**Limitations Identified:**
- No shared project architecture
- Metrics and scripts tightly coupled
- High token cost for re-contextualization

---

## v0.2 – Land Use Methodology Formalization

**Major Changes:**
- Defined station areas as the **authoritative unit of analysis**
- Standardized 0.5-mile station buffers
- Adopted area-weighted allocation for census geographies

**Impacts:**
- Enabled consistent population and employment calculations
- Improved defensibility for FTA-style review

---

## v0.3 – Equity and Housing Enhancements

**Major Changes:**
- Introduced **LBAR Housing Inventory** module
  - LIHTC data geocoded directly from user-provided lat/lon
  - Avoided address-based geocoding dependencies
- Defined **county-level housing units** as denominator for LBAR ratios
- Added **Community Risk (CRE)** using census tract geography

**Key Clarifications:**
- CRE limited to tract-level resolution
- Explicit GEOID-based joins required

---

## v0.4 – Essential Services Definition

**Major Changes:**
- Added Essential Services as a Land Use subfactor
- Defined services as point-based, walk-access destinations
- Established count-per-station methodology

**Impacts:**
- Shifted Land Use focus from density-only to functional access
- Improved alignment with FTA qualitative intent

---

## v0.5 – Toolchain Stabilization

**Major Changes:**
- Standardized on ArcGIS Pro as the primary analytical environment
- Explicitly limited tools to:
  - Buffer
  - Intersect
  - Spatial Join
  - Attribute Join
- Eliminated reliance on live web APIs

**Impacts:**
- Improved reproducibility
- Reduced technical fragility

---

## v0.6 – Modular Architecture Introduction

**Major Changes:**
- Introduced a formal **module-based architecture (M01–M13)**
- Separated geometry, analytics, ratings, and reporting
- Enabled independent module development

**Impacts:**
- Simplified maintenance
- Enabled partial runs and scenario testing

---

## v0.7 – Plain-Language Workflow & Infographic

**Major Changes:**
- Rewrote methodology in layman-friendly workflow format
- Created one-page infographic representation

**Purpose:**
- Improve communication with non-technical stakeholders
- Reduce interpretation risk

---

## v0.8 – Architecture Consolidation

**Major Changes:**
- Created a single authoritative architecture document
- Integrated:
  - System structure
  - Module index
  - Core analytical concepts
  - Data dictionary

**Design Decision:**
- One document serves as the single source of truth

---

## v0.9 – Codex / LLM Readiness

**Major Changes:**
- Optimized documentation for LLM onboarding
- Explicitly documented assumptions, dependencies, and constraints
- Added usage guidance for future chat instances

**Outcome:**
- New LLM instances can operate at the module level immediately
- Token burn from re-contextualization minimized

---

## Current State Summary

As of the latest revision:
- Architecture is stable
- Core assumptions are explicit
- All major Project Justification criteria are represented
- Documentation is optimized for both human and LLM reuse

---

**End of Change Log**

