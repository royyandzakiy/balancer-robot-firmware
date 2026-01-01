# Development

### Sprint

- v1
    - copy from zephyr modern cpp. empty main.cpp, start fresh
    - ble: peripheral, uart
    - implement in real hardware (use nrf5340dk as devkit, use xiao ble as final board)
- v2
    - implement base of app event manager, event folder
        - create base foldering for seperation of concerns
    - connect & pwm on led
    - connect & read imu
    - implement feedback loop: pid, controls led
- v3
    - ble ota

### Feature Backlog

- control
    - receive command via ble uart shell
    - sending debug logs via ble uart shell
    - use the desktop app to send keypad arrows to control the robot
    - using a gamepad console, connected to the desktop app to control the robot
- testing
    - learn, poc ztest, fff, twister > add to current code
- using a more rigour control algorithm

---

### Features

- c++ features
    - span (for buffer u8), mdspan
    - view ranges
    - expected, errorcode, optional
    - stl: array
    - string view
    - concepts, constraints
    - constexpr, constinit
    - chrono
    - variant
    - attributes: [[nodiscard]]
- compiling
    - flags: -fno-exceptions -fno-rtti -Werror -c++=23
    - clang tidy
- external libs
    - boost smf header

---

### Project Structure

```
- balance-robot-workspace
	- zephyr
	- desktop-app
	- firmware
        - github workflow
        - cmake
		- samples
		- app
			- events
			- modules
				- stabilizer
				- actuators
				- ble
				- system
				- shell
			- main.cpp
		- boards
		- tests
		- docs
    - workspace-manifest
        - adr
            - class diagram, dfd
        - prd
            - roadmap.md
        - erd
            - frameworks & technical references
        - hardware
```

```
.
├── .github/workflows/      # CI/CD (Twister for Ztest, Doxygen deploy)
├── .west/                  # West manifest and configuration
├── boards/                 # Custom ESP32 board definitions/overlays
├── cmake/                  # Custom CMake modules (e.g., versioning)
├── docs/                   # Doxygen config and architecture .md files
├── drivers/                # Custom out-of-tree drivers (if needed)
├── scripts/                # Python scripts for BLE telemetry parsing
├── tests/                  # Ztest suites for PID & Filter logic
├── west.yml                # Project manifest (Zephyr + Modules)
└── firmware/                    # THE MAIN APPLICATION
    ├── Kconfig             # App-specific config options
    ├── prj.conf            # Main config (BLE, MCUboot, Event Mgr)
    ├── CMakeLists.txt      # Build instructions
    ├── src/
    │   ├── main.c          # App Entry, Init, and Event Loop
    │   ├── events/         # Event Manager definitions (.h/.c)
    │   ├── modules/        # Decoupled functional blocks
    │   │   ├── stabilizer/ # PID & IMU Fusion (The "Brain")
    │   │   ├── actuators/  # Motor & PWM management
    │   │   ├── system
    │   │   ├── shell
    │   │   └── ble/      # BLE, NUS (UART), & Telemetry
    └── include/            # Public headers for modules
```