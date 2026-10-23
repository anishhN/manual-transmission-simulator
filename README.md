# Transmission Dynamics Studio — Manual Transmission & Powertrain Simulator (PRO v3.0)

An interactive, browser-based 3D manual transmission and powertrain engineering simulator and virtual test bench built with a **Graphite & Black industrial aesthetic**, discrete transient rotational mechanics ($J_{eff} \frac{d\omega}{dt} = T_{ind} - T_{fric} - T_{load}$), energy-balance thermodynamics, an **8-point physical consistency validation engine**, and comprehensive engineering dataset export capabilities.

---

## 🌟 Key Features

### 1. Dual Operational Modes & Graphite UI
* **🏎️ 3D Live Cockpit**: Real-time interactive driving mode with live 3D cutaway powertrain, tactile gear selector gate, automotive pedal switchboard, 8 KPI telemetry metric cards, and live strip charts styled in a premium graphite, black, and metallic silver palette.
* **📊 Test Cases & Data Bench**: Comprehensive engineering test bench for running structured test suites, inspecting numerical time series with pagination, overlaying comparison curves, analyzing statistical distributions, validating physical integrity, and exporting data.

### 2. Interactive 3D Powertrain Cutaway (Three.js)
* **Procedural Gear Meshes**: Custom extruded 2D spur gears for 5 forward speeds and reverse.
* **Dynamic Digital Twin**: Real-time rotating crankshaft, dual-mass flywheel, clutch disc, input/output countershafts, and differential.
* **Kinematic Feedback**: Synchronizer sleeves visibly slide along the countershaft into the selected gear; active gear pairs highlight in satin metallic silver (`#C0C5CC`).
* **Studio Lighting**: Studio-grade neutral white key lighting (`0xFFFFFF`), cool grey rim lighting, and warm amber combustion glow.
* **Camera Controls**: Orbit controls (drag to orbit, pan, mouse wheel to zoom), auto-rotate, and view presets (*Isometric*, *Gearset Close-Up*, *Differential*, and *Reset View*).

### 3. Engineering-Grade Kinematics, Dynamics & Thermodynamics
* **Discrete Rotational Dynamics**: Numerically integrated rotational equation of motion:
  $$J_{eff} \frac{d\omega}{dt} = T_{ind}(\omega, \alpha) - T_{fric}(\omega) - T_{load,e}$$
  where $J_{eff} = J_{eng} + \frac{J_{veh}}{(i_g \cdot i_{fd})^2}$ when the clutch is locked.
* **Torque & Power Curves**: Piecewise torque curve calibrated to Yanmar 2GM20F continuous (11.8 kW @ 3,400 RPM / 33.1 N·m) and 1-hour ratings (13.4 kW @ 3,600 RPM / 35.5 N·m).
* **Friction & Idle Governor**: Quadratic engine friction model ($T_{fric} = c_0 + c_1 \omega + c_2 \omega^2$) coupled to a proportional-integral idle governor maintaining 700 RPM.
* **Clutch Friction Limit & Engine Stall**: Coulomb clutch capacity model with dynamic slip calculation; realistic engine stall triggers when load torque drops RPM below critical threshold ($< 350\text{ RPM}$).
* **Thermodynamics & Energy Balance**: Dynamic lumped-capacitance thermal model for coolant ($C_{cool} = 35\text{ kJ/K}$) and exhaust ($C_{exh} = 8\text{ kJ/K}$):
  $$C_{cool} \frac{dT_{cool}}{dt} = \dot{Q}_{cool,in} - UA_{rad}(T_{cool} - T_{amb})$$
* **Fuel & Emissions**: Instantaneous BSFC, volumetric consumption rate (L/h), cumulative consumption (L), and heat release via lower heating value ($42.7\text{ MJ/kg}$).
* **Model Configuration Modal**: Live tuning of physical constants (engine inertia $J_{eng}$, vehicle mass $m$, thermal capacities $C$, radiator conductance $UA$, diesel LHV) without code modification.

### 4. 8-Point Physical Consistency Validation Engine
Every simulation run automatically verifies physical conservation laws:
1. **First Law Thermal Conservation**: $\Delta E_{cool} \approx Q_{cool,in} - Q_{rad,out}$
2. **First Law Mechanical Conservation**: $\int P_{ind} dt \approx \Delta E_{kin} + W_{fric} + W_{load}$
3. **Gear Ratio Consistency**: $\omega_{in} / \omega_{out} = i_g$ when clutch is locked
4. **Second Law / Friction Non-Negativity**: $W_{fric} \ge 0$
5. **Thermodynamic Directionality**: $T_{cool} \ge T_{amb}$
6. **Efficiency Bound**: $0\% \le \eta \le 100\%$
7. **Fuel Mass & Chemical Energy Consistency**: $E_{chem} = m_{fuel} \cdot LHV$
8. **Clutch Energy Conservation**: $W_{slip} \ge 0$

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

## 🌐 Live Online Demo
The simulator is hosted live on GitHub Pages:
👉 **[https://anishhn.github.io/manual-transmission-simulator/](https://anishhn.github.io/manual-transmission-simulator/)**

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
