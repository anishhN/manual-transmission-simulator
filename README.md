# Transmission Dynamics Studio — Generator & Powertrain Simulation Platform (PRO v3.5)

An advanced, browser-based 3D manual transmission and synchronous generator powertrain engineering simulator and virtual test bench. Built with a **Graphite & Black industrial aesthetic**, discrete transient rotational mechanics ($J \frac{d\omega}{dt} = T_{drive} - T_{load} - T_{friction}$), connected drivetrain dynamics, progressive clutch and brake controllers, an **automated 12-point physical consistency validation engine**, and a dedicated **14-Channel Live Performance Graphs Studio**.

---

## 🌟 Key Architecture & Upgrades (v3.5)

### 1. Fix of Random RPM Fluctuations & Decoupled Architecture
* **Fixed Physics Timestep**: Runs a discrete, fixed 10 ms timestep ($\Delta t = 0.01\text{ s}$, 100 Hz simulation rate) using an accumulator in `requestAnimationFrame`.
* **Zero Synthetic Randomness**: Completely removed random-number generation from all mechanical equations.
* **Separation of Concerns**: UI input events (slider dragging, pedal clicks) update target demands only—never advancing the physics clock or mutating state out-of-sequence.
* **Steady-State Stability**: Under constant throttle, load, gear, clutch, and brake conditions, derivative $\frac{d\omega}{dt} = 0$, guaranteeing rock-solid stability with $0.00\text{ RPM}$ fluctuation.

### 2. Engine Operating Limits & 4 Distinct Shafts
* **Idle Speed Regulation**: Proportional-integral (PI) governor actively regulating unloaded engine speed to **1,000 RPM**.
* **Operating Redline & Rev Limiter**: Realistic soft limiter ramp starting at 5,250 RPM with hard fuel cut at **5,400 RPM**.
* **4 Independent Rotational Shafts**:
  1. **Engine Crankshaft ($\omega_{eng}$ / $\text{RPM}_{eng}$)**: Driven by combustion indicated torque and idle governor.
  2. **Gearbox Input Shaft ($\omega_{in}$ / $\text{RPM}_{in}$)**: Clutch driven friction plate side.
  3. **Gearbox Output Countershaft ($\omega_{out}$ / $\text{RPM}_{out}$)**: Driven via active gear pair ratio ($i_{gear}$).
  4. **Generator Rotor Shaft ($\omega_{gen}$ / $\text{RPM}_{gen}$)**: Coupled via final drive ratio ($i_{fd}$): $\omega_{gen} = \omega_{out} \cdot i_{fd}$.

### 3. Realistic Progressive Clutch Dynamics
* **Progressive Engagement**: Releasing the clutch pedal initiates a smooth $2.5\text{ s}$ engagement transition ($k_{clutch} \in [0, 1]$); pressing initiates smooth disengagement over $1.2\text{ s}$.
* **Coulomb Friction Model**: Torque capacity is bounded by clamping force:
  $$T_{cap} = k_{clutch} \cdot T_{clutch,max} \quad (T_{clutch,max} = 120\text{ N}\cdot\text{m})$$
* **Equal & Opposite Slip Torque**:
  $$T_{clutch} = \text{clamp}\left((\omega_{eng} - \omega_{in}) \cdot 35.0, \, -T_{cap}, \, T_{cap}\right)$$
  Acting in equal and opposite directions on the engine and transmission input shafts.
* **Locked Drive**: When slip speed $\Delta\omega < 0.2\text{ rad/s}$ and $k_{clutch} \ge 0.98$, shafts lock into synchronous drive.

### 4. Realistic Progressive Service Brake
* **Ramp-Up & Release**: Braking force builds smoothly over $1.5\text{ s}$ and releases over $1.2\text{ s}$.
* **Opposing Braking Torque**:
  $$T_{brake} = k_{brake} \cdot T_{brake,max} \cdot \tanh(\omega_{out} / 0.5) \quad (T_{brake,max} = 180\text{ N}\cdot\text{m})$$
  Acts strictly as a resisting torque on the output shaft opposing rotation, avoiding zero-speed chattering or negative RPM.

### 5. Gear-Shifting State Machine & Momentum Continuity
* **Synchromesh State Machine**: Shifts progress through defined states:
  $$\text{LOCKED} \;\longrightarrow\; \text{DISENGAGING} \;\longrightarrow\; \text{SHIFTING (Neutral)} \;\longrightarrow\; \text{SYNCHRONIZING} \;\longrightarrow\; \text{RE-ENGAGING} \;\longrightarrow\; \text{LOCKED}$$
* **Momentum Continuity**: Engine and drivetrain rotational inertias ($J_{eng}$, $J_{driveline}$) are preserved during shifts. Engine RPM never drops to zero or resets arbitrarily during gear changes.

### 6. Synchronous Generator Electrical Model
* **Frequency**: Modeled for a 4-pole synchronous generator:
  $$f = \frac{p \cdot \text{RPM}_{gen}}{120} = \frac{\text{RPM}_{gen}}{30}\text{ Hz} \quad (50.0\text{ Hz at } 1,500\text{ RPM})$$
* **Terminal Voltage (AVR)**: Automatic Voltage Regulation maintaining $400\text{ V}$ nominal 3-phase AC.
* **Load Resisting Torque**: Electrical power demand reflects back to the prime mover:
  $$T_{gen} = \frac{P_{gen,mech}}{\omega_{gen}} = \frac{P_{elec} / \eta_{gen}}{\omega_{gen}}$$

