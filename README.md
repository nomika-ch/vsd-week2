# System on Chip

A SoC **(System on Chip)** is an integrated circuit (IC) that combines all the essential components of a complete electronic system into a single chip.

Instead of having separate chips for processor, memory, communication interfaces, and peripherals, a SoC integrates them together.

### Typical Components of a SoC:
- **Processor core(s)**: CPU (like ARM, RISC-V, etc.) or multiple cores for performance.
- **Memory blocks**: RAM, ROM, cache, sometimes embedded Flash.
- **I/O interfaces**: USB, SPI, I²C, UART, GPIO, etc.
- **Accelerators**: DSPs, GPUs, AI accelerators, encryption engines.
- **Analog blocks**: ADCs, DACs, PLLs, voltage regulators.
- **Interconnect**: High-speed bus or network-on-chip (NoC) that links everything.

### Example Devices using SoCs:
- Smartphones (Qualcomm Snapdragon, Apple A-series, MediaTek).
- Embedded systems (Raspberry Pi’s Broadcom SoC).
- Automotive controllers.
- IoT devices.

### Advantages of SoC:
- Compact (everything on one chip).
- Low power consumption (important for mobile devices).
- Cost-effective in high volumes.
- Higher performance due to tight integration.

# RVMYTH microprocessor

RVMYTH is an open-source RISC-V based microprocessor core that was co-developed by VSD (VLSI System Design) community in collaboration with RVMyth Shakti processor team (IIT Madras).

### Key Points about RVMYTH:

- **RISC-V ISA**: Implements a simple subset of the RISC-V instruction set.
- **Open-source**: Available for anyone to download, study, and modify.
- **Educational focus**: Designed to help students and engineers understand:
          How a microprocessor works at RTL level.
          The flow from RTL → Synthesis → Physical design → GDSII.
- **Part of VSDSquadron board**: RVMYTH is the CPU core used inside the VSDSquadron, an educational RISC-V development board.

## RVMYTH vs Commercial RISC-V Cores (e.g., SiFive, Andes, etc.)

### 1. Purpose

**RVMYTH:**
- Primarily educational and experimental.
- Built to help students, hobbyists, and engineers learn RISC-V ISA, processor design, and SoC flow using open-source tools.
- Not optimized for high performance or power efficiency.

**Commercial RISC-V cores (e.g., SiFive E31, U74, Andes cores):**
- Industry-grade, designed for real products (IoT, AI, automotive, mobile, servers).
- Optimized for power, performance, area (PPA).
- Extensively verified for bug-free silicon.

### 2. Complexity

**RVMYTH:**
- Simple, small subset of the RV32I ISA.
- Usually single-cycle or multi-cycle CPU (not pipelined).- 
No cache, MMU, branch predictors, or advanced accelerators.

**Commercial cores:**
- Full ISA support: RV32/64 with optional extensions (M, A, F, D, V, etc.).
- Complex microarchitecture: pipelines, out-of-order execution, superscalar, branch prediction.
- Support for cache hierarchies, virtual memory (MMU), and coherency protocols.

### 3. Verification & Reliability

**RVMYTH:**
- Verified mainly with simulation testbenches and educational projects.
- Limited corner-case coverage.
- Intended for FPGA boards and learning environments.

**Commercial cores:**
- Verified with industrial-grade EDA flows (Synopsys VCS, Cadence Xcelium, JasperGold).
- Formal verification, emulation, FPGA prototyping.
- Millions of verification cycles before tapeout → tapeout-ready.

### 4. Target Use

**RVMYTH:**
- Teaching tool (universities, workshops).
- Hobbyist projects.
- Demonstrations of open-source chip design (RTL → GDSII).

**Commercial cores:**
- Smartphones, AI accelerators, IoT devices, automotive systems, servers.
- Licensed to semiconductor companies for real silicon production.

### 5. Support & Ecosystem

**RVMYTH:**
- Community support (VSD, GitHub repos).
- Documentation geared towards beginners.

**Commercial cores:**
- Backed by companies with customer support, SDKs, compiler toolchains, and OS support (Linux, RTOS).

## Main Components of a Typical SoC

### 1. Processor Cores
- **CPU**: Central Processing Unit (ARM, RISC-V, MIPS, etc.), often multi-core.
- **GPU**: Graphics Processing Unit for graphics/parallel processing.
- **DSP/AI accelerators**: Specialized units for signal processing, ML/AI tasks.

### 2. Memory System
- **On-chip memory**: SRAM, cache (L1, L2, L3).
- **ROM / eFlash**: For boot code or firmware.
- **External memory controllers**: DRAM (DDR, LPDDR) interfaces.

