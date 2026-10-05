# <center>Technical Testing Log: System Glitches, Parameter Fixes, and RC Control Profiles</center>
### <center>By M.Anas Baig (23ES020), Usman Zubair (22ES025), Daniyal Arain (22ES071) & Prof. Abbas Shah Syed</center>
This document details the software bugs, sensor adjustments, parameter configurations, and testing profiles recorded during bench optimization of the **F450 Quadcopter** and **F550 Hexacopter** configurations.

---

## ⚡ Critical Power & Battery Safety Thresholds

The telemetry system utilizes an inline monitoring module to protect the LiPo cell packs from over-discharge degradation.

* **Minimum Safety Floor:** **`10.5 V`**
* **Automated Failsafe Behavior:** If the 3S LiPo battery pack drops below this voltage parameter during flight, the Pixhawk will trigger a low-battery alarm event and command an autonomous **Return-to-Launch (RTL)** sequence along its pre-calculated navigation path.
* **GCS Parameters Configuration:** These boundary profiles can be audited or adjusted inside the Mission Planner Full Parameter List via:
  * `BATT_CRT_VOLT` (Critical voltage threshold value)
  * `BATT_LOW_VOLT` (Low warning voltage threshold value)

---

## 🔧 Hardware Glitches & Parameter Override Log

During initial bench configuration and flight preparation, the following systemic issues were identified and resolved using specific firmware overrides:

### 1. Defective Voltage/Current Sensor Glitch
* **Symptom:** Flight controller throws continuous errors or readings regarding cell states despite a healthy, balanced battery pack.
* **Root Cause:** A hardware component failure or calibration drift inside the inline power module sensing circuitry.
* **Applied Solution:** Swapped the primary Pixhawk 2.4.8 flight controller unit with a verified working, identical reference board to re-establish clean internal sensor readouts.

### 2. Barometer Discrepancy Error (`PreArm: Baro: GPS alt error 5802m`)
* **Symptom:** The safety system blocks arming commands, warning of an altitude reading variance of several thousand meters between sensors.
* **Root Cause:** A configuration mismatch where the internal barometric sensor data strongly conflicts with the 3D-Fix altitude metric delivered by the Readytosky M10 external module.
* **Applied Resolution Protocol:**
  1. Perform a complete, clean rerun of the multi-satellite GPS calibration matrix.
  2. Enter the **Full Parameter List** inside Mission Planner's configuration environment.
  3. Search for barometric indexing parameters and locate **`BARO_ALTER_OPTIONS`** (or firmware equivalent). Modify its parameter value to **`1`**.
  4. Override structural checking logic by explicitly setting **`BARO_OP`** to **`0`**.
  5. Perform a full manual power cycle / physical reset of the Pixhawk board to write parameters into permanent EEPROM storage.

### 3. Non-Simultaneous Motor Arming Initialization
* **Symptom:** When adding throttle inputs, some motors fail to spin up at the same time, leading to dangerous tips or yaw drift during takeoffs.
* **Root Cause:** Small variations in factory ESC deadband settings, or insufficient startup electrical power delivered to overcome motor friction.
* **Applied Resolution Protocol:**
  1. Disconnect propellers and calibrate the low-end throttle ranges of all ESCs/motors individually while tracking the diagnostic readout screen.
  2. If staggered startups persist, navigate to *Config/Tuning > Full Parameter List*.
  3. Locate the parameter **`MOT_SPIN_ARM`**.
  4. Increase the value slightly—shifting it from its default **`0.10` up to `0.15`**. This elevates the baseline idle current passed to all channels upon arming, forcing synchronous motor ignition.

### 4. Direct Throttle Response Failure
* **Symptom:** The RC transmitter throttle stick shows standard behavior in calibration screens, but physical motor outputs do not respond to channel inputs.
* **Root Cause:** Signal routing layout anomalies within default channel function matrices on custom hardware configurations.
* **Applied Resolution Protocol:** Go to the **Full Parameter List** and remap output channel profiles precisely as follows:
  * Change parameter **`SERVO1_FUNCTION`** to **`4`** (Maps to Aileron/Roll behavior).
  * Change parameter **`SERVO3_FUNCTION`** to **`70`** (Maps explicitly to Throttle command loops).

---

## 🎮 Transmitter Stick Control Values & Operational Behavior

The radio control link operates across specialized PWM (Pulse Width Modulation) threshold windows during manual live-testing procedures:

### Manual Arming Execution
To arm the flight controller, hold the sticks for **RC1 (Roll), RC2 (Pitch), and RC4 (Yaw)** down into their absolute **minimum** positions simultaneously. Once the Pixhawk LED switches to an armed status indication, immediately return the Yaw stick back to its spring-loaded neutral midpoint.

### Throttle (RC3) Response Metrics
* **Absolute Maximum Boundary:** `1976` PWM value.
* **Motor Spin Initialization Zone:** Motors activate and spin at idle when the RC3 throttle value reaches **`1400`** PWM.
* **Thrust Linear Curve Injection:** Motor RPM escalates predictably once the throttle input crosses the **`1600`** PWM line.
* **Deceleration Mapping Threshold:** Motors slow down to minimum idle rates when throttle inputs are dropped to **`1300`** PWM.

### Complete Motor Shutdown Routine
To turn off motor outputs during ground operations, drop the **RC3** throttle stick completely below the **`1600`** PWM threshold window while actively pulling the **RC2 (Pitch)** stick down away from its neutral mean center position.
