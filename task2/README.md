# Overview
The **VSDBabySoC** is a simple SoC (System-on-Chip) design incorporating a RISC-V processor (rvmyth), a PLL (Phase-Locked Loop) module (pll), and a DAC (Digital-to-Analog Converter) module (dac). This project demonstrates integration of these IP cores and aims to simulate and verify the design behavior using pre-synthesis and post-synthesis simulations. VSDBabySoC is a small yet powerful RISCV-based SoC. The main purpose of designing such a small SoC is to test three open-source IP cores together for the first time and calibrate the analog part of it. VSDBabySoC contains one RVMYTH microprocessor, an 8x-PLL to generate a stable clock, and a 10-bit DAC to communicate with other analog devices.

<img width="2270" height="1260" alt="vsdbabysoc_block_diagram" src="https://github.com/user-attachments/assets/d0244596-646b-4a30-bb36-e4fd6078d17d" />

This is the block diagram of VSD BabySoC (System-on-Chip) — a simplified educational SoC model developed by VLSI System Design (VSD) to help learners understand SoC integration concepts. 
## Main Components in the Diagram

**1. rvmyth (RISC-V core)**

- At the center of the SoC is the RISC-V "MYTH" processor (rvmyth).
- It’s a small RISC-V based CPU designed during the “Microprocessor for You in       Thirty Hours (MYTH)” workshop.
- Works in the 1.8V voltage domain.

**2. PLL (avsdpll_1v8)**

- avsdpll - **A**nalog **V**ery **S**mall **D**esign **P**hase **L**ocked **L**oop
- A Phase Locked Loop (yellow block) used for clock generation.
- Inputs: Crystal oscillator (XO, XI pads) and a current source.
- It generates the clock (CLK) signal that drives the RISC-V core.
- Operates in 1.8V voltage domain.

**3. SPI (Serial Peripheral Interface)**

- Handles external communication with the SoC.
- Works in the 3.3V domain (since most IO devices use 3.3V).
- Connected to rvmyth via level shifters (LS).

**4. DAC (avsd_dac_3v3)**

- A Digital-to-Analog Converter block (gray).
- Input: Digital data bus D[9:0] (10-bit).
- Output: Analog signal (OUT).
- Operates in the 3.3V domain (to drive external analog circuits).
- Reference voltages: VREFL and VREFH.

**Voltage Domains**
The SoC uses two voltage domains:
  - 1.8V domain → Used internally (rvmyth core, PLL).
  - 3.3V domain → Used for external interfaces (SPI, DAC).
Since the SoC has both low-voltage core logic (1.8V) and higher-voltage IO (3.3V), it needs Level Shifters (LS) for safe communication between domains.

**Other Signals**
- adc_low, adc_high → Pins for ADC (analog-to-digital converter) reference input.
- Current Source (5 µA / 10 µA) → Used by PLL for biasing.
- Crystal Oscillator Pads (XO, XI) → Provide the input reference clock to PLL.

**How it Works Together**
- PLL generates a stable clock from the crystal oscillator.
- rvmyth RISC-V CPU executes instructions using that clock (1.8V domain).
- SPI interface allows external communication (e.g., loading programs, data).
- DAC converts digital signals from rvmyth into analog voltages (for mixed-signal applications).
- Level shifters ensure safe voltage translation between 1.8V and 3.3V blocks.

rvmyth - RISC-V (Instruction Set Architecture) Microprocessor for You in Thirty Hours (a VSD workshop where this RISC-V core was developed).

Reason for different voltages
**CPU & PLL in 1.8V domain**
Reason:
- The CPU (rvmyth) and PLL are core logic blocks.
- Modern digital logic (standard cells in Sky130 PDK) is optimized to work at lower voltages (1.8V).
Benefits:
✅ Lower power consumption (since dynamic power ∝ V²).
✅ Reduced heat dissipation.
✅ Higher transistor density and faster switching.
- The PLL is also in 1.8V because it drives the clock network for the CPU — both must stay in the same domain to avoid unnecessary level shifting.

