#  Week 2 – VSDBabySoC Design and Functional Modelling

##  Introduction

**Week 2** introduces the **functional modelling phase** of SoC design using the **VSDBabySoC** as a reference. Instead of dealing with the full complexity of commercial SoCs, BabySoC focuses on a minimal yet complete system that includes a **processor**, a **clock generator**, and an **analog converter**. Through this, learners understand how different IP blocks interact, how clock synchronization works, and how digital data can be converted to analog signals for external use.
 
## 📝 Table of Contents


- [System on Chip](#-system-on-chip-soc)
- [Key Parts of a System-on-Chip](#key-parts-of-a-system-on-chip)
- [Technical Challenges of SoC Design](#technical-challenges-of-soc-design)
- [Types of SoCs](#types-of-socs)
- [SoC Design Flow](#soc-design-flow)
- [VSD BabySoC](#vsd-babysoc)
     - [RVMYTH (RISC-V CPU)](#rvmyth-risc-v-cpu)
     - [8× Phase Locked Loop (PLL)](#8-phase-locked-loop-pll)
     - [10-bit Digital-to-Analog Converter (DAC)](#10-bit-digital-to-analog-converter-dac)
- [Summary](#summary)
---


---

# 📘 System on Chip (SoC)

---

##  What is an SoC?
- A **System on Chip (SoC)** is an **integrated circuit (IC)** that consolidates all essential components of a computer or electronic system into a single chip.  
- Instead of having **CPU, memory, I/O controllers, and communication units** as separate chips on a PCB, an SoC integrates them together.  
- Commonly used in **smartphones, IoT devices, embedded systems, automotive electronics, and AI accelerators**.  
- **Advantages:**  
  - Smaller size  
  - Lower power consumption  
  - Higher performance  
  - Cost efficiency


   ![soc](https://github.com/MOHANAPRIYANP16/Week-2-VSD-RISC-V-Tapeout-Program-/blob/main/Part1/Images/soc_chip1.jpg)

---

##  Popular SoCs 

- **Apple A-Series**: ARM-based high-performance CPUs with integrated GPU and Neural Engine, used in iPhones and iPads.  
- **Qualcomm Snapdragon**: Multi-core ARM CPUs with Adreno GPU and 5G modem, powering most Android smartphones.  
- **Samsung Exynos**: ARM-based processors with Mali GPU or custom GPU, used in Samsung devices for balanced performance.  
- **NVIDIA Jetson/Tegra**: Combines ARM CPUs with NVIDIA GPUs, designed for AI, robotics, and autonomous systems.  
- **Broadcom BCM2711**: Quad-core ARM CPU with VideoCore GPU, widely used in Raspberry Pi boards for education and prototyping.  
- **ESP32**: Low-cost dual-core SoC with Wi-Fi and Bluetooth, popular for IoT and embedded applications.


![popular_socs](https://github.com/MOHANAPRIYANP16/Week-2-VSD-RISC-V-Tapeout-Program-/blob/main/Part1/Images/popular_socs.jpg)

---

#  Key Parts of a System-on-Chip

---

## 1. Processing Units
The "brain" of the SoC. These are specialized cores designed for different kinds of computation.

- **CPU (Central Processing Unit):**  
  - Executes general-purpose instructions.  
  - Often based on architectures like **ARM Cortex-A/M/R, RISC-V, or x86**.  
  - Can be **single-core, multi-core, or heterogeneous clusters** (e.g., big.LITTLE in ARM).  
  - Handles OS, control logic, and application-level tasks.  

- **GPU (Graphics Processing Unit):**  
  - Specialized for **parallel processing**.  
  - Performs graphics rendering and compute-intensive tasks (AI/ML workloads in mobile).  
  - Examples: **ARM Mali, Adreno, NVIDIA CUDA cores**.  

- **DSP (Digital Signal Processor):**  
  - Optimized for **repetitive, math-heavy tasks** like audio/video compression, filtering, FFT, voice recognition.  
  - Uses special instruction sets for **fast multiply-accumulate (MAC) operations**.  

- **AI/ML Accelerators (NPUs / TPUs):**  
  - Dedicated hardware for **matrix multiplications** and **deep learning inference**.  
  - Provides much higher efficiency than CPU/GPU for ML workloads.  
  - Used in **smartphones, edge AI devices, self-driving cars**.  

---

## 2. Memory System
Efficient memory is critical to prevent bottlenecks.

- **On-Chip SRAM (Static RAM):**  
  - Small, fast memory located near cores.  
  - Used for **caches (L1, L2, L3)**.  

- **DRAM Controller & Interface:**  
  - Connects external **DDR4/DDR5/LPDDR memory**.  
  - Provides large storage but slower than caches.  

- **ROM / Flash Memory:**  
  - Non-volatile storage for **boot code, firmware, security keys**.  

- **Embedded DRAM (eDRAM):**  
  - Medium-capacity memory embedded on the chip.  
  - Balances **performance and area** better than SRAM.  

- **Cache Hierarchy:**  
  - **L1 Cache:** Closest to CPU, very small (tens of KBs).  
  - **L2 Cache:** Bigger, shared across cores.  
  - **L3 Cache:** Large, shared across the system.  

---

## 3. Interconnect / Bus Systems
The "highways" of the SoC that connect cores, memory, and peripherals.

- **AMBA Bus Family (by ARM):**  
  - **AXI (Advanced eXtensible Interface):** High-speed, high-bandwidth.  
  - **AHB (Advanced High-performance Bus):** Medium performance.  
  - **APB (Advanced Peripheral Bus):** Low-power, simple peripherals.  

- **Network-on-Chip (NoC):**  
  - Used in modern SoCs (e.g., smartphones, GPUs).  
  - Provides **scalable, packet-switched interconnects** instead of simple buses.  
  - Reduces congestion and supports many cores.  

- **Crossbar Switches:**  
  - Direct **high-speed point-to-point links** between key components.  

---

## 4. Peripherals & I/O Interfaces
Allows communication with the outside world.

- **Serial Communication:** UART, SPI, I2C.  
- **High-Speed Interfaces:** USB, PCI Express, SATA.  
- **Networking:** Ethernet, Wi-Fi, Bluetooth, 5G/4G, Zigbee.  
- **Multimedia:** Camera Serial Interface (CSI), Display Serial Interface (DSI), HDMI.  
- **Storage:** eMMC, UFS, SD card interfaces.  
- **GPIO (General Purpose I/O):** Simple pins for sensors/actuators.  

---

## 5. Power Management Subsystem
Manages energy efficiency and thermal stability.

- **PMU (Power Management Unit):** Controls power domains.  
- **DVFS (Dynamic Voltage and Frequency Scaling):** Adjusts CPU/GPU frequency to save energy.  
- **Clock Gating / Power Gating:** Turns off unused blocks.  
- **Thermal Sensors:** Monitor overheating and throttle performance if needed.  

---

## 6. Security Subsystem
Critical for smartphones, IoT, automotive, and banking systems.

- **Secure Boot:** Ensures system boots only trusted firmware.  
- **Cryptographic Engines:** AES, RSA, ECC, SHA accelerators.  
- **TPM / TEE (Trusted Execution Environment):** Isolated environment for sensitive code.  
- **Hardware Firewalls:** Isolate different IP blocks for security.  

---

## 7. Analog & Mixed-Signal Blocks
Even digital SoCs require analog parts.

- **PLL (Phase-Locked Loop):** Generates stable, high-frequency clock signals.  
- **ADC (Analog-to-Digital Converter):** Converts sensor signals into digital data.  
- **DAC (Digital-to-Analog Converter):** For audio/video playback, actuators.  
- **RF Circuits:** Handle wireless communication (Bluetooth, Wi-Fi, LTE).  
- **Voltage Regulators:** Provide stable power to subsystems.  

---

## Technical Challenges of SoC Design  

1. **Integration Complexity**  
   - Combining CPUs, GPUs, DSPs, AI accelerators, memory, and peripherals.  
   - Ensuring correct communication and timing across heterogeneous IP blocks.  

2. **Power Management**  
   - Implementing techniques like **DVFS, clock gating, and power domains**.  
   - Avoiding excessive heat while maintaining performance.  

3. **Performance Bottlenecks**  
   - Memory bandwidth limitations and interconnect congestion.  
   - Arbitration between multiple masters (CPU, GPU, DMA) competing for resources.  

4. **Verification & Validation**  
   - Requires testing at **RTL, gate-level, and system level**.  
   - Simulation, emulation, and FPGA prototyping are resource-intensive.  

5. **Physical Design Challenges**  
   - Floorplanning, placement, routing, and heat dissipation in dense layouts.  
   - Achieving **timing closure, signal integrity, and low power leakage**.  

6. **Security**  
   - Protecting against **hardware Trojans, side-channel attacks, and data leaks**.  
   - Ensuring **secure boot, cryptographic engines, and hardware isolation**.

---

## Types of SoCs

System-on-Chip (SoC) designs can be categorized based on their purpose, processing capabilities, and applications. Each type balances **performance, power efficiency, and functionality** for its intended use case.

| Type of SoC                  |  Description | Typical Applications | Key Specifications |
|-------------------------------|-----------------|--------------------|------------------|
| **Microcontroller-based SoC** | Built around a microcontroller, designed for simple control tasks. Known for **low power usage and efficiency**. | Home appliances, car systems, IoT devices | Low clock speed, small memory (RAM/ROM), limited peripherals, optimized for low power |
| **Microprocessor-based SoC**  | Features a microprocessor capable of handling **demanding tasks and running operating systems**. Supports complex, multitasking applications. | Smartphones, tablets, interactive/data-intensive applications | Higher clock speed, larger memory, multiple cores, supports OS, more advanced peripherals |
| **Application-Specific SoC**  | **Custom-designed** for high-performance, specialized tasks. Optimized for **speed and efficiency** in dedicated roles. | Graphics cards, AI hardware, specialized industrial/financial systems | High-speed cores, specialized accelerators (GPU/AI/DSP), high memory bandwidth, optimized for target application |

---

# SoC Design Flow

Designing a System-on-Chip (SoC) is a **complex multi-stage process** that integrates hardware and software components. The flow ensures the chip meets **functional, performance, and power requirements**.

---

## 1. Specification & Requirements
- Define the **purpose** of the SoC and target applications.  
- Determine **performance, power, area, and cost constraints**.  
- Decide on **processing units, memory size, peripherals, and interfaces**.  

---

## 2. Architecture Design
- Create a **high-level block diagram** showing CPUs, GPUs, memory, buses, and peripherals.  
- Plan **interconnects, clocking, and power domains**.  
- Decide on **hardware accelerators or custom blocks** if required.  

---

## 3. RTL (Register Transfer Level) Design
- Write the **hardware description** using **Verilog, VHDL, or SystemVerilog**.  
- Implement all components: CPU cores, memory controllers, interfaces, and peripherals.  
- Ensure **modularity and reusability** of IP blocks.  

---

## 4. Simulation & Functional Verification
- Verify RTL functionality using **testbenches**.  
- Perform **functional simulation** to ensure correctness.  
- Detect and fix **logical errors** before synthesis.  

---

## 5. Synthesis
- Convert RTL code into **gate-level netlist** using **synthesis tools**.  
- Optimize for **area, speed, and power**.  
- Map design to **target technology library** (ASIC/FPGA).  

---

## 6. Floorplanning & Physical Design
- **Floorplanning:** Place major blocks on the chip.  
- **Placement & Routing:** Connect blocks with wires, optimize paths.  
- **Clock Tree Synthesis (CTS):** Ensure synchronized clock distribution.  
- **Power Planning:** Assign power and ground networks.  

---

## 7. Timing Analysis
- Perform **Static Timing Analysis (STA)** to check timing constraints.  
- Ensure **setup, hold, and clock domain crossing requirements** are met.  

---

## 8. Power Analysis & Optimization
- Analyze **dynamic and leakage power**.  
- Apply techniques like **DVFS, clock gating, and power gating**.  
- Optimize design for **low energy consumption** without performance loss.  

---

## 9. Verification & Sign-Off
- Perform **formal verification, lint checks, and equivalence checking**.  
- Run **post-layout simulation** including parasitics.  
- Ensure the design meets all **functional, timing, and power specs**.  

---

## 10. Fabrication & Testing
- Send the design to **foundry for manufacturing**.  
- Test **first silicon** for functionality, timing, and power.  
- Debug and iterate if necessary before mass production.  

---

## 11. Software & Firmware Integration
- Develop device drivers, OS support, and middleware.  
- Validate **hardware-software co-design**.  
- Optimize **performance, memory usage, and power** for final application.  

---

![soc_design_flow](https://github.com/MOHANAPRIYANP16/Week-2-VSD-RISC-V-Tapeout-Program-/blob/main/Part1/Images/soc_design_flow.png)

---

## VSD BabySoC

**VSDBabySoC** is a compact yet powerful **System on Chip (SoC)** based on the **RISC-V** architecture.  
Its main goal is to enable the simultaneous testing of **three open-source IP cores** while also calibrating its analog components.

###  Key Components
- **RVMYTH Microprocessor** – Handles data processing and instruction execution.  
- **8× Phase-Locked Loop (PLL)** – Generates a stable, synchronized clock signal for all components.  
- **10-bit Digital-to-Analog Converter (DAC)** – Converts digital values into analog signals for external devices.

###  Working Overview
1. **Initialization & Clock Generation**  
   Upon receiving an input signal, the **PLL** activates and generates a stable clock. This clock synchronizes the RVMYTH and DAC, ensuring smooth, coordinated operation.

2. **Data Processing in RVMYTH**  
   The **RVMYTH** processor uses its `r17` register to store and cycle through values that are sent to the DAC. As instructions are executed, `r17` updates continuously to prepare data for conversion.

3. **Analog Signal Generation via DAC**  
   The **DAC** receives digital values from RVMYTH, converts them into an **analog signal**, and stores the output in a file named `OUT`. This analog output can be connected to external devices like TVs or mobile phones, enabling real-world multimedia interfacing.

![vsd_baby_soc](https://github.com/MOHANAPRIYANP16/Week-2-VSD-RISC-V-Tapeout-Program-/blob/main/Part1/Images/vsd_babysoc.png)

---

## RVMYTH (RISC-V CPU)

**RVMYTH** is a compact RISC-V based microprocessor core that serves as the digital processing unit of VSDBabySoC. It executes instructions and manages data flow within the system. Specifically, it uses its `r17` register to hold and update values that are later sent to the DAC for conversion. This allows continuous digital data processing and streaming.

![rv](https://github.com/MOHANAPRIYANP16/Week-2-VSD-RISC-V-Tapeout-Program-/blob/main/Part1/Images/rvmyth.png)

---

## 8× Phase Locked Loop (PLL)

The **8× PLL** is responsible for generating a stable and synchronized clock signal. When the SoC is initialized, the PLL locks onto the input frequency and multiplies it by 8, providing a high-frequency clock source for the RVMYTH CPU and DAC. This ensures all components operate in sync, avoiding timing mismatches and improving performance.

![pll](https://github.com/MOHANAPRIYANP16/Week-2-VSD-RISC-V-Tapeout-Program-/blob/main/Part1/Images/PLL.jpeg)
       

---

## 10-bit Digital-to-Analog Converter (DAC)

The **10-bit DAC** converts digital values processed by RVMYTH into analog signals. These signals are stored in a file named `OUT` and can be fed into external devices such as TVs or mobile phones. This enables VSDBabySoC to interface with real-world analog systems, demonstrating how digital processing can drive multimedia outputs.

![dac](https://github.com/MOHANAPRIYANP16/Week-2-VSD-RISC-V-Tapeout-Program-/blob/main/Part1/Images/dac.jpg)

---

##  Why BabySoC is a Simplified Model ? :

**VSDBabySoC** is designed as a **small-scale, educational SoC** that focuses on the essential building blocks of a real chip while keeping the overall complexity manageable.  
Instead of integrating dozens of IPs and peripherals like commercial SoCs, BabySoC uses only **three key components** — a RISC-V CPU (RVMYTH), an 8× PLL, and a 10-bit DAC — to demonstrate the **core principles of SoC design** such as clock generation, digital processing, and analog interfacing.

This simplification allows students, researchers, and developers to:

- Understand the fundamental interactions between digital and analog blocks.  
- Learn SoC integration techniques without dealing with excessive complexity.  
- Quickly verify functionality and debug the system.  
- Build a strong foundation before scaling to larger, more complex SoC designs.

---

##  Role of Functional Modelling

**Functional modelling** plays a critical role in the development of BabySoC by allowing designers to **simulate and validate the behavior** of each component and their interactions **before physical implementation**.

Key roles include:

- **Early Verification** – Ensures that the CPU, PLL, and DAC work correctly together in a virtual environment.  
- **Error Detection** – Identifies design flaws, timing mismatches, or integration issues early in the flow.  
- **Faster Iteration** – Enables rapid testing and modification of the SoC without the need for hardware.  
- **Design Clarity** – Provides a clear functional view of how data flows through the SoC, from clock generation to processing to analog output.

By using functional models, BabySoC demonstrates how **concept-to-validation** can be achieved efficiently, making it an excellent learning platform for SoC design.

---

For more details, visit the [VSDBabySoC GitHub repository](https://github.com/manili/VSDBabySoC)

##  Summary

This week focuses on understanding the **basics of SoC design** and exploring **VSDBabySoC** as a practical, simplified example. It connects theoretical SoC concepts with real functional modeling techniques used in chip development.

### Key Topics Covered

- **Introduction to SoCs:** Combines processing units, memory, and peripherals into a single chip for compact and efficient computing.  
- **Main Components:**  
  - **Processing Unit (CPU)** – Executes instructions.  
  - **Memory Blocks** – Store data and programs.  
  - **Peripherals** – Enable input/output and analog interfacing.  
  - **Interconnect** – Links all modules for data transfer.  
- **Categories of SoCs:** Includes microcontroller-based, processor-based, and specialized application SoCs.  
- **Design Stages:** `Specification → Modeling → RTL implementation → Verification → Synthesis → Physical design`.  
- **VSDBabySoC Features:**  
  - RVMYTH RISC-V CPU for data processing.  
  - 8× PLL for stable clock generation.  
  - 10-bit DAC for digital-to-analog conversion.  
  - Simple architecture aimed at testing multiple IP cores.  
- **Functional Modeling Role:** Acts as an early validation step to check architecture behavior before detailed RTL and layout, ensuring efficient design iterations.

👉 By working with BabySoC, learners get hands-on exposure to **SoC architecture, clocking systems, CPU-peripheral communication**, and analog output generation in a controlled environment.

