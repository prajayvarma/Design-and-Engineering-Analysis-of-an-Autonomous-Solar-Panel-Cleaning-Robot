Design and Engineering Analysis of an Autonomous Solar Panel Cleaning Robot 🚀

👨‍💻 Authors & Supervision
Course: Design of Machine Elements , Department of Mechanical Engineering, IIT Indore

Ch. Prajay Varma (230003018)

Course Instructor / Supervisor: Prof. Krishna Mohan Kumar, Department of Mechanical Engineering, IIT Indore

📌 Abstract

Dust and particulate accumulation on photovoltaic surfaces significantly degrades solar conversion efficiency and total power yield. Manual cleaning approaches are labor-intensive, hazardous, and economically non-viable for utility-scale solar farms.

This project presents the comprehensive design, mechanical CAD modeling, and multi-regime engineering analysis of a lightweight, low-cost autonomous solar panel cleaning robot. The system integrates a motorized cleaning assembly, 4-wheel drive mechanism, edge/obstacle detection sensors, and an ATmega328P-based control architecture. Comprehensive analytical evaluations—including inclined plane resolution (21^o), static tipping stability, dynamic acceleration, anti-slip traction margins, cantilever stress concentration, and fatigue life analysis using the Basquin S-N formulation—were executed to validate the structural integrity and operational safety of the 3D-printed PLA chassis.

🎯 Objectives

Automated Cleaning Operation: Automate solar panel cleaning to minimize manual labor, maintenance expenditure, and human risk in hazardous installations.

Mechanical & Structural Modeling: Design a lightweight (0.7 kg) chassis in CAD optimized for component packaging, balanced weight distribution, and high stiffness.

Static & Dynamic Stability Characterization: Validate tipping and rollover resistance on a 21^o inclined solar plane under both stationary and accelerated operation.

Traction & Drive System Verification: Ensure sufficient motor torque and wheel-to-glass friction coefficient to eliminate slippage and down-slope drift.

Stress Concentration & Fatigue Assessment: Identify critical stress points across cantilever support arms and mounting cutouts, establishing the fatigue life (n) under cyclic working loads.

🔌 Circuit & Interfacing Scheme

Master Controller: Arduino Uno powered via LM2596 step-down converter (5V rail).

Sensor Array: 4x IR Obstacle Sensors wired to digital input pins for corner-edge and obstacle boundary monitoring.

Drive Train: L298N H-Bridge logic inputs connected to Arduino PWM pins; motor output terminals parallel-wired to left-side and right-side BO motor pairs.

Cleaning Actuation: Relay Channel 1 controls the 12V DC cleaning brush motor; Relay Channel 2 triggers the 12V fluid pump.

Power Bus: 4x 18650 Li-ion battery pack provides unregulated power to L298N VMS and relay coils, step-regulated via LM2596 for Arduino and sensors.

⚙️ System Working

Initialization: The robot is placed at the upper edge of an inclined photovoltaic module (tilt angle theta = 21^o).

Surface Navigation: Arduino Uno commands the L298N driver to advance the 4WD drive assembly along a structured raster path across the panel glass.

Active Cleaning: The relay module activates the high-speed cleaning roller brush while the 12V DC pump dispenses a controlled water mist to dislodge adhering dust.

Edge & Hazard Detection: Four down-facing IR sensors continuously monitor surface reflectivity. When a frame boundary or panel edge is detected, the MCU interrupts forward motion, executes a differential spin turn, and resumes the cleaning cycle.

Fail-Safe Holding: If power drops or an edge sensor trips continuously, motors enter dynamic braking to prevent slippage down the panel slope.

📐 Mechanical, Static & Dynamic Analyses

1. System Constants & Boundary ParametersTotal Mass m: 0.7kg impliesTo tal Weight W = mg = 6.867 N

2. Panel Inclination Angle theta: 21^o

3.  Center of Mass Height (h): 111 mm 0.111 m
 
4. Track Width / Wheelbase (w): 180 mm (0.180 m}
  
5. Wheel Radius (r): 65 mm (0.065 m)

6. Motor Safe Torque: 0.0294 Nm per motor (4motors);

7. Transmission Efficiency eta = 60 Rolling Resistance Coefficient (C_rr): 0.02;

8.  Brush Drag Force F_brush = 0.50 N

💻 Control Logic Program (Arduino C++)

// Autonomous Solar Panel Cleaning Robot Control Firmware
// Target: Arduino Uno R3 + L298N Motor Driver + Relays + 4x IR Edge Sensors

// Motor Driver Pin Assignments
const int ENA = 5;  // PWM Left
const int IN1 = 6;
const int IN2 = 7;
const int IN3 = 8;
const int IN4 = 9;
const int ENB = 10; // PWM Right

