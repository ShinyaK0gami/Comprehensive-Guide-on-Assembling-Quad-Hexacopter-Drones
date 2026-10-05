# <center>Comprehensive Drone Assembly and Pre-Flight Guide: F450 Quadcopter & F550 Hexacopter</center>
### <center>By M.Anas Baig (23ES020), Usman Zubair (22ES025) & Daniyal Arain (22ES071) Supervisor: Abbas Shah Syed</center>

This manual provides structural instructions for building, wiring, and configuring multirotors based on the **F450 Quadcopter** and **F550 Hexacopter** chassis configurations using a **Pixhawk 2.4.8** flight controller and **Mission Planner** ground station software.

---

## 🛠️ Step 1: Soldering the Main Frame Power Distribution Board (PDB)
The lower glass-fiber plate on both the F450 and F550 frames functions as an integrated Power Distribution Board (PDB).

1. **Setup:** Place the bottom frame plate on a heat-safe workspace with the gold-plated solder tracks facing upward.
2. **Main Power Lead:** Solder a male XT60 connector wire (or main high-current battery cable) directly to the central large **+** (positive) and **-** (negative) solder pads.
3. **ESC Power Lines:** Strip and solder the positive (red) and negative (black) power leads of your Electronic Speed Controllers (ESCs) directly to the respective **+** and **-** terminal pads at the extremities of the frame plate.
   * **F450 Quadcopter:** Solder **4 ESCs** to the 4 corner pads.
   * **F550 Hexacopter:** Solder **6 ESCs** to the 6 perimeter pads.
4. **Quality Control:** Inspect all joints with a magnifying lens or multimeter continuity check. Ensure no cold solder connections or short circuits exist between positive rails and ground.

---

## ⚙️ Step 2: Motor and Structural Frame Assembly

1. **Motor Mounting:** Secure your brushless DC motors (e.g., A2212 1000kV) to the mounting slots at the end of each frame arm using the provided M3 hex screws. Apply low-strength thread locker if available.
2. **Arm Orientation & Frame Fastening:** Fasten the arms to the bottom plate using the M2.5 frame screws according to structural orientation rules:
   * **F450 Quadcopter Layout:** 
     * Position **2 Red Arms** at the front (indicating forward flight orientation).
     * Position **2 White Arms** at the rear.
   * **F550 Hexacopter Layout:** 
     * Position **2 Red Arms** directly facing the forward-most vector (front flight path).
     * Position **4 White/Black Arms** along the remaining sides and rear to complete the hexagram footprint.
3. **Landing Gear:** Secure the landing gear brackets safely to the underside of the frame structure or lower plate mounting points.

---

## 🔌 Step 3: ESC Mounting and Motor Phase Wiring

1. **Chassis Securing:** Secure each 40A Brushless ESC tightly underneath or along the vertical profiles of its respective arm using heavy-duty zip ties. Ensure they do not block propeller downwash completely.
2. **Three-Phase Connection:** Connect the 3 bullet leads coming out of each brushless motor to the corresponding 3 output bullet receptacles on the ESC.
   * *Note:* If any motor spins backward during software checkouts later, swap **any two of these three bullet wires** to invert the phase rotation.

---

## 🛰️ Step 4: Top Plate & Flight Controller Structural Installation

1. **Chassis Closure:** Secure the top frame plate over the arm alignment pins using the remaining frame screws to complete the structural cage.
2. **Damping Platform:** Mount an anti-vibration damping platform or high-density double-sided foam tape precisely at the **geometric center** of the top frame plate. This is vital to isolate the internal IMU/gyroscopes from motor frequencies.
3. **Pixhawk Orientation:** Adhere the **Pixhawk 2.4.8** flight controller onto the vibration mount.
   * **CRITICAL ORIENTATION:** Ensure the physical embossed arrow on top of the Pixhawk casing points directly forward (between the two front red arms).

---

## 🗺️ Step 5: Peripheral Wiring & Motor Channel Layout Mapping

