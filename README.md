# Digitally Controlled Interleaved Buck Converter

[![Hardware](https://img.shields.io/badge/Hardware-STM32G474RET6-blue.svg)](https://www.st.com/en/microcontrollers-microprocessors/stm32g474re.html)
[![Control Theory](https://img.shields.io/badge/Control-Augmented%20LQR%20-orange.svg)](https://www.mathworks.com/products/control.html)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Digitally controlled **2-Phase Interleaved Synchronous Buck Converter** utilizing a state-space **Linear Quadratic Regulator** 
Optimized for time-critical embedded deployment at a switching frequency of **200 kHz** using the STM32G474 microcontroller.

<img height="400" alt="Top Render" src="https://github.com/Natchpai/LQRx_InterleavedBuck/blob/main/Images/Render-Board/2Phase_SynchBuck_Topview.png" /> 

---

## 🚀 Key Features

*   **2-Phase Interleaved Topology:** Reduce input and output current ripples, yields a **400 kHz** equivalent ripple, allowing for smaller filter components.
*   **Augmented LQR Control:** Discrete-time State-space control , Integral Action and Delay Compensation.
*   **Hard Execution:** Optimized algorithms execute within **1.8 µs**, inside the 5 µs timing window of the switching cycle.

---

## 📊 Key Specifications

| Parameter | Nominal Value | Note |
| :--- | :--- | :--- |
| **Input Voltage ($V_{in}$)** | 24 VDC | 28V MAX  |
| **Output Voltage ($V_{out}$)** | 12 VDC | Regulated target reference ($V_{ref}$) can changed. |
| **Maximum Load Current** | 18 A (9A per phase) | Rated for **200W+** continuous output capability  |
| **Switching Frequency ($f_{sw}$)** | 200 kHz | **400 kHz** equivalent input/output ripple frequency due to 180° interleaving |
| **Controller Strategy** | State Feedback | Mathematical compensation for discrete execution delay  |
| **Firmware Execution Time** | $\approx 2.2\ \mu s$ | Executed completely within a $5\ \mu s$ timing window |

---

## Hardware

### Power Stage Components
* **High-Side MOSFET**: OptiMOS-6 BSZ018N04LS6
* **Low-Side MOSFET**: OptiMOS-6 IQE013N04LM6
* **Power Inductors**: $10\ \mu\text{H}$
* **Capacitors**: $140\ \mu\text{F}$
  
---

## Control Implementation
1.  **Digital Delay Augmentation:** Mathematically models and compensates for the computation and ADC sampling delays within the state vector to prevent phase-margin degradation.
2.  **Integral Action Augmentation:** Includes an augmented error-integral state to eliminate steady-state tracking error, forcing $V_{out} = V_{ref}$ under varying load conditions.


* **Project Developer** - *Core Hardware, Embedded Firmware & Control Design* - [@Natchpai](https://github.com/Natchpai)