### 7. Exhaust Gas Temperature (EGT) & Coolant Thermodynamics
* **Dynamic Lumped-Capacitance EGT**:
  $$C_{egt} \frac{dT_{egt}}{dt} = \dot{Q}_{egt,in} - h_{exh}(T_{egt} - T_{amb})$$
  Calibrated thermal time constant $\tau \approx 5.0\text{ s}$ responding smoothly to fuel rate, load, and speed ($140^\circ\text{C}$ to $650^\circ\text{C}$).
* **Coolant Thermal Balance**: $C_{cool} = 35.0\text{ kJ/K}$ responding to radiator dissipation and water pump circulation.

### 8. Mechanical Shaft Power Formulation
* **Consistent Power Formulation**:
  $$P = T \cdot \omega \quad \Longleftrightarrow \quad P_{\text{kW}} = \frac{T_{\text{N}\cdot\text{m}} \cdot \text{RPM}}{9549.3}$$
  Separately evaluated for Engine Shaft, Gearbox Output, and Generator Mechanical Input.

---

## 📈 14 Live Performance Graphs Studio

A dedicated **Performance Graphs Studio** tab featuring high-DPI interactive canvases, time window zooming, pause/clear controls, tooltip crosshairs, and statistics:

1. **Engine & Drivetrain RPM vs. Time**: Crankshaft, Gearbox In, Gearbox Out, Generator Rotor.
2. **Engine RPM vs. Engine Coolant Temperature**: State-space trajectory paired at identical timestamps.
3. **Engine RPM vs. Exhaust Gas Temperature (EGT)**: State-space trajectory paired at identical timestamps.
4. **Shaft Power vs. Time**: Engine Brake Power, Gearbox Output Power, Electrical Output Power.
5. **Shaft Torque vs. Time**: Engine Delivered Torque, Output Torque, Generator Resisting Torque.
6. **Generator Shaft Speed & Frequency vs. Time**: RPM & AC Frequency (Hz).
7. **Generator Output Voltage vs. Time**: 3-Phase AC Line Voltage (V).
8. **Generator Output Current vs. Time**: 3-Phase AC Line Current (A).
9. **Electrical Output Power vs. Time**: Net electrical power delivered (kW).
10. **Engine Coolant Temperature vs. Time**: Cylinder head & jacket thermal history (°C).
11. **Exhaust Gas Temperature (EGT) vs. Time**: Manifold EGT thermal history (°C).
12. **Mechanical Vibration RMS vs. Time**: Rotational imbalance & harmonic diagnostic (mm/s).
13. **Fuel Consumption vs. Time**: Volumetric rate (L/h) and cumulative consumption (L).
14. **Mechanical & Generator Efficiency vs. Load**: Drivetrain $\eta_{trans}$ & generator $\eta_{gen}$ (%).
15. **Composite Overview Matrix**: Multi-channel live monitoring overview.

---

## 🧪 Automated 12-Point Physical Consistency Engine

Every simulation and test run automatically verifies fundamental mechanical and thermodynamic laws:
1. **Kinematic Gear Speed Matching**: $\omega_{out} = \omega_{in} / i_{gear}$ when clutch is locked.
2. **Shaft Power Formula**: $|P_{kW} - (T \cdot \text{RPM} / 9549.3)| < 0.05\text{ kW}$.
3. **Driveline Passive Efficiency Limit**: $P_{out} \le P_{in}$ ($\eta \le 100\%$).
4. **Generator Electrical Conversion Limit**: $P_{elec} \le P_{gen,mech}$ ($\eta_{gen} \le 100\%$).
5. **Clutch Action / Reaction Balance**: $T_{clutch,eng} = -T_{clutch,in}$ (Newton's 3rd Law).
6. **Brake Motion Opposition**: $T_{brake} \cdot \omega_{out} \ge 0$ (braking torque resists motion).
7. **Steady-State Stability**: $d\text{RPM}/dt \approx 0$ under constant inputs (zero numerical noise).
8. **Idle Governor Regulation Target**: Engine stabilizes at $1,000 \pm 5\text{ RPM}$ when unloaded.
9. **Maximum Rev Limiter Bound**: Engine speed strictly constrained $\le 5,400\text{ RPM}$.
10. **Gear Shift Momentum Continuity**: No instantaneous speed drop to zero during gear changes.
11. **Second Law Thermal Directionality**: $T_{cool} \ge T_{amb}$ and $T_{egt} \ge T_{amb}$.
12. **Fuel Combustion Energy Balance**: $E_{chem} = m_{fuel} \cdot LHV$.

---

## 🚀 How to Run Locally

### 1-Click Launch (Windows)
Double-click **`start_local_server.bat`** in the repository root. It starts a local server and opens your default browser at:
👉 **[http://localhost:8000](http://localhost:8000)**

### Manual Launch
```bash
python -m http.server 8000
```
Then navigate to `http://localhost:8000`.

---

## 🌐 Live Online Demo
The simulator is hosted live on GitHub Pages:
👉 **[https://anishhn.github.io/manual-transmission-simulator/](https://anishhn.github.io/manual-transmission-simulator/)**

---

## 📄 License

MIT License. See [LICENSE](LICENSE) for details.
