# 🖥️ RTL-to-GDSII Implementation of a RISC-V Core (PicoRV32) on Sky130 PDK

## 📌 Project Overview
This project demonstrates the **end-to-end VLSI design flow** of a RISC-V core ([PicoRV32](https://github.com/YosysHQ/picorv32)) using **Cadence EDA tools** and the open-source [Skywater 130nm PDK](https://github.com/google/skywater-pdk).

The flow covers:
- RTL → Gate-level synthesis  
- Floorplanning → Placement → CTS → Routing  
- Timing, Power, and Physical Verification  
- Exporting **tapeout-ready GDSII**

The objective is to gain hands-on experience with backend integration, **PPA (Power, Performance, Area) trade-offs**, and **sign-off closure** in a realistic ASIC flow.

---

## 🚀 Why This Project is Important
- **Hands-on with industry-standard flow** → Mimics real-world ASIC methodology.  
- **Open-source RISC-V core** → Compact, configurable, and widely adopted for research.  
- **Open PDK (Sky130)** → First-ever open-source CMOS PDK for fabrication & academia.  
- **Cadence Toolchain** → [Genus](https://www.cadence.com/en_US/home/tools/digital-design-and-signoff/synthesis/genus-synthesis-solution.html), [Innovus](https://www.cadence.com/en_US/home/tools/digital-design-and-signoff/implementation/innovus-implementation-system.html), [Tempus](https://www.cadence.com/en_US/home/tools/digital-design-and-signoff/timing-signoff/tempus-timing-signoff-solution.html), [Voltus](https://www.cadence.com/en_US/home/tools/system-analysis/voltus-ic-power-integrity-solution.html), [Pegasus](https://www.cadence.com/en_US/home/tools/digital-design-and-signoff/physical-verification/pegasus-physical-verification-system.html).  
- **Tapeout-ready methodology** → Includes PPA optimization and IR/EM sign-off.

---

## 🛠️ High-Level Flow
1. **RTL Selection** → [PicoRV32 Verilog source](https://github.com/YosysHQ/picorv32).  
2. **Logic Synthesis (Genus)** → RTL → Gate-level netlist mapped to Sky130 standard cells.  
3. **Formal Verification (Conformal LEC)** → RTL vs. Netlist equivalence.  
4. **Floorplanning (Innovus)** → Die/core sizing, I/O placement, power grid.  
5. **Placement** → Cell placement with congestion/timing optimization.  
6. **Clock Tree Synthesis (CTS)** → Balanced skew & clock insertion delay.  
7. **Routing (Innovus)** → Global + detailed routing.  
8. **Power Analysis (Voltus)** → IR-drop & EM verification.  
9. **Timing Closure (Tempus)** → STA across corners.  
10. **Physical Verification (Pegasus/PVS)** → DRC/LVS/ERC clean.  
11. **GDSII Export** → Final manufacturable layout.

---

## 🔹 What is PicoRV32?
[PicoRV32](https://github.com/YosysHQ/picorv32) is a **compact, lightweight RISC-V CPU core** in Verilog.  
- Implements **RV32IMC** (32-bit Integer + Multiply + Compressed instructions).  
- Configurable pipeline (1–4 stages).  
- Can enable/disable multiplier, shifter, compressed ISA.  
- Widely used in **FPGA prototyping & ASIC tapeouts**.

---

## 🔹 Why PicoRV32 for This Project?
- Synthesizable RTL → Works with **Cadence Genus**.  
- Small size → Feasible on university compute resources.  
- Proven in academic tapeouts (e.g., **[TinyTapeout](https://tinytapeout.com/)**).  
- A real CPU core for **full RTL-to-GDS validation**.

---

## ⚙️ Detailed Design Flow

### 1️⃣ RTL Design & Simulation
- RTL Source: `picorv32.v`  
- Simulated in Cadence **nclaunch** (or any Verilog simulator).  
✅ Functionally verified RTL.

### 2️⃣ Logic Synthesis (Cadence Genus)
**Inputs**: RTL (`picorv32.v`), Sky130 library (`sky130_fd_sc_hd__tt_025C_1v80.lib`), Constraints (`picorv32.sdc`)  
**Outcome**: Netlist + timing/area/power reports.

### 3️⃣ Logic Equivalence Check (Conformal LEC)
Ensures **RTL ↔ Synthesized Netlist equivalence**.  
✅ No mismatches introduced.

### 4️⃣ Floorplanning (Innovus)
Defined **core area, die size, pin placement, power rings/straps**.  
✅ DEF with floorplan + power grid.

### 5️⃣ Placement
Optimized placement with `place_opt_design`.  
✅ Reduced congestion & improved timing.

### 6️⃣ Clock Tree Synthesis (CTS)
Inserted buffers/inverters for balanced clocks.  
✅ Post-CTS netlist.

### 7️⃣ Routing
`routeDesign` → Global + detailed routing.  
✅ SPEF + DRC-clean routed design.

### 8️⃣ Static Timing Analysis (Tempus)
Multi-corner STA for setup/hold closure.  
✅ WNS ≥ 0, TNS = 0.

### 9️⃣ Power Integrity (Voltus)
- IR-drop  
- EM Analysis  
✅ IR/EM clean.

### 🔟 Physical Verification (Pegasus / PVS)
- DRC  
- LVS  
- ERC  
✅ Verification clean.

### 1️⃣1️⃣ GDSII Export
Final GDSII → `picorv32.gds`  
✅ Tapeout-ready!

---

## 📊 Reports

### ⏱️ Timing Report
| Metric                     | Value           |
| -------------------------- | --------------- |
| Target Clock Period        | 10 ns (100 MHz) |
| Worst Negative Slack (WNS) | **0.00 ns**     |
| Total Negative Slack (TNS) | **0.00 ns**     |
| Violating Paths            | 0               |

✅ Timing is clean.

---

### 📐 Area Report
| Metric               | Value           |
| -------------------- | --------------- |
| Top Module           | `picorv32_main` |
| Total Standard Cells | 6,459           |
| Cell Area            | 69,708 µm²      |
| Total Area           | 69,708 µm²      |

---

### ⚡ Power Report
| Component | Leakage (W) | Internal (W) | Switching (W) | Total (W) | Contribution |
| --------- | ----------- | ------------ | ------------- | --------- | ------------ |
| Registers | 4.57e-06    | 7.11e-03     | 4.05e-04      | 7.52e-03  | 83.6%        |
| Logic     | 4.84e-06    | 7.38e-04     | 7.35e-04      | 1.48e-03  | 16.4%        |
| Others    | ~0          | ~0           | ~0            | ~0        | 0%           |

**Total Power**: 8.99 mW  
- Leakage: 0.01 mW (0.1%)  
- Internal: 7.85 mW (87.2%)  
- Switching: 1.14 mW (12.7%)

---

### 📊 QoR Report
| Metric         | Value        |
| -------------- | ------------ |
| Target Clock   | 10 ns        |
| Achieved Slack | 0.00 ns      |
| Cell Count     | 6,459        |
| Area           | 69,708 µm²   |
| Power          | 8.99 mW      |

✅ Meets PPA targets.

---

## 📌 Summary
| Metric           | Result      |
| ---------------- | ----------- |
| Frequency Target | 100 MHz     |
| WNS / TNS        | 0.00 / 0.00 |
| Standard Cells   | 6,459       |
| Total Area       | 69,708 µm²  |
| Total Power      | 8.99 mW     |

📂 **Full Reports:**  
- [timing.rpt](https://github.com/SRB-TYAGI/RISC-V-PROCESSOR/blob/main/1.Genus/2.Reports/timing.rpt)  
- [area.rpt](https://github.com/SRB-TYAGI/RISC-V-PROCESSOR/blob/main/1.Genus/2.Reports/area.rpt)  
- [power.rpt](https://github.com/SRB-TYAGI/RISC-V-PROCESSOR/blob/main/1.Genus/2.Reports/power.rpt)  
- [report_qor.rpt](https://github.com/SRB-TYAGI/RISC-V-PROCESSOR/blob/main/1.Genus/2.Reports/report_qor.rpt)  

---

## 🔗 References
- [PicoRV32 GitHub](https://github.com/YosysHQ/picorv32)  
- [SkyWater Sky130 PDK](https://github.com/google/skywater-pdk)  
- [TinyTapeout Project](https://tinytapeout.com/)  
- [Cadence Innovus](https://www.cadence.com/en_US/home/tools/digital-design-and-signoff/implementation/innovus-implementation-system.html)  
- [RISC-V Foundation](https://riscv.org/)  