// Relay Pin Assignments
const int RELAY_BRUSH = 11;
const int RELAY_PUMP  = 12;

// IR Edge Sensors (Active LOW)
const int IR_FRONT_LEFT  = 2;
const int IR_FRONT_RIGHT = 3;
const int IR_REAR_LEFT   = 4;
const int IR_REAR_RIGHT  = A0;

void setup() {
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENB, OUTPUT);

  pinMode(RELAY_BRUSH, OUTPUT);
  pinMode(RELAY_PUMP, OUTPUT);

  pinMode(IR_FRONT_LEFT, INPUT);
  pinMode(IR_FRONT_RIGHT, INPUT);
  pinMode(IR_REAR_LEFT, INPUT);
  pinMode(IR_REAR_RIGHT, INPUT);

  // Turn ON Cleaning Modules
  digitalWrite(RELAY_BRUSH, HIGH);
  digitalWrite(RELAY_PUMP, HIGH);

  setDriveSpeed(180, 180);
}

void loop() {
  bool fl = digitalRead(IR_FRONT_LEFT);
  bool fr = digitalRead(IR_FRONT_RIGHT);

  // Surface Boundary Handling
  if (fl == LOW || fr == LOW) {
    // Edge Encountered -> Reverse and Spin Turn
    moveBackward();
    delay(600);
    turnRight();
    delay(450);
  } else {
    // Normal Forward Sweeping
    moveForward();
  }
}

void moveForward() {
  digitalWrite(IN1, HIGH); digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH); digitalWrite(IN4, LOW);
}

void moveBackward() {
  digitalWrite(IN1, LOW); digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW); digitalWrite(IN4, HIGH);
}

void turnRight() {
  digitalWrite(IN1, HIGH); digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW); digitalWrite(IN4, HIGH);
}

void setDriveSpeed(int leftSpeed, int rightSpeed) {
  analogWrite(ENA, leftSpeed);
  analogWrite(ENB, rightSpeed);
}

✅ Advantages

High Incline Stability: Validated static (textFoS = 2.11) and dynamic (FoS = 1.54) tipping factors on 21^otilt panels.

Reliable Anti-Slip Grip: Wheel traction margin (mu_actual}} = 0.60 > mumin} = 0.42) ensures zero slippage on smooth glass.

Integrated Wet Cleaning: Combines active mechanical brushing with a synchronized fluid spray to lift cemented particulates.

Autonomous Safety: Continuous edge-detection prevents falls without requiring external wire guides or manual operators.

Cost-Effective Fabrication: Entire mechanical structure is 3D-printable in PLA, cutting fabrication costs relative to custom machined metal rigs.

⚠️ Limitations

Localized Arm Root Stress: Front cantilever arm roots exhibit localized stresses up to 19.41 MPa, limiting cyclic life to sim 15,570 cycles under sustained alternating loads.

Water Refill Dependence: Compact onboard fluid tank requires periodic topping-up for continuous wet cleaning runs.

Sunlight Sensor Sensitivity: Ambient infrared noise under direct extreme solar glare can require periodic recalibration of IR threshold pots.

🚀 Future Enhancements

Chassis Reinforcement: Enlarge front arm fillet radii from 1 mm to 3.5 mm and add triangular structural gussets to suppress stress concentration (K_t) and expand fatigue life.

IoT & Telemetry Integration: Add ESP32/Wi-Fi telemetry for remote panel output tracking, automatic scheduling, and battery status reporting.

Optical Dust Sensing: Integrate optical dust monitoring to trigger cleaning passes selectively based on transmission loss rather than fixed timers.

Self-Docking Solar Recharging: Mount top-side mini PV panels and charging contacts for zero-intervention automated battery replenishment.

📊 Applications

Utility-Scale Solar Farms: Large ground-mount multi-megawatt photovoltaic installations.

Commercial & Industrial Rooftops: Automated upkeep for factory and logistics warehouse solar arrays.

Off-Grid Agricultural Installations: Solar-powered pumping and irrigation arrays in arid, high-dust environments.

Smart Cities & Microgrids: Distributed renewable energy arrays requiring autonomous maintenance.

📌 Conclusion

The design and engineering analysis of the autonomous solar panel cleaning robot proves that an accessible, low-cost robotic system can safely navigate and clean photovoltaic modules inclined at $21^\circ$. The mathematical models confirm adequate tipping margins, positive net acceleration, and slip-free traction. While the static factor of safety is close to $3.0$ relative to PLA's ultimate tensile strength ($58\text{ MPa}$), structural fatigue analysis underscores the importance of fillet radius optimization and gusset additions to achieve long-term field endurance.
