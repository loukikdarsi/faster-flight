# faster-flight
PID-based fin stabilization system for experimental rockets using STM32 and FreeRTOS

## Overview

**faster-flight 🚀** is a real-time fin-actuated stabilization system designed for experimental model rockets. It uses a low-power STM32 microcontroller running FreeRTOS to dynamically adjust aerodynamic fin angles for real-time flight control and orientation stabilization.

The system combines real-time task management, PID-based control, and sensor fusion to improve in-flight stability and accuracy. It’s built with extensibility in mind and is suitable for small-to-medium scale rocketry platforms.

---

## Key Features

- **Real-Time Fin Control**  
  PID-based feedback control loop to actuate fins and stabilize rocket orientation in real time.

- **RTOS-Based Architecture**  
  Built on **FreeRTOS**, with task scheduling, inter-task communication (queues/mutexes), and prioritization for sensor, control, and telemetry tasks.

- **Advanced Sensor Fusion**  
  Combines data from IMU, barometer, and magnetometer using an **Extended Kalman Filter (EKF)** and **Complementary Filter** for accurate attitude and position estimation.

- **Telemetry + Logging**  
  Real-time telemetry via **LoRa radio**, with onboard flight data logging to external flash for post-flight analysis.

---

## System Architecture

```text
+-------------------------+       +------------------------+
|      Sensor Task        | <---> |     Sensor Fusion      |
|  IMU, Barometer, etc.   |       |  (EKF, Complementary)  |
+-------------------------+       +------------------------+
              |                              |
              v                              v
     +----------------+             +---------------------+
     | Control Task   | <---------> |   PID Controller    |
     | (FreeRTOS)     |             +---------------------+
     +----------------+
              |
              v
     +----------------+             +---------------------+
     | Fin Driver     | --------->  | Servo / PWM Output  |
     +----------------+             +---------------------+

         ^                         ^
         |                         |
+----------------+       +------------------------+
| Logging Task   |       |   Telemetry Task       |
| Flash Storage  |       |  LoRa Packet Encoding  |
+----------------+       +------------------------+
```

---

## Control Loop Details

- **PID Controller**  
  Continuously adjusts aerodynamic **fin angles** to stabilize pitch and yaw. Uses feedback from IMU sensors to correct **attitude error** and **angular rate**, ensuring smooth and responsive orientation control throughout flight.

- **Sensor Fusion**  
  - **Extended Kalman Filter (EKF)**: Provides high-accuracy estimation of the rocket’s **9-DOF orientation** and **velocity**, combining IMU, magnetometer, and barometric data.  
  - **Complementary Filter**: Offers faster convergence under **low-G** or **high-noise** conditions, improving short-term attitude estimation and startup performance.

- **RTOS Integration**  
  Control loop and sensing operations are split into separate **FreeRTOS tasks**. This modular design ensures:  
  - Real-time responsiveness  
  - Improved maintainability  
  - Reduced coupling between sensing, control, telemetry, and logging

---

## Telemetry & Logging

- **LoRa Communication**  
  Transmits real-time flight data — including **attitude**, **velocity**, and **altitude** — using a low-power **LoRa transceiver**. This enables live monitoring and ground station diagnostics during flight.

- **Flash Storage**  
  All sensor readings and control commands are buffered to external **SPI flash memory** (e.g., W25Q series). This allows for detailed **post-flight data review**, tuning of control parameters, and system performance analysis.
