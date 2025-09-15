# 🚀 RISC-V (PicoRV32) Physical Design Flow – RTL to GDSII  

A complete RTL-to-GDSII implementation of the **PicoRV32 RISC-V Processor Core** using the **SkyWater130 PDK** and Cadence tools.  
This project demonstrates the **entire digital ASIC design flow**: synthesis, formal verification, place & route, clock tree synthesis, routing, signoff (STA, power, DRC/LVS).  

---

## 📑 Table of Contents  
- [📌 Project Overview](#-project-overview)  
- [🚀 Why This Project is Important](#-why-this-project-is-important)  
- [🧩 What is PicoRV32?](#-what-is-picorv32)  
- [📐 High-Level Flow (RTL-to-GDSII)](#-high-level-flow-rtl-to-gdsii)  
- [⚙️ Detailed Flow with Reports](#️-detailed-flow-with-reports)  
  - [1️⃣ RTL Design & Simulation](#1️⃣-rtl-design--simulation)  
  - [2️⃣ Logic Synthesis (Genus)](#2️⃣-logic-synthesis-cadence-genus)  
  - [3️⃣ Logic Equivalence Check (Conformal LEC)](#3️⃣-logic-equivalence-check-cadence-conformal-lec)  
  - [4️⃣–11️⃣ Physical Design Flow](#4️⃣11️⃣-physical-design-flow-innovus--tempus--voltus--pegasus)  
- [📊 Key Reports](#-key-reports)  
  - [Timing Report](#timing-report)  
  - [Area Report](#area-report)  
  - [Power Report](#power-report)  
  - [QoR Report](#qor-report)  
- [📌 Summary Table](#-summary-table)  
- [🔗 Full Reports](#-full-reports-click-to-view)  
- [📚 References](#-references)  

---

## 📌 Project Overview  
This repository documents the **ASIC design of a PicoRV32 RISC-V processor core** implemented using **Sky130 technology** and Cadence EDA tools.  

---

## 🚀 Why This Project is Important  
- Demonstrates the **entire RTL-to-GDSII backend flow**  
- Provides **educational material** for VLSI design students & engineers  
- Includes **full timing, power, area, and signoff reports**  

---

## 🧩 What is PicoRV32?  
**PicoRV32** is a compact, size-optimized **32-bit RISC-V CPU core**.  
- Open-source, simple, and widely used in research.  
- Supports RV32IMC instruction set.  
- Small footprint → perfect for FPGA/ASIC learning projects.  

---

## 📐 High-Level Flow (RTL-to-GDSII)  
1. RTL Coding (Verilog)  
2. Simulation & Verification  
3. Logic Synthesis (Cadence Genus)  
4. LEC (Cadence Conformal)  
5. Floorplanning  
6. Placement  
7. Clock Tree Synthesis (CTS)  
8. Routing  
9. Static Timing Analysis (Tempus)  
10. Power Analysis (Voltus)  
11. DRC/LVS Signoff (Pegasus)  
12. GDSII Generation  

---

## ⚙️ Detailed Flow with Reports  

### 1️⃣ RTL Design & Simulation  
- RTL written in Verilog  
- Simulated using **Icarus Verilog** and waveform viewed in **GTKWave**  

### 2️⃣ Logic Synthesis (Cadence Genus)  
- Generated **gate-level netlist**  
- Reports: Timing, Area, Power, QoR  

### 3️⃣ Logic Equivalence Check (Cadence Conformal LEC)  
- Verified equivalence between RTL and synthesized netlist  

### 4️⃣–11️⃣ Physical Design Flow (Innovus / Tempus / Voltus / Pegasus)  
- Floorplanning → Placement → CTS → Routing  
- Signoff analysis: Timing, Power, DRC, LVS  

---

## 📊 Key Reports  

### Timing Report  
📄 [timing.rpt](reports/timing.rpt)  

### Area Report  
📄 [area.rpt](reports/area.rpt)  

### Power Report  
📄 [power.rpt](reports/power.rpt)  

### QoR Report  
📄 [report_qor.rpt](reports/report_qor.rpt)  

---

## 📌 Summary Table  

| Metric | Value |
|--------|-------|
| Frequency Target | 100 MHz |
| Achieved WNS (Worst Negative Slack) | 0 ns (timing clean) |
| Area | (from area.rpt) |
| Total Power | (from power.rpt) |
| DRC | ✅ Clean |
| LVS | ✅ Clean |

---

## 🔗 Full Reports (Click to View)  
- [timing.rpt](reports/timing.rpt)  
- [area.rpt](reports/area.rpt)  
- [power.rpt](reports/power.rpt)  
- [report_qor.rpt](reports/report_qor.rpt)  

---

## 📚 References  
- [RISC-V Official Site](https://riscv.org/)  
- [PicoRV32 GitHub](https://github.com/cliffordwolf/picorv32)  
- [SkyWater130 PDK](https://github.com/google/skywater-pdk)  
- Cadence Tool Documentation (Genus, Innovus, Tempus, Voltus, Pegasus, Conformal)  

---
