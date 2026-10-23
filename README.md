# Transmission Dynamics Studio — Manual Transmission & Powertrain Simulator

An interactive browser-based 3D manual transmission and powertrain engineering simulator. It models the kinematics, thermodynamics, and mechanical dynamics of a manual transmission coupled to a diesel engine modeled after the published specifications of the **Yanmar 2GM20F**.

![Screenshot](screenshot.png) *(optional preview)*

---

## 🌟 Features

* **Interactive 3D Powertrain Cutaway (Three.js)**:
  * Procedurally generated spur gears for 5 forward speeds and reverse.
  * Real-time rotating crankshaft, flywheel, clutch assembly, input/output shafts, and differential.
  * Synchronizer sleeve and collar movement matching the active gear.
  * Orbit camera controls (drag to orbit, mouse wheel to zoom, reset view, and auto-rotate).
* **Kinematics & Dynamics Model**:
  * Engine RPM transient response with load-dependent speed curves.
  * Piecewise torque and power curves derived from the continuous and 1-hour ratings of the Yanmar 2GM20F.
  * Realistic gear multiplication: 1st (3.60), 2nd (2.10), 3rd (1.40), 4th (1.00), 5th (0.80), Reverse (3.30).
  * Configurable Final Drive ratio (2.00:1 to 5.00:1).
  * Gear mesh efficiency degradation modeled against mechanical wear.
* **Thermodynamics & Combustion**:
  * Lumped first-order transient thermal response for coolant and exhaust gas temperatures.
  * Fuel consumption (L/h) and Brake Specific Fuel Consumption (BSFC in g/kWh).
  * Fuel chemical heat input based on diesel Lower Heating Value (LHV).
* **Vibration & Diagnostics**:
  * Heuristic vibration amplitude (mm/s RMS) responding to speed, load, clutch disengagement, and wear.
  * Clutch slip speed calculation (RPM).
* **Telemetry & Live Charts**:
  * Real-time rolling strip charts for Engine RPM vs. Output RPM.
  * Thermal and vibration trend visualization.
  * 1-click CSV data export for lab analysis.

---

## 🚀 How to Run

1. Clone or download this repository.
2. Open `manual_transmission_simulator.html` in any modern web browser (Google Chrome, Microsoft Edge, Firefox, Brave).
   * *Note: Internet access is required on first launch to load the Three.js 3D library via CDN.*
3. Alternatively, serve locally:
   ```bash
   python -m http.server 8000
   ```
   Then navigate to `http://localhost:8000/manual_transmission_simulator.html`.

---

## ⚙️ Driver Controls

* **Gear Selector**: Switch between Neutral, 1st–5th gear, and Reverse.
* **Pedals**:
  * **Clutch**: Press to decouple engine from transmission (essential for smooth shifts).
  * **Brake**: Apply load to output shaft.
  * **Ignition**: Start or shut down the engine.
* **Sliders**: Throttle, Applied Load, Ambient Temperature, Mechanical Wear/Misalignment, and Final Drive Ratio.

---

## 📊 Technical Reference

* **Engine Baseline**: Yanmar 2GM20F (0.636 L, 2-cylinder, 4-stroke marine diesel).
* **Continuous Rating**: 11.8 kW @ 3400 RPM.
* **One-Hour Rating**: 13.4 kW @ 3600 RPM.
* **Data Provenance**: Engine geometry and ratings reflect published specifications. Transmission gearset, thermal constants, and vibration curves are illustrative engineering models.

---

## 📄 License

MIT License. See [LICENSE](LICENSE) for details.