### 3. Interconnect / Bus
- Connects all components. Examples:
  - AMBA (AXI/AHB/APB) in ARM-based SoCs.
  - NoC (Network-on-Chip) in large SoCs.

 ### 4. I/O Interfaces
- **Serial interfaces**: UART, SPI, I²C, CAN.
- **High-speed interfaces**: PCIe, USB, SATA, HDMI, MIPI.
- **Networking**: Ethernet, Wi-Fi, Bluetooth.
- **GPIO**: General-purpose input/output pins.

### 5. Peripherals
- Timers, watchdogs.
- PWM generators.
- Real-time clocks (RTC).
- Security modules (crypto engines, secure boot).

### 6. Analog & Mixed-Signal Blocks
- **ADC / DAC**: Analog-to-digital and digital-to-analog converters.
- **PLL / Clock generators.**
- Power management circuits (voltage regulators, LDOs).

### 7. System Management
- **Power management unit (PMU)**: Low-power modes, battery control.
- Reset and clock control.
- Debug & trace logic (JTAG, debug probes).

### 8. Optional Specialized Blocks
- Video codec units (H.264, H.265, AV1).
- Image signal processors (ISP) for cameras.
- Neural Processing Units (NPUs) for AI.


## What is BabySoC?
- BabySoC is an educational, minimal SoC created by the VSD community.
- It integrates the **RVMYTH RISC-V ** core with just a few essential components.
- Designed as a “hello world SoC” → simple enough for beginners but still teaches the real flow of SoC design.

## Why BabySoC is Simplified?
### Minimal Components
- Only the CPU core (RVMYTH) + a simple peripheral (e.g., PWM or GPIO) + basic memory/clock/reset.
- Leaves out complex units like cache, GPU, AI accelerators, DDR controllers.

### Easy to Understand
- Block diagram and RTL are small, so beginners can trace signals end-to-end.
- Students can clearly see how CPU interacts with memory and peripheral.

### Focus on SoC Concepts, Not Complexity
- Teaches the essentials:
   - CPU integration
   - Bus communication (simple bus instead of AXI/NoC)
   - Memory-mapped peripheral access
   - Clock/reset strategy
- Avoids overwhelming learners with real-world complexity.

### Smooth Tool Flow
- Works well with open-source EDA tools (Icarus, Yosys, OpenSTA, OpenLane).
- Can be synthesized, simulated, and taken up to GDSII layout without hitting tool/memory limits.

### Hands-On Learning
- Lets students write software (firmware) for the CPU and see it control a peripheral (like blinking an LED or PWM wave).
- Bridges the gap between computer architecture (theory) and chip design (practice).

### Stepping Stone
- Once learners are comfortable with BabySoC, they can scale up to full SoC projects (with caches, interconnects, multiple IPs).

**BabySoC is simplified because it strips the SoC down to just the essentials (CPU + peripheral + memory + basic glue logic).**

## Role of Functional Modeling before RTL & Physical Design

### 1. What is Functional Modeling?
- A high-level model of the design, usually written in C, C++, SystemC, or Python, sometimes even in transaction-level modeling (TLM).
- Describes what the system should do, not how it will be implemented at the hardware level.
- Example: For a CPU core, a functional model executes instructions correctly, but it doesn’t show pipelines, clock cycles, or timing.

### 2. Why Functional Modeling is Needed Before RTL?
- **Early validation of functionality**
    - Checks whether the architecture meets the specifications before investing time in RTL coding.
    - Example: Test if an instruction set simulator (ISS) of a CPU runs programs correctly.
- **Fast simulation**
    - Functional models run much faster than RTL simulations, allowing quick verification of system-level behavior.

- **System architecture exploration**
   - Designers can experiment with cache sizes, bus width, memory organization, etc., at a high level before finalizing RTL.

- **Software development can start early**
   - Firmware/OS teams can write and test software on the functional model before silicon exists.

### 3. Relation to RTL Design Stage
- RTL focuses on cycle-accurate hardware implementation (flip-flops, timing, pipelines).
- Without functional modeling, designers risk building incorrect RTL that matches implementation details but doesn’t meet the system requirements.
- Functional modeling provides a golden reference model to compare RTL simulation results against.

### 4. Relation to Physical Design Stage
- Physical design (synthesis, floorplanning, placement, routing) is time-consuming and costly.
- Doing this before functional correctness is validated would waste resources.
- Functional modeling ensures the specifications are correct and frozen before moving to RTL → synthesis → layout.
