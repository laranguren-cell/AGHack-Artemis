# AGHack-Team Artemis
Artemis team submission for Arizona Hack, November 15, 2026
# CanopyClear — Orchard Boundary-Layer & Spray Decision Telemetry (`index.html`)
Presentation: https://docs.google.com/presentation/d/1kRqKMToIvLMfo4pgYEG91DVn5Teik8UNPZKtS88Bmpk/edit?usp=sharing 
**CanopyClear** is a standalone, zero-dependency HTML5/CSS3/JavaScript microclimate telemetry and spray decision-support interface built for California orchard operations. Packaged in a single self-contained `index.html` file, it loads instantaneously offline in any modern web browser without external libraries, servers, or build tools.

> **Core Thesis:** A conventional single-height ground sensor only measures conditions near the orchard floor—it cannot detect whether air above the canopy is warmer (surface temperature inversion), faster (wind-speed shear), or blowing in another direction (directional twist). Adding a second sensor node above the canopy makes true vertical boundary-layer profiling possible.

---

## Quick Start

1. Download `index.html` from this repository.
2. Double-click `index.html` to open it in any modern browser (**Chrome**, **Edge**, **Firefox**, or **Safari**).
3. *(Optional)* Host directly on **GitHub Pages**, **Netlify**, or **Vercel** with zero configuration—everything (layout styles, telemetry logic, and SVG diagrams) is self-contained inside `index.html`.

---

## Interface & Telemetry Capabilities

### 1. Single-Sensor vs. Dual-Sensor Side-by-Side Comparison
* **Conventional Single-Sensor Baseline:** Evaluates temperature, wind speed, relative humidity, lower-node placement, and downwind receptor rules using only the sub-canopy sensor (`LOWER NODE PASS`), explicitly marking vertical temperature gradients ($\Delta T$), speed shear, and directional twist as `CANNOT MEASURE`.
* **CanopyClear Dual-Sensor System:** Combines lower and upper node readings to compute vertical atmospheric gradients and automatically classifies the comparison outcome:
  * **`BLIND SPOT CAUGHT`**: Lower-node readings appear acceptable, while the upper sensor catches an inversion, excessive upper wind, or severe canopy shear.
  * **`FAIL-SAFE ACTIVATED`**: Upper sensor telemetry is missing or out of range; automated clearance halts (`UNKNOWN`) rather than assuming upper-canopy safety.
  * **`SAME DECISION, MORE EVIDENCE`**: Both systems reach the same decision, while the dual system verifies vertical stability across the canopy.
  * **`DUAL-SYSTEM CHECK REQUIRED`**: Upper sensor is mounted below or within the uncertainty-adjusted canopy envelope.

### 2. Orchard Geometry & Canopy Uncertainty Engine
* **5 California Commercial Crops:** Almond, Walnut (`Traditional` and `Hedgerow` spacing systems), Pistachio, Citrus, and Apple/Pear.
* **Illustrative Curves + Field Overrides:** Estimates canopy height from tree age or accepts farmer-measured canopy heights (`±1.0 ft` assumed field uncertainty) that remain preserved even if tree age is later adjusted.
* **Uncertainty-Adjusted Clearance:** Calculates an **Est. Canopy Top** (`Canopy Height + Uncertainty`) and highlights a **6–10 ft** target clearance band above it. Modifying canopy or mast geometry locks out stale readings until **`Collect New Reading`** is triggered.

### 3. Automatic Pesticide Product Label & Limitation Verification
Automatically evaluates active orchard, receptor, and microclimate conditions against embedded product profiles—updating restrictions live as weather or time forecasts change:
* **JMS Stylet-Oil** (`EPA Reg. No. 65564-1`) — Enforces `85°F` heat advisory / `90°F` stop ceilings, `35°F`/`32°F` cold injury limits, low-humidity evaporation thresholds (`≤30% RH`), and sulfur-interval warnings.
* **Brigade 2EC** (`EPA Reg. No. 279-3313`, *Restricted Use Pesticide*) — Enforces stricter canopy shear limits (`3.0 mph` / `5.0 mph`), directional twist limits (`30°` / `60°`), inversion prohibitions, and mandatory `150 ft` surface-water/canal buffers.
* **Microthiol Disperss** (`EPA Reg. No. 70506-187`) — Flags crop eligibility restrictions (e.g., walnut sulfur sensitivity), lower heat-scorch thresholds (`80°F` / `85°F`), low-RH caps (`≤40%`), and 21-day oil compatibility intervals.

### 4. Diurnal & Seasonal Time-Variance Forecast Mode
* Switch between **Snapshot (`+0h`)** and **Time Variance Forecast (`+0.0h` to `+6.0h`)** to simulate how boundary-layer conditions evolve from the selected local hour (`00:00–23:59`) and month across California Central Valley seasonal regimes (**Winter**, **Spring**, **Summer**, **Fall**).
* Includes an animated **`Play Progression (+6h)`** controller, a clickable **7-step hourly forecast strip**, and an inline **SVG temperature & inversion ($\Delta T$) sparkline** modeling dawn inversion breakup, afternoon thermal mixing, and evening radiative cooling.

### 5. True-Scale SVG Elevation Diagram & 5-Column Gate Table
* **Interactive Elevation Chart:** Renders trees, canopy uncertainty shading, conservative canopy tops, the translucent **6–10 ft** recommended upper-sensor zone, sensor mast lines, and directional wind vectors to true vertical scale.
* **5-Column Gate Breakdown:** Separates `Single Input` and `1-Sensor Result` from `Dual Input` and `2-Sensor Result` for every atmospheric, geometry, and label gate.
* **Browser CSV Export:** Clicking **`Export CSV Log`** downloads `canopyclear_telemetry_log.csv` with UTC and California (`America/Los_Angeles`) timestamps, central/effective canopy clearances, comparison classifications, and active rule codes.

---

## Built-In Demonstration Scenarios

| Scenario | Lower Node (`5 ft`) | Upper Node (`Above Canopy`) | 1-Sensor Result | 2-Sensor Result | What It Demonstrates |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Ground Illusion** *(Default)* | `58.0°F`, `4.0 mph` at `90°` | `63.0°F`, `12.0 mph` at `30°` | `LOWER NODE PASS` | `STOP` | Exposes `+5.0°F` inversion, `8.0 mph` speed shear, and `60°` twist |
| **Normal Weather** | `72.0°F`, `5.0 mph`, `52% RH` | `70.0°F`, `6.0 mph`, `50% RH` | `LOWER NODE PASS` | `GO` | Confirms normal lapse rate and low shear across both heights |
| **Developing Inversion** | `58.0°F`, `4.0 mph`, `62% RH` | `58.7°F`, `4.5 mph`, `60% RH` | `LOWER NODE PASS` | `CAUTION` | Catches an early `+0.7°F` warm-air cap forming above the trees |
| **Hot and Dry** | `86.0°F`, `5.0 mph`, `26% RH` | `85.0°F`, `6.5 mph`, `25% RH` | `CAUTION` / `STOP` | `CAUTION` / `STOP` | Tests product-specific heat and evaporative drift thresholds |
| **Upper Sensor Failure** | `70.0°F`, `5.0 mph`, `60% RH` | `OFFLINE / NULL` | `LOWER NODE PASS` | `UNKNOWN` | Activates fail-safe halt when upper-canopy telemetry drops out |

---

## Disclaimer
CanopyClear is a research and decision-support prototype. Illustrative growth curves and diurnal boundary-layer projections are simulated for demonstration purposes, and embedded product checks do not replace a complete review of official US EPA and California Department of Pesticide Regulation (CDPR) labels prior to pesticide application.