Wire your electronic peripherals to the Pixhawk rails based on your specific multirotor frame type selected in Mission Planner:

### Multirotor Servo Output Mappings

| Motor Position | F450 Quadcopter (Quad-X Layout) | F550 Hexacopter (Hexa-X Layout) |
| :--- | :--- | :--- |
| **Motor 1** | MAIN Out Channel 1 (Front-Right, CCW) | MAIN Out Channel 1 (Front-Right, CCW) |
| **Motor 2** | MAIN Out Channel 2 (Rear-Left, CCW) | MAIN Out Channel 2 (Rear-Left, CW) |
| **Motor 3** | MAIN Out Channel 3 (Front-Left, CW) | MAIN Out Channel 3 (Middle-Left, CCW) |
| **Motor 4** | MAIN Out Channel 4 (Rear-Right, CW) | MAIN Out Channel 4 (Rear-Right, CCW) |
| **Motor 5** | — | MAIN Out Channel 5 (Front-Left, CW) |
| **Motor 6** | — | MAIN Out Channel 6 (Middle-Right, CW) |

### Peripheral Component Wiring Connections
* **Radio Receiver:** Connect the PPM/SBUS single-line output cable from your RC receiver into the designated **RC IN** port on the Pixhawk servo rail.
* **GPS & Compass Mount:** Secure the Readytosky M10 GPS pedestal module onto one of the rear frame arms to distance it from high-current EMI fields. Plug the GPS sensor cable into the **GPS** port and the external compass cable into the **I2C** port.
* **Inline Power Module:** Connect the Power Module inline between your LiPo battery connector and the PDB XT60 lead. Plug the 6-pin telemetry monitoring cable directly into the Pixhawk's **POWER** port.

---

## 💻 Step 6: Software Pre-Flight Calibration (PROPELLERS REMOVED)

⚠️ **SAFETY WARNING: Do NOT mount propellers on the motors under any circumstances during initial programming and calibration routines.**

1. **GCS Link:** Interconnect the Pixhawk to your computer via a robust USB cable and boot **Mission Planner**. Set firmware parameters to Pixhawk 1 defaults.
2. **IMU Calibration:** Navigate to *Initial Setup > Compass/Accel Calibration*. Perform the full multi-axis Accelerometer Calibration by positioning the drone frame on all 6 axes when prompted.
3. **Compass Priority Setup:** Calibrate your internal and external compasses. Set compass priorities depending on which sensor is designated primary in your hardware tree. 
   * *Internal GPS/Compass Mapping:* Communicates via **I2C** protocol.
   * *External GPS/Compass Mapping:* Communicates via **SPI / Serial** bus profiles.
4. **Radio Transmitter Setup:** Turn on your transmitter. Go to *Radio Calibration* and move all sticks to their limits to establish absolute minimum and maximum PWM endpoints.
5. **ESC Sync Calibration:** Execute a comprehensive ESC calibration routine through Mission Planner to ensure all ESC throttle curves synchronize evenly across the digital servo rail.
6. **Motor Spin Check:** Connect the LiPo test battery. Use the Motor Test interface in Mission Planner to spin each motor at a low threshold (e.g., 5-10%) to verify correct directions:
   * **F450 Rotations:** Motor 1 (CCW), Motor 2 (CCW), Motor 3 (CW), Motor 4 (CW).
   * **F550 Rotations:** Swap wires on any phase where rotations mismatch the standard Hexa-X compass vectors noted in the mapping table.

---

## 🔋 Step 7: Flight Readiness Preparation

1. **Propeller Installation:** Mount your matching 1045 or 9450 multirotor propellers using self-tightening caps or prop adapters. 
   * Ensure text markings face **UP**.
   * Match CW props to CW spinning motors, and CCW props to CCW spinning motors.
2. **Battery Securement:** Center the 3S LiPo 11V 4200mAH battery tightly on the lower frame or battery tray using hook-and-loop straps. Ensure the physical Center of Gravity (CoG) lands squarely at the exact center of the chassis footprint to prevent motor load imbalances.
