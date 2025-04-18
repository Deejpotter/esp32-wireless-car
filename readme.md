# ESP32 Wireless RC Car Driver

A wireless RC car driver built on the ESP32 platform, featuring dual motor control with encoder feedback and ESP-NOW communication.

## 🚀 Features

- ESP32-based motor control system
- ESP-NOW wireless receiver
- Dual motor control with encoder feedback
- Real-time speed and position monitoring
- PID control implementation
- Battery monitoring
- Fail-safe mechanisms

## 🛠️ Hardware

### Components Required

- ESP32 Development Board
- L298N Motor Driver Module
- 2x DC Motors with Encoders
- 6V-12V Battery Pack
- Power distribution board/cables
- Optional 3D printed chassis

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
Battery(6-12V) → Switch → Distribution Board
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
