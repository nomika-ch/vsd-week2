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
