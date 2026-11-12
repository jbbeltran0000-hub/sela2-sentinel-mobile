# SELA Sentinel — Mobile Environmental Monitoring & Analytics Tool

**SELA Sentinel** is a mobile-first, open-source community air quality monitoring and analytics prediction tool designed for Southeast Los Angeles (SELA) communities—including South Gate, Huntington Park, Cudahy, Bell Gardens, Maywood, Florence-Firestone, and Walnut Park.

Built for rapid field use on smartphones during community walks, fence-line observations, and Assembly Bill (AB) 617 meetings, the tool aggregates open-source environmental telemetry and formats observations into standardized schemas compatible with California Air Resources Board (CARB) AQview and Community Emissions Reduction Program (CERP) reporting workflows.

---

## Key Features

* **Mobile-First Bottom-Sheet UI (`100dvh`):** Full-bleed map viewport with a collapsible bottom drawer that keeps filter controls, bilingual toggles (`EN / ES`), and export triggers accessible one-handed on small screens.
* **Multi-Variable Corridor Filtering:** Filter live readings by pollutant (`PM2.5`, `PM10`, `VOCs / Benzene`, `NO₂ / Diesel`), SELA jurisdiction, and freight/industrial corridors (`I-710`, `Alameda Corridor`, `I-105`).
* **CARB AQview & CERP CSV Export:** One-tap export packages filtered readings with standardized EPA AQS parameter codes, QA/QC flags, CERP strategy references, and prepended community science metadata headers.
* **Zero-Build Standalone Architecture:** Runs as a lightweight static web application (`index.html`) that deploys directly from GitHub Pages with no backend server overhead.

---

## Repository Structure

```text
sela-sentinel/
├── index.html          # Standalone mobile-first interface, bottom-sheet UI, and CSV exporter
├── data/
│   └── stations.json   # Local/fallback sensor telemetry payload
└── README.md           # Project documentation, schema reference, and legal disclaimer
