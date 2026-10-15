# Eugene: VEX V5 Robotics Autonomous Code (2025–26)

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
| Pneumatics  | Piston on 3-wire port H (only in `trumpy`, work in progress)    |
| Controller  | Split arcade drive; shoulder buttons run the intake and output  |

## Programs

| File | VEXcode slot | What it does | Status |
| ---- | ------------ | ------------ | ------ |
| `gharwala` | 1 | **Full skills route.** Collects and scores at several goals, re-aligning with the distance sensor between moves. 50% speed. | Most complete |
| `akela_bhediya` | 2 | Left-side match autonomous: three-goal route. | Draft; its saved device list is empty, so devices must be re-added in VEXcode |
| `trumpy` | 3 | Left-side autonomous plus pneumatic double-park / claw helpers for driver control. | Work in progress; doesn't compile as saved |
| `sleepy_joe` | 4 | Right-side (mirrored) match autonomous. | Needs `print_variable` restored (see file header) |

Each program has two files:

- **`.v5python`**: the original VEXcode project (JSON with the code and device configuration). Open this one in VEXcode.
- **`.py`**: the same Python code extracted for reading on GitHub, with comments explaining each routine and any known issues. The code is identical to the `.v5python`; only comments were added.

## How the code works

Every program follows the VEXcode competition template:

1. **Generated configuration** (`#region VEXcode Generated Robot Configuration`): device objects, plus a background thread that maps the controller joysticks to the drivetrain and the shoulder buttons to the intake and output motors.
2. **`pre_autonomous()`** sets motor torque and speed and the starting heading.
3. **`autonomous()`** is the scripted route. Instead of driving fixed distances only, the routes use loops like `while dis.object_distance(INCHES) < 26: pass` to stop at a measured distance from a wall or goal, which makes them less sensitive to wheel slip. Short `intake.spin(REVERSE)` pulses clear jammed blocks.
4. **`user_control()`** keeps the program alive while the generated thread handles driving.
5. **`Competition(user_control, autonomous)`** hands both routines to the field controller.

## My role

I wrote and tuned the autonomous routines and the driver-control setup in VEXcode Python, including the distance-sensor alignment strategy.

## Loading a program onto the robot

1. Install [VEXcode V5](https://www.vexrobotics.com/vexcode/install/v5).
2. Clone or download this repository.
3. In VEXcode: **File → Open** and choose a `.v5python` file. Don't open the `.py` files; they lack the device configuration.
4. Connect the V5 Brain (or the controller paired to it) over USB.
5. Pick the slot in the toolbar (the files above default to slots 1–4) and click **Download**.
6. Run it from the Brain's Programs screen, or under competition control with a field controller or competition switch.
