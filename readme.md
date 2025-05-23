# ESP32 Wireless RC Car

A 4WD wireless RC car project using ESP32 for both the car and controller units, featuring dual motor control and ESP-NOW communication.

## 🚀 Features

- 4WD drive system
- ESP-NOW wireless control
- Dual H-bridge motor control
- Battery voltage monitoring
- Fail-safe mechanisms
- USB programmable

## 🛠️ Bill of Materials (BOM)

### Mechanical Assembly

- **Main Chassis Kit** (1x)
  [4WD Chassis Kit](https://www.makerstore.com.au/product/robo-4wdsmartcar-chassis-kit/)
  Includes:
  - 4x [TT Motors](https://www.makerstore.com.au/product/mb-elc-ttmotor-pg48/)
  - 4x Wheels
  - Acrylic chassis plates
  - Mounting hardware

### Car Electronics (Receiver Unit)

- **Main Controller** (1x)
  - [ESP32 Development Board (ESP32-WROOM-32-DevKitC)](https://www.makerstore.com.au/product/esp32-development-board/)
  - Features:
    - Built-in WiFi/ESP-NOW
    - Sufficient GPIO pins
    - USB programming
- **Motor Driver** (1x)
  - [L298N Dual H-Bridge Motor Driver](<https://www.makerstore.com.au/product/elec-l298n-motor-drv/>)
  - Features:
    - Handles 4 motors
    - Built-in heatsink
    - 6-12V power supply compatible
    - 2A per channel

### Power System

- **Primary Power Supply**
  - [18650 Battery Shield](https://www.makerstore.com.au/product/mb-elc-bat-mod-18650/)
  - 1x 18650 batteriy (not included)
  - Switchable between 3.7V and 5V output
  - 2A output current at 5V, 1A at 3.7V
- **Alternative Power Supply**
  - 4x AA Battery Pack
  - 6V nominal voltage
  - ~2500mAh capacity with alkaline
  - Easy battery replacement
- **Accessories**
  - Cables
  - Power Switch (Optional)
  - Power Distribution Board (Optional)
  - Voltage Regulator (Optional)

### Controller Electronics (Transmitter Unit)

See the repo for the controller unit:
<https://github.com/Deejpotter/cyd-wireless-controller>

- **Controller Board** (1x)
  - JC2432W328 ESP32 Display Module
  - Features:
    - 2.4" touch display (320x240)
    - ESP32 processor
    - USB-C connection
    - Built-in WiFi/ESP-NOW

### Additional Materials

- **Wiring**
  - 22 AWG wire set (various colors)
  - Heat shrink tubing assortment
  - JST connectors
  - Screw terminals
  - Wire ties
- **Mounting Hardware**
  - M3 screws and nuts
  - Standoffs
  - Double-sided mounting tape
  - Cable ties
  - Rubber feet/bumpers

## 📦 Software Requirements

### Development Environment

- Visual Studio Code
- PlatformIO IDE Extension

### Required Libraries

```ini
lib_deps = 
    ESP32 Arduino Core
    AsyncElegantOTA  ; for OTA updates


### Pin Configuration

```cpp
// Motor Control Pins
#define MOTOR_LEFT_IN1   25  // Left motor direction 1
#define MOTOR_LEFT_IN2   26  // Left motor direction 2
#define MOTOR_RIGHT_IN3  27  // Right motor direction 1
#define MOTOR_RIGHT_IN4  14  // Right motor direction 2
#define MOTOR_LEFT_EN    12  // Left motor enable/speed (PWM)
#define MOTOR_RIGHT_EN   13  // Right motor enable/speed (PWM)

// Encoder Pins
#define ENCODER_LEFT_A   34  // Left motor encoder A
#define ENCODER_LEFT_B   35  // Left motor encoder B
#define ENCODER_RIGHT_A  36  // Right motor encoder A
#define ENCODER_RIGHT_B  39  // Right motor encoder B

// Status LED Pin
#define STATUS_LED      2   // Built-in LED
```

### Wiring Diagram

```
Power System:
4x AA Battery(6V) → Switch → Distribution Board
                          ├─► L298N VM (Motor Power)
                          └─► ESP32 VIN (5V Regulator)

Motor Connections:
L298N    Motors
OUT1 ──► Left Motor (+)
OUT2 ──► Left Motor (-)
OUT3 ──► Right Motor (+)
OUT4 ──► Right Motor (-)

ESP32          L298N
GPIO25 ───────► IN1
GPIO26 ───────► IN2
GPIO27 ───────► IN3
GPIO14 ───────► IN4
GPIO12 ───────► ENA
GPIO13 ───────► ENB
GND    ───────► GND

Encoder Connections:
Motors    ESP32
VCC ────► 3.3V
GND ────► GND
A   ────► GPIO34/36
B   ────► GPIO35/39
```

## 📦 Software Requirements

### Development Environment

- Visual Studio Code
- PlatformIO IDE Extension

### Required Libraries

```ini
lib_deps = 
    ESP32 Arduino Core
    ESP32Encoder
    AsyncElegantOTA  ; for OTA updates
```

## 🔧 Setup Instructions

1. Clone the repository:

```bash
git clone https://github.com/yourusername/esp32-wireless-car.git
cd esp32-wireless-car
```

2. Configure platformio.ini:

```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
monitor_speed = 115200
lib_deps = 
    madhephaestus/ESP32Encoder @ ^0.10.1
    AsyncElegantOTA
```

3. Build and upload:

```bash
pio run --target upload
```

## 🚗 Operation

### Power on sequence

1. Connect battery
2. ESP32 will initialize
3. Status LED indications:
   - Fast blink: Waiting for connection
   - Steady: Connected
   - Off: Error state

### Safety Features

- Low battery cutoff
- Motor current monitoring
- Connection loss failsafe (motors stop after 1s without commands)
- Emergency stop

## 🔍 Troubleshooting

### Motor Issues

- Check power connections
- Verify L298N pin connections
- Test encoder signals
- Check motor direction
- Measure battery voltage

### Communication Issues

- Verify MAC addresses match between controller and car
- Check for interference sources
- Ensure both devices are powered
- Check status LED patterns

## 🛠️ Maintenance

### Regular Checks

- Battery voltage
- Motor connections
- Encoder operation
- Wheel alignment
- Motor temperature

### Calibration

```cpp
// PID Parameters in config.h
#define KP 1.0
#define KI 0.1
#define KD 0.05

// Dead zone compensation
#define MIN_PWM 50  // Minimum PWM to move motors
#define MAX_PWM 255 // Maximum PWM value
```

## 📚 Resources

- [ESP32 Documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)
- [ESP-NOW Guide](https://randomnerdtutorials.com/esp-now-esp32-arduino-ide/)
- [L298N Motor Driver Guide](https://components101.com/modules/l298n-motor-driver-module)
- [ESP32Encoder Library](https://github.com/madhephaestus/ESP32Encoder)

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.
