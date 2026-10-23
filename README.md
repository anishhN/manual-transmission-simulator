# Transmission Dynamics Studio — Manual Transmission & Powertrain Simulator (PRO v2.5)

An interactive, browser-based 3D manual transmission and powertrain engineering simulator and virtual test bench. It models the kinematics, thermodynamics, mechanical losses, and dynamic response of a manual transaxle coupled to a diesel engine benchmarked against the published specifications of the **Yanmar 2GM20F**.

![Screenshot](screenshot.png) *(optional preview)*

---

## 🌟 Key Features

### 1. Dual Operational Modes
* **🏎️ 3D Live Cockpit**: Real-time interactive driving mode with live 3D cutaway powertrain, tactile gear selector gate, automotive pedal switchboard, 8 KPI telemetry metric cards, and live strip charts.
* **📊 Test Cases & Data Bench**: Comprehensive engineering test bench for running structured test suites, inspecting numerical time series with pagination, overlaying comparison curves, analyzing statistical distributions, and exporting data.

### 2. Interactive 3D Powertrain Cutaway (Three.js)
* **Procedural Gear Meshes**: Custom extruded 2D spur gears for 5 forward speeds and reverse.
* **Dynamic Digital Twin**: Real-time rotating crankshaft, dual-mass flywheel, clutch disc, input/output countershafts, and differential.
* **Kinematic Feedback**: Synchronizer sleeves visibly slide along the countershaft into the selected gear; active gear pairs highlight in anodized teal (`#22D3C5`).
* **Camera Controls**: Orbit controls (drag to orbit, pan, mouse wheel to zoom), auto-rotate, and view presets (*Isometric*, *Gearset Close-Up*, *Differential*, and *Reset View*).

### 3. Kinematics, Dynamics & Thermodynamics
* **Engine Dynamics**: Transient exponential lag filter with load-dependent speed curves (idle: 700 RPM, operating limit: 3,600 RPM).
* **Torque & Power Curves**: Piecewise torque curve calibrated to Yanmar 2GM20F continuous (11.8 kW @ 3,400 RPM / 33.1 N·m) and 1-hour ratings (13.4 kW @ 3,600 RPM / 35.5 N·m).
* **Gear Ratios**: Reverse (3.30:1), Neutral (0:1), 1st (3.60:1), 2nd (2.10:1), 3rd (1.40:1), 4th (1.00:1), 5th Overdrive (0.80:1), plus configurable Final Drive (2.00:1 to 5.00:1).
* **Thermodynamics**: First-order dynamic thermal lag response for coolant ($\tau = 18\text{ s}$) and exhaust gas ($\tau = 5\text{ s}$).
* **Fuel & Energy**: Volumetric fuel consumption (L/h), cumulative fuel consumption (L), BSFC curve (g/kWh), and fuel chemical heat release based on diesel lower heating value ($42.7\text{ MJ/kg}$).
* **Mechanical Losses & Vibration**: Transmission efficiency degradation modeled against backlash/wear, clutch slip angular velocity, and heuristic vibration amplitude (mm/s RMS).

---

## 🧪 Test Case Suite & Templates

The virtual test bench comes pre-configured with **10 structured test templates**:

1. **TC-001: Engine Idle Operation** — Neutral gear baseline at nominal temperature.
2. **TC-002: First Gear Acceleration** — Maximum tractive torque evaluation in 1st gear (3.60:1).
3. **TC-003: Gear Shift Comparison (2nd Gear)** — Mid-range cruise dynamics in 2nd gear (2.10:1).
4. **TC-004: High Load Dyno (4th Direct Drive)** — 95% throttle, 90% dyno load in direct drive (1.00:1).
5. **TC-005: Clutch Slip Dynamics** — Throttle applied with clutch disengaged; quantify slip RPM.
6. **TC-006: Service Braking Dynamics** — High load braking deceleration and thermal dissipation.
7. **TC-007: Thermal Stabilization Transient** — Extended 35s run examining thermal equilibrium convergence.
8. **TC-008: Vibration vs Mechanical Wear** — Heuristic vibration amplitude under 85% backlash and misalignment.
9. **TC-009: Overdrive Gear Efficiency (5th Gear)** — Highway cruise assessing mechanical losses and BSFC.
10. **TC-010: High RPM Operating Limit** — Full throttle exploration approaching the 3,600 RPM redline.

* **Suite Actions**: Run individual tests, edit parameters, duplicate test cases, delete, or click **"Run All Tests"** for automated sequential execution.

---

## 📈 Data Studio & Export Capabilities

* **📋 Time-Series Table Explorer**:
  * 18 channels sampled per time step: Time, Engine RPM, Output RPM, Torque, Power, Fuel Rate, Cumulative Fuel, Gear Ratio, Overall Ratio, Efficiency, Coolant Temp, Exhaust Temp, Vibration RMS, Clutch Slip, Mechanical Loss, and Operating State.
  * Search, filter, and pagination (15, 25, 50, 100 rows per page).
* **📈 Multi-Test Comparison Chart**:
  * Multi-select completed tests to overlay their response curves on a single high-DPI canvas.
  * Compare any metric: Engine RPM, Output RPM, Torque, Power, Coolant, Exhaust, Fuel Rate, or Vibration.
* **📊 Statistical Summary & Provenance**:
  * Min, Max, Mean, Standard Deviation ($\pm\sigma$), and Final Readings for all metrics.
  * Data Provenance Matrix distinguishing manufacturer specifications, calculated outputs, and estimated heuristics.
* **💾 Multi-Format Export Center**:
  1. **CSV Telemetry**: Single test case time series with metadata headers and units.
  2. **Consolidated Batch CSV**: All completed test cases merged with test ID tags for Pandas / R / MATLAB.
  3. **JSON Dataset**: Hierarchical structure with metadata, benchmark specs, input vectors, statistics, and sample arrays.
  4. **Excel Workbook (`.xlsx`)**: Multi-sheet workbook via SheetJS including an *Executive Summary Matrix* and dedicated sheets for each test case.

---

## 🚀 How to Run Locally

### 1-Click Launch (Windows)
Double-click **`start_local_server.bat`** in this folder. It starts the local server and opens your browser to:
👉 **[http://localhost:8000](http://localhost:8000)**

### Manual Launch
```bash
python -m http.server 8000
```
Then navigate to `http://localhost:8000` in any modern web browser (Chrome, Edge, Firefox, Brave).

---

## 📊 Technical Reference

* **Engine Baseline**: Yanmar 2GM20F (0.636 L, 2-cylinder, 4-stroke marine diesel).
* **Continuous Rating**: 11.8 kW @ 3,400 RPM.
* **One-Hour Rating**: 13.4 kW @ 3,600 RPM.
* **Data Provenance**: Engine geometry and ratings reflect published specifications. Transmission gearset, thermal constants, and vibration curves are illustrative engineering models.
* **Source Manual**: [Yanmar GM-Series Operation Manual](https://www.yanmar.no/theme/yanmarportal/uploadedFiles/Marine/productDownloads/Leisure-Previous-Models/Operation-Manuals/GM-OP-Manual-Multi.pdf).

---

## 📄 License

MIT License. See [LICENSE](LICENSE) for details.
