# Digitally Controlled Interleaved Buck Converter

[![Hardware](https://img.shields.io/badge/Hardware-STM32G474RET6-blue.svg)](https://www.st.com/en/microcontrollers-microprocessors/stm32g474re.html)
[![Control Theory](https://img.shields.io/badge/Control-Augmented%20LQR%20-orange.svg)](https://www.mathworks.com/products/control.html)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Digitally controlled **2-Phase Interleaved Synchronous Buck Converter** utilizing a state-space **Linear Quadratic Regulator (LQR)** 
Optimized for time-critical embedded deployment at a switching frequency of **200 kHz** using the STM32G4 "Digital Power" microcontroller.

<img height="400" alt="Top Render" src="https://github.com/Natchpai/LQRx_InterleavedBuck/blob/main/Images/Render-Board/2Phase_SynchBuck_Topview.png" /> 

---

## 🚀 Key Features

*   **2-Phase Interleaved Topology:** Shifts the two phases by 180° to drastically reduce input and output current ripples. This effectively yields a **400 kHz** equivalent ripple frequency, allowing for smaller filter components.
*   **Augmented LQR Control:** Replaces traditional PI loops with modern state-space control. It features an integrated discrete-time delay compensation matrix.
*   **Hard Real-Time Execution:** Optimized control algorithms execute completely within **2.2 µs**, well inside the tight 5 µs timing window of the switching cycle.

---

## 📊 Key Specifications

| Parameter | Nominal Value | Note |
| :--- | :--- | :--- |
| **Input Voltage ($V_{in}$)** | 24 VDC | 28V MAX  |
| **Output Voltage ($V_{out}$)** | 12 VDC | Regulated target reference ($V_{ref}$) can changed. |
| **Maximum Load Current** | 18 A (9A per phase) | Rated for **200W+** continuous output capability  |
| **Switching Frequency ($f_{sw}$)** | 200 kHz | **400 kHz** equivalent input/output ripple frequency due to 180° interleaving |
| **Target Controller** | Full-state Feedback | Mathematical compensation for discrete execution delay  |
| **Firmware Execution Time** | $\approx 2.2\ \mu s$ | Executed completely within a $5\ \mu s$ timing window |

---

## Hardware & PCB Architecture

### Power Stage Components
* **High-Side MOSFET**: OptiMOS-6 BSZ018N04LS6 (Ultra-low $R_{DS(on)}$ for low switching losses)
* **Low-Side MOSFET**: OptiMOS-6 IQE013N04LM6
* **Power Inductors**: $10\ \mu\text{H}$
  
### PCB Architecture
- ...
---

## Real-Time Scheduling & Timing
To eliminate jitter and maintain strict deterministic behavior, the firmware execution respects a tightly designed digital control cycle layout
* **$t = 0.0\ \mu s$**: The High-Resolution Timer triggers the dual ADC to sample phase currents and output voltage simultaneously.
* **$t = 0.0 \to 2.2\ \mu s$**: The MCU computes the discrete LQR full-state feedback law and pre-loads the computed duty cycles into the hardware shadow registers.
* **$t = 5.0\ \mu s$**: Phase 1 hardware registers latch and update the duty cycle, yielding an exact **$1.0\ T_s$** loop latency.
* **$t = 7.5\ \mu s$**: Phase 2 hardware registers latch and update, seamlessly handling the fractional **$1.5\ T_s$** loop shift via robust algorithmic design.

---

## Control Implementation
The physical plant model is mapped into a discrete **State-Space** structure, augmented to handle physical constraints and execution characteristics
1.  **Digital Delay Augmentation:** Mathematically models and compensates for the computation and ADC sampling delays within the state vector to prevent phase-margin degradation.
2.  **Integral Action (Tracking):** Includes an augmented error-integral state to eliminate steady-state tracking error, forcing $V_{out} = V_{ref}$ under varying load conditions.
3.  **Anti-Windup Protection:** Clamps the integral state accumulator to prevent duty-cycle saturation issues during heavy transient steps or startup.
  

## Author

* **Project Developer** - *Core Hardware, Embedded Firmware & Control Design* - [@Natchpai](https://github.com/Natchpai)
