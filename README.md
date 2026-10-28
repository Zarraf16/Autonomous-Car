# Dual-Mode F1 RC Car

**Arduino control • PID line following • Pixy2 vision • Custom PCB**

A Schoolcraft Engineering Club vehicle project combining manual RC operation with autonomous line following and traffic-light detection. This portfolio fork preserves the team's source history and adds engineering documentation.

## Engineering highlights

- **6 IR sensors** provide a weighted steering error for the PID controller.
- **Pixy2 color signatures** identify red and green traffic-light targets.
- **Arduino Nano, steering servo, and ESC** form the autonomous control system.
- A **2-layer custom PCB** integrates power and signal connections within the chassis.

## PCB design contribution

I contributed PCB design in EasyEDA as part of the Schoolcraft Engineering Club team: schematic capture, component placement, routing, and manufacturing preparation. The board measures **44.58 × 23.62 mm** and connects the controller, sensor array, steering servo, and ESC interfaces. Ground stitching vias connect the copper pours. The [engineering report](F1_RC_Car_Engineering_Report.pdf) documents the PCB and the wider team project.

[View my PCB schematic, layouts, and assembly previews](docs/README.md)

![My Arduino Nano interface PCB schematic](docs/pcb/schematic.png)

Vehicle design, firmware, mechanical work, integration, and testing were collaborative. This fork does not imply sole authorship of the original code.

## How the autonomous mode works

The sketch reads the six IR channels, calculates a weighted error, and applies PID steering with **Kp = 12.0, Ki = 0.1, Kd = 0.8**. It limits the integral and filters the derivative before constraining steering to 20–160 degrees.

Pixy2 checks color signatures **1 (red)** and **3 (green)**. The current implementation acts on the first recognized red/green block and permits motion when neither is found. The ESC alternates forward and neutral commands during movement. Neutral is an ESC command, not a guaranteed braking distance.

Manual/autonomous changeover was performed manually; this sketch does not implement automatic mode switching.

## Repository guide

| Path | Purpose |
|---|---|
| [AutonomousDrivingCode](AutonomousDrivingCode/) | Integrated autonomous driving sketch |
| [libraries/IrSensorArray](libraries/IrSensorArray/) | Six-sensor validity and weighting logic |
| [IRSensorTest](IRSensorTest/) | Sensor and PID steering test |
| [ESCMotorTest](ESCMotorTest/) | ESC command test |
| [PixyTest](PixyTest/) | Traffic-light signature test |
| [ServoTest](ServoTest/) | Steering servo test |
| [TrafficLightCode](TrafficLightCode/) | Separate red/green test-light controller |
| [PCB design documentation](docs/README.md) | My EasyEDA schematic, PCB routing, copper pours, assembly previews, and pin-revision notes |
| [Engineering report](F1_RC_Car_Engineering_Report.pdf) | Hardware, PCB design, testing, photos, and complete main-code listings |

## Setup

1. Install Arduino IDE with support for the classic ATmega328P Arduino Nano.
2. Install the **Pixy2** and **Servo** libraries.
3. Copy `libraries/IrSensorArray` into your Arduino sketchbook's `libraries` directory and restart the IDE.
4. Open `AutonomousDrivingCode/AutonomousDrivingCode.ino`.
5. Select the correct Nano board, processor/bootloader, and port. Verify the wiring below and your ESC calibration.
6. Train Pixy2 for the red/green targets and test the individual sketches before running the integrated vehicle. Perform initial motor tests with the driven wheels lifted.

| Interface | Main-sketch pin |
|---|---|
| Steering servo | A0 |
| ESC signal | A4 |
| Left IR sensors 1 / 2 / 3 | A5 / A6 / A7 |
| Right IR sensors 1 / 2 / 3 | A3 / A2 / A1 |

Test sketches may use different pin assignments and pulse values. The supplied PCB schematic and archived integrated sketch differ at the ESC, left IR 1, and left IR 2 pins. See the [hardware/firmware revision table](docs/README.md#hardware-and-firmware-revision-check) before wiring or uploading.

## Development experience

The team iterated on traffic-light color and shape and retrained Pixy2 during testing. Mechanical fit, printed-part durability, equipment delays, and balancing assembly with autonomous testing also influenced development.

This fork adds documentation without changing the archived firmware. Calibration, loss-of-line behavior, and traffic-light decision logic should be validated before reuse.

## Team and source credit

Developed collaboratively by the **Schoolcraft Engineering Club**. Original source maintained in [Ce3Pi0/Autonomous-Car](https://github.com/Ce3Pi0/Autonomous-Car), itself forked from [schoolcraftengineeringclub/Autonomous-Car](https://github.com/schoolcraftengineeringclub/Autonomous-Car). Portfolio documentation and PCB contribution: **Zarraf Alam Fahmid**. Original commit history is retained.
