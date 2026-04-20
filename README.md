# 🏎️ 8-Channel Fast Line Follower Robot

**A high-speed, competition-ready line follower built with an Arduino Nano, N20 motors, and an 8-channel TCRT5000 sensor array multiplexed for fast map pattern recognition.**

![Fast Line Follower](image.png)
*(Built and programmed to compete in fast line-follower robotics competitions, capable of handling complex track patterns, sharp turns, and intersections).*

---

## 📂 Repository Structure

```text
Fast-Line-Follower-Nano-8CH/
│
├── 💻 Firmware/               # Arduino Source Code
│   └── Fast_Line_Follower.ino # Main control logic and PID loop
│
├── 🔌 Hardware/               # Electronics and Design Files
│   └── schematics/            # Wiring diagrams and pinouts
│
├── 🖼️ Assets/                 # Images of the robot and competition maps
│
└── 📄 README.md               # This documentation file
```
---

## 🛒 Bill of Materials (Hardware)

To replicate this competition build, you will need:

* **Arduino Nano** (The main brain, compact and lightweight)
* **8-Channel TCRT5000 Sensor Array** (Infrared sensors for high-resolution line detection)
* **Multiplexer (MUX)** (Used to efficiently read the 8 sensors without exhausting the Nano's pins)
* **2x N20 Micro Gear Motors** (High RPM, low weight for maximum speed)
* **MP1485 Buck Converter** (Highly efficient step-down converter to provide stable 5V logic power from a LiPo battery)
* **Motor Driver** (e.g., TB6612FNG or MX1508 for driving the N20 motors)
* **2S LiPo Battery** (7.4V for high discharge rate)

---

## 🧠 System Architecture & Wiring

### Power Management (MP1485)

In high-speed robots, voltage drops from sudden motor acceleration can reset the microcontroller. The **MP1485 Buck Converter** steps down the 7.4V LiPo battery to a rock-solid 5V to power the Nano, MUX, and Sensors, while the Motor Driver receives the raw 7.4V directly.

### Hardware Schematic Diagram

```text
                                [ 2S LiPo Battery 7.4V ]
                                          |
                   +----------------------+--------------------+
                   |                                           |
            [ Motor Driver ]                          [ MP1485 Buck ]
             |          |                                   | (5V)
        [ N20 Left ] [ N20 Right ]                          |
                                                            v
                                                     [ Arduino Nano ]
                                                            |
                                                   [ Multiplexer (MUX) ]
                                                            |
                                        [ 8-Channel TCRT5000 Sensor Array ]

```
---

## 💻 Software Logic & Map Pattern Recognition

To achieve high speeds, basic "if/else" left-right logic is not enough. This code is optimized to read all 8 sensors rapidly and recognize complex track patterns commonly found in competitions.

### 1. The 8-Channel Advantage
With 8 sensors, the robot has a wide field of view:
* **Inner Sensors (S3, S4, S5, S6):** Used for precise PD (Proportional-Derivative) line centering.
* **Outer Sensors (S1, S2, S7, S8):** Used exclusively for detecting extreme track features.

### 2. Map Pattern Handling
The code includes dedicated logic blocks to handle every track anomaly without losing speed:

| Track Feature | Sensor Trigger Pattern | Robot Action |
|---|---|---|
| **Straight Line** | `0 0 0 1 1 0 0 0` | Maximum Speed, PD active. |
| **Slight Curve** | `0 0 1 1 0 0 0 0` | Adjust motor PWM via PD (Proportional steering). |
| **90° Sharp Turn** | `1 1 1 1 0 0 0 0` | Hard brake inner wheel, max power outer wheel. |
| **T-Junction / Cross**| `1 1 1 1 1 1 1 1` | Competition specific: Usually ignore and drive straight, or turn based on pre-programmed maze rules. |
| **Dashed Lines** | `0 0 0 0 0 0 0 0` | Memory state: Continue driving at last known motor speeds until the line reappears. |

*(0 = White/Background, 1 = Black/Line)*


## 🚀 Getting Started & Tuning

1. **Wiring:** Connect the Nano, MUX, Motor Driver, and MP1485 according to the schematic. **Make sure the MP1485 is tuned to exactly 5.0V BEFORE connecting it to the Nano!**
2. **Sensor Calibration:** TCRT5000 sensors are sensitive to ambient light. Calibrate the thresholds in the code based on the competition floor and lighting conditions.
3. **Upload Firmware:** Open the `.ino` file and flash it to the Arduino Nano.
4. **PD Tuning (Crucial):** *(Note: This project uses a PD controller, skipping the `Ki` term to prevent integral windup at high speeds).*
   * Set `Kp` and `Kd` to 0.
   * Increase `Kp` until the robot is able to follow the line but oscillates (wobbles side-to-side).
   * Increase `Kd` gradually to dampen the wobble and make the robot lock smoothly onto the straight lines and curves.


---

## 🚧 Future Improvements

* [ ] Upgrade to a 32-bit microcontroller (like STM32 or ESP32) for faster PD loop calculation times.
* [ ] Implement a cooling fan for the motor driver for endurance races.
* [ ] Add a start/stop module using an IR remote to comply with strict competition rules.

---

## 🤝 Let's Connect

If you want to discuss line follower algorithms or embedded systems, feel free to reach out!
* **LinkedIn:** [slimane boucetta](https://linkedin.com/in/YourProfile)