**DAC in 3.3V domain**
Reason:
- The DAC (Digital-to-Analog Converter) produces analog output signals.
- External devices (sensors, actuators, audio circuits, measurement equipment, etc.) usually require higher voltage swing (e.g., 0–3.3V) for better noise immunity and interfacing.
- Analog circuits are more robust and accurate with larger voltage headroom.
- DAC reference voltages (VREFL, VREFH) are typically tied to IO supply rails, which are 3.3V in most microcontroller-style SoCs.

**Why not put everything in 3.3V?**
- If the CPU and PLL were in 3.3V:
❌ Power consumption would skyrocket (V² factor).
❌ Reliability issues (higher electric fields in tiny transistors → faster aging).
❌ Lower maximum clock speed (transistors switch slower at higher voltages in deep-submicron nodes).
So designers separate:
- Core (digital logic, CPU, PLL) → 1.8V
- IO and analog (DAC, SPI, pads) → 3.3V


## Problem Statement
This work discusses the different aspects of designing a small SoC based on RVMYTH (a RISCV-based processor). This SoC will leverage a PLL as its clock generator and controller and a 10-bit DAC as a way to talk to the outside world. Other electrical devices with proper analog input like televisions, and mobile phones could manipulate DAC output and provide users with music sound or video frames. At the end of the day, it is possible to use this small fully open-source and well-documented SoC which has been fabricated under Sky130 technology, for educational purposes.

## What is SoC
An SoC is a single-die chip that has some different IP cores on it. These IPs could vary from microprocessors (completely digital) to 5G broadband modems (completely analog).

## What is RVMYTH
RVMYTH core is a simple RISCV-based CPU, introduced in a workshop by RedwoodEDA and VSD. During a 5-day workshop students (including middle-schoolers) managed to create a processor from scratch. The workshop used the TLV for faster development. All of the present and future contributions to the IP will be done by students and under open-source licenses.

## What is PLL
A phase-locked loop or PLL is a control system that generates an output signal whose phase is related to the phase of an input signal. PLLs are widely used for synchronization purposes, including clock generation and distribution.

## What is DAC
A digital-to-analog converter or DAC is a system that converts a digital signal into an analog signal. DACs are widely used in modern communication systems enabling the generation of digitally-defined transmission signals. As a result, high-speed DACs are used for mobile communications and ultra-high-speed DACs are employed in optical communications systems.

## VSDBabySoC Modeling
Here we are going to model and simulate the VSDBabySoC using iverilog, then we will show the results using gtkwave tool. Some initial input signals will be fed into vsdbabysoc module that make the pll start generating the proper CLK for the circuit. The clock signal will make the rvmyth to execute instructions in its imem. As a result the register r17 will be filled with some values cycle by cycle. These values are used by dac core to provide the final output signal named OUT. So we have 3 main elements (IP cores) and a wrapper as an SoC and of-course there would be also a testbench module out there.

Please note that in the following sections we will mention some repos that we used to model the SoC. However the main source code is resided in Source-Code Directory and these modules are in Modules Sub-Directory.

## RVMYTH modeling
As we mentioned in What is RVMYTH section, RVMYTH is designed and created by the TL-Verilog language. So we need a way for compile and trasform it to the Verilog language and use the result in our SoC. Here the sandpiper-saas could help us do the job.

Here is the repo we used as a reference to model the RVMYTH

## PLL and DAC modeling
It is not possible to sythesis an analog design with Verilog, yet. But there is a chance to simulate it using real datatype. We will use the following repositories to model the PLL and DAC cores:

Here is the repo we used as a reference to model the PLL
Here is the repo we used as a reference to model the DAC
CAUTION: In the beginning of the project, we get our verilog model of the PLL from here. However, by proceeding the project to the physical design flow we realize that this model needs a little changes to become sufficient for a real IP core. So we changed it a little and created a new model named AVSDPLL based on this IP
