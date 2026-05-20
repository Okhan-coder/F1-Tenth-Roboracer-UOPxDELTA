# F1TENTH Roboracer — Jetson + VESC + Hokuyo LiDAR

PS4-controlled F1TENTH / Roboracer platform built on a Traxxas Ford Fiesta ST Rally VXL (74276-4) with a VESC 6 MkVI motor controller, Hokuyo URG-04LX LiDAR, and NVIDIA Jetson Orin Nano.

## Hardware

| Component | Model |
|---|---|
| Chassis | Traxxas Ford Fiesta ST Rally VXL (74276-4) |
| Motor controller | VESC 6 MkVI |
| Computer | NVIDIA Jetson Orin Nano |
| LiDAR | Hokuyo URG-04LX |
| Controller | Sony PS4 DualShock / AceGamer wireless |

## Port Assignments

| Device | Port |
|---|---|
| VESC 6 MkVI | `/dev/ttyACM1` |
| Hokuyo URG-04LX | `/dev/ttyACM0` |

> Some older scripts in `controllers/` use `/dev/vesc`. This is a udev symlink you can create with:
> ```bash
> echo 'SUBSYSTEM=="tty", ATTRS{idVendor}=="0483", SYMLINK+="vesc"' | sudo tee /etc/udev/rules.d/99-vesc.rules
> sudo udevadm control --reload-rules && sudo udevadm trigger
> ```

## Installation

```bash
pip3 install -r requirements.txt
```

## Scripts

### `main/` — Primary driving scripts

| Script | Description |
|---|---|
| `SteeringAssist.py` | **Most advanced.** PS4 drive + 7-sector LiDAR steering assist + live 2D bird's-eye map. |
| `LiDARCode.py` | PS4 drive + LiDAR collision stop. |
| `AssistedAutonomyV1.py` | PS4 drive + 4-zone LiDAR speed control (no steering assist). |
| `CAR.py` | Basic PS4 drive, no LiDAR. |

### `controllers/` — Alternative controller scripts

| Script | Description |
|---|---|
| `ps4_vesc_controller_duty_linux.py` | Smooth duty-mode PS4 driver (earlier version). |
| `roboracer.py` | Compact PS4 driver with rescaled deadzone. |
| `acegamer_controller.py` | AceGamer wireless controller support (D-Pad steering). |

### `tests/` — Hardware diagnostic utilities

| Script | Description |
|---|---|
| `detection.py` | Detect and print connected controller info. |
| `duty_test_linux.py` | Send a short duty pulse to verify VESC communication. |
| `servo_only_linux_hold.py` | Hold servo at left/center/right to verify steering range. |
| `servo_float_linux.py` | Experimental float-format servo test. |
| `vesc_ping_linux.py` | Request VESC firmware version to verify serial link. |

## Controls (PS4)

| Input | Action |
|---|---|
| Left stick up/down | Forward / reverse |
| Right stick left/right | Steering |
| Hold X | Emergency stop |
| Circle | Quit |
| Triangle *(SteeringAssist only)* | Toggle steering assist on/off |

## Safety

- Always put the car on a stand before running motor scripts.
- Close VESC Tool before running any script (both compete for the serial port).
- The LiDAR scripts enforce an emergency stop when an obstacle is closer than 800 mm.

## Docs

See the `docs/` folder for setup guides, system reports, and known bug fixes.
