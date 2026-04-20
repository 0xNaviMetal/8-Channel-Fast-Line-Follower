

## ✨ Features
* **Competition-Ready:** Designed and tested for fast line-following competitions.
* **Complex Pattern Resolution:** Programmed to handle sharp angles, intersections, and difficult map patterns without losing the line.
* **High-Speed Drive:** Utilizes fast N20 micro gear motors paired with a highly efficient TB6612FNG motor driver.
* **Pin Optimization:** Uses a CD74HC4067 16-channel multiplexer to read 8 analog sensors using only one analog pin on the Arduino.
* **Stable Power Delivery:** Integrates an MP1584 step-down (buck) converter to provide clean, stable 5V power from a LiPo battery to the logic components.

---

## 🛠️ Hardware Components

| Component | Description |
| :--- | :--- |
| **Microcontroller** | Arduino Nano (Type-C) |
| **Sensor Array** | 8x TCRT5000 IR reflectance sensors (custom bar) |
| **Multiplexer** | CD74HC4067 16-Channel Analog MUX |
| **Motor Driver** | TB6612FNG Dual DC Motor Driver |
| **Motors** | 2x N20 Micro Gear Motors |
| **Power Management**| MP1584EN Buck Converter (Step-down to 5V) |
| **Power Source** | LiPo Battery |

---

## 🔌 Pin Mapping / Schematic Overview

Based on the custom shield design, here is how the core components route to the Arduino Nano:

### Motor Driver (TB6612FNG)
| Nano Pin | Driver Pin | Function |
| :---: | :---: | :--- |
| **D6** | PWMA | Motor A Speed (PWM) |
| **D7** | AIN1 | Motor A Direction 1 |
| **D8** | AIN2 | Motor A Direction 2 |
| **D12** | PWMB | Motor B Speed (PWM) |
| **D11** | BIN1 | Motor B Direction 1 |
| **D9** | BIN2 | Motor B Direction 2 |
| **5V** | STBY | Standby (Pulled HIGH to enable) |

### Multiplexer (CD74HC4067)
| Nano Pin | MUX Pin | Function |
| :---: | :---: | :--- |
| **D14 (A0)** | S0 | Control Pin 0 |
| **D13** | S1 | Control Pin 1 |
| **D11** | S2 | Control Pin 2 |
| **D10** | S3 | Control Pin 3 |
| **A2** | SIG | Common Analog Signal (Reads the active sensor) |

*(Note: The MUX allows the Arduino to rapidly cycle through all 8 sensors on the TCRT5000 array by changing the binary state of the S0-S3 pins and reading the single SIG pin).*

---

## 🚀 How It Works

1. **Reading the Line:** The Arduino rapidly switches the MUX control pins to read the analog values of all 8 IR sensors in a loop.
2. **Error Calculation:** The sensor data is converted into a weighted error value (determining exactly where the line is relative to the center of the robot).
3. **Motor Control:** A control algorithm (like PID) calculates the necessary speed adjustments for the left and right N20 motors.
4. **Execution:** The TB6612FNG driver updates the motor speeds via PWM, keeping the robot tightly locked onto the line even at high speeds.
