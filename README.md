# Eugene: VEX V5 Robotics Code (2025–26)

Autonomous and driver-control programs for **Eugene**, Pickering High School's VEX V5 robot in the 2025–26 VEX V5 Robotics Competition season. Eugene placed **1st at the Skills Ontario Durham Regional** and advanced to Provincials.

All programs are written in Python in VEXcode V5.

## The robot

| Subsystem   | Hardware                                                        |
| ----------- | --------------------------------------------------------------- |
| Drivetrain  | 4-motor, 4" wheels, 18:1 cartridges (ports 11, 12, 1, 2)        |
| Heading     | Inertial sensor (port 7), used for turns                        |
| Intake      | 1 motor (port 9) that collects blocks                           |
| Output      | 1 motor (port 10), scoring roller                               |
| Alignment   | Distance sensor (port 6), used to line up on walls and goals    |
| Pneumatics  | Piston on 3-wire port H (used by `auton_left_pneumatics`)       |
| Controller  | Split arcade drive; shoulder buttons run the intake and output  |

## Programs

| File | Originally named | VEXcode slot | What it does |
| ---- | ---------------- | ------------ | ------------ |
| `skills_full_route` | `gharwala` | 1 | **Autonomous skills, full route.** Collects and scores at several goals, re-aligning with the distance sensor between moves. 50% speed. |
| `auton_left_three_goal` | `akela_bhediya` | 2 | Match autonomous from the left tile: three-goal route. |
| `auton_left_pneumatics` | `trumpy` | 3 | Left-start autonomous plus pneumatic driver controls (X/B double park, Up/Down claw). |
| `auton_right` | `sleepy_joe` | 4 | Match autonomous from the right tile (mirror of the left route). |

The programs were renamed from their original nicknames to describe what they do. Each file's header records its original name.

Each program has two files:

- **`.v5python`**: the VEXcode project (JSON with the code and device configuration). Open this one in VEXcode.
- **`.py`**: the same Python code extracted for reading on GitHub, with comments explaining each routine. The code is identical to the `.v5python`; only comments were added.

### Fixes (October 2026)

Three of the original projects would not have run as saved. All four now compile and pass a check that every name they use is defined:

- `auton_left_three_goal`: the saved device list was empty, so the motors and sensors were never created. It now uses the same device config as the other programs.
- `auton_right`: restored the missing `print_variable()` debug helper that startup code depended on.
- `auton_left_pneumatics`: finished the half-written `crabClaw()` (syntax error), switched button checks to the VEX API's `controller_1.buttonX.pressing()` form, called the pneumatics helpers from the driver-control loop, and restored `print_variable()`.

## How the code works

Every program follows the VEXcode competition template:

1. **Generated configuration** (`#region VEXcode Generated Robot Configuration`): device objects, plus a background thread that maps the controller joysticks to the drivetrain and the shoulder buttons to the intake and output motors.
2. **`pre_autonomous()`** sets motor torque and speed and the starting heading.
3. **`autonomous()`** is the scripted route. Instead of driving fixed distances only, the routes use loops like `while dis.object_distance(INCHES) < 26: pass` to stop at a measured distance from a wall or goal, which makes them less sensitive to wheel slip. Short `intake.spin(REVERSE)` pulses clear jammed blocks.
4. **`user_control()`** keeps the program alive while the generated thread handles driving (and, in `auton_left_pneumatics`, polls the piston buttons).
5. **`Competition(user_control, autonomous)`** hands both routines to the field controller.

## My role

I wrote and tuned the autonomous routines and the driver-control setup in VEXcode Python, including the distance-sensor alignment strategy.

## Loading a program onto the robot

1. Install [VEXcode V5](https://www.vexrobotics.com/vexcode/install/v5).
2. Clone or download this repository.
3. In VEXcode: **File → Open** and choose a `.v5python` file. Don't open the `.py` files; they lack the device configuration.
4. Connect the V5 Brain (or the controller paired to it) over USB.
5. Pick the slot in the toolbar (the programs above default to slots 1–4) and click **Download**.
6. Run it from the Brain's Programs screen, or under competition control with a field controller or competition switch.
