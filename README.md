# **Raspberry Pi Pico & MPU6050 USB Gyro Steering Wheel & Gamepad Project**

In this project, we are building a fully custom, plug-and-play USB steering wheel and game controller using a **Raspberry Pi Pico** and an **MPU6050** gyroscope sensor.

The code processes the Z-axis movements of the MPU6050 to generate a precise steering angle (X-axis), alongside supporting a full D-Pad and numerous buttons.

📺 Watch the Full Project Video on YouTube:  https://www.youtube.com/watch?v=9KQNjhdO-iA&t

---
For electronic project design and [pcb layout](https://design.jlcpcb.com?from=makerio_layout) tools, you can check JLCPCB.

## **🛠️ Hardware and Pin Connections**

All buttons used in the code are configured with INPUT_PULLUP. This means the other leg of each button must be connected directly to **GND (Ground)**, eliminating the need for external resistors.

📺 Watch the Full Project Video on YouTube: comming soon

### **1\. MPU6050 Sensor Connections**

| **Function** | **GPIO Number** | **Physical Pin (Pico Pin)** | **Description**    |
| ------------ | --------------- | --------------------------- | ------------------ |
| **I2C SDA**  | GPIO 0          | Pin 1                       | MPU6050 Data Line  |
| ---          | ---             | ---                         | ---                |
| **I2C SCL**  | GPIO 1          | Pin 2                       | MPU6050 Clock Line |
| ---          | ---             | ---                         | ---                |

### **2\. Voltage and Ground (Power) Connections**

| **Rail / Pin** | **Raspberry Pi Pico Pin Equivalent**      | **Description**                                                            |
| -------------- | ----------------------------------------- | -------------------------------------------------------------------------- |
| **VCC / VBUS** | Pin 39 (VSYS) or Pin 40 (VBUS / 3V3 OUT)  | MPU6050 Power Supply Input (match your module specs, typically 3.3V or 5V) |
| ---            | ---                                       | ---                                                                        |
| **GND**        | Pin 3, 8, 13, 18, 23, 28, 33, or 38 (Any) | System Ground (GND) Line                                                   |
| ---            | ---                                       | ---                                                                        |

### **3\. Button and D-Pad Connections**

| **Function / Button** | **GPIO Number** | **Physical Pin (Pico Pin)** | **Description**                      |
| --------------------- | --------------- | --------------------------- | ------------------------------------ |
| **D-PAD UP**          | GPIO 2          | Pin 4                       | Directional Pad - Up                 |
| ---                   | ---             | ---                         | ---                                  |
| **D-PAD DOWN**        | GPIO 3          | Pin 5                       | Directional Pad - Down               |
| ---                   | ---             | ---                         | ---                                  |
| **D-PAD LEFT**        | GPIO 4          | Pin 6                       | Directional Pad - Left               |
| ---                   | ---             | ---                         | ---                                  |
| **D-PAD RIGHT**       | GPIO 5          | Pin 7                       | Directional Pad - Right              |
| ---                   | ---             | ---                         | ---                                  |
| **Button A**          | GPIO 6          | Pin 9                       | Action Button A                      |
| ---                   | ---             | ---                         | ---                                  |
| **Button B**          | GPIO 7          | Pin 10                      | Action Button B                      |
| ---                   | ---             | ---                         | ---                                  |
| **Button X**          | GPIO 8          | Pin 11                      | Action Button X                      |
| ---                   | ---             | ---                         | ---                                  |
| **Button Y**          | GPIO 9          | Pin 12                      | Action Button Y                      |
| ---                   | ---             | ---                         | ---                                  |
| **LB (Left Bumper)**  | GPIO 10         | Pin 14                      | Left Bumper                          |
| ---                   | ---             | ---                         | ---                                  |
| **RB (Right Bumper)** | GPIO 11         | Pin 15                      | Right Bumper                         |
| ---                   | ---             | ---                         | ---                                  |
| **START**             | GPIO 12         | Pin 16                      | Start Button                         |
| ---                   | ---             | ---                         | ---                                  |
| **SELECT**            | GPIO 13         | Pin 17                      | Select Button                        |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 1**             | GPIO 14         | Pin 19                      | Extra Button 1                       |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 2**             | GPIO 15         | Pin 20                      | Extra Button 2                       |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 3**             | GPIO 16         | Pin 21                      | Extra Button 3                       |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 4**             | GPIO 17         | Pin 22                      | Extra Button 4                       |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 5**             | GPIO 18         | Pin 24                      | Extra Button 5                       |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 6**             | GPIO 19         | Pin 25                      | Extra Button 6                       |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 7**             | GPIO 20         | Pin 26                      | Extra Button 7                       |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 8**             | GPIO 21         | Pin 27                      | Extra Button 8                       |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 9**             | GPIO 22         | Pin 29                      | Extra Button 9                       |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 10**            | GPIO 26         | Pin 31                      | Extra Button 10                      |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 11**            | GPIO 27         | Pin 32                      | Extra Button 11                      |
| ---                   | ---             | ---                         | ---                                  |
| **BTN 12**            | GPIO 28         | Pin 34                      | Extra Button 12                      |
| ---                   | ---             | ---                         | ---                                  |
| **Onboard LED**       | GPIO 25         | \-                          | Status Indicator (Calibration, etc.) |
| ---                   | ---             | ---                         | ---                                  |

## **⚙️ Code Analysis & Highlights**

1. **TinyUSB HID Support:** Thanks to the Adafruit_TinyUSB.h library, the Raspberry Pi Pico is recognized by your computer as a standard USB Game Controller without needing external drivers.
2. **Adaptive Smoothing:** Employs a tiered filtering algorithm (SMOOTH_HEAVY, SMOOTH_MEDIUM, SMOOTH_LIGHT) to ensure quick response during rapid turns while avoiding jitters during slow movements.
3. **Auto-Center:** When the gyro sensor becomes still, the steering wheel smoothly glides back to the center (0 position) automatically.
4. **Quick Reset:** Pressing START and SELECT simultaneously instantly zeroes out and locks the steering angle back to the center.
5. **Gyro Calibration:** The onboard LED flashes upon startup to indicate sensor calibration. Keep the wheel/sensor completely still during this phase.

## **📥 Step-by-Step Upload Guide via Arduino IDE**

Follow these steps sequentially to upload the project to your Raspberry Pi Pico:

### **Step 1: Arduino IDE & Pico Core Setup**

1. Open **Arduino IDE** (v2.x recommended).
2. Go to File > Preferences.
3. Add the following URL to **Additional Boards Manager URLs**:  
   <https://github.com/earlephilhower/arduino-pico/releases/download/global/package_rp2040_index.json>
4. Open Tools > Board > Boards Manager, search for **Pico**, and install the **Raspberry Pi Pico/RP2040** package.

### **Step 2: Library Installation**

This project requires the TinyUSB-based HID library:

1. Open Tools > Manage Libraries...
2. Search for **Adafruit TinyUSB** and install it (accept any dependency prompts if requested).

### **Step 3: Board Configuration**

From the Tools menu, configure your Pico settings as follows:

- **Board:** Raspberry Pi Pico (or your specific Pico model variant)
- **USB Stack:** Adafruit TinyUSB _(Crucial: must be selected instead of the standard stack)_
- **Port:** The designated COM port when your Pico is connected.

### **Step 4: Uploading the Code**

1. Copy the source code below and paste it into your Arduino IDE.
2. Click the **Verify** button in the top-left corner to compile and check for errors.
3. Hold down the **BOOTSEL** button on your Pico while plugging it into your computer via USB. Once it mounts as a drive, click **Upload**.
