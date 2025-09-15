# 🖥️ RTL-to-GDSII Implementation of a RISC-V Core (PicoRV32) on Sky130 PDK
## 📌 Project Overview

This project demonstrates the end-to-end VLSI design flow of a RISC-V core (PicoRV32) using Cadence EDA tools and the Skywater 130nm (Sky130) open-source PDK.

The design begins with Register Transfer Level (RTL) Verilog and proceeds through logic synthesis, floorplanning, placement, clock tree synthesis (CTS), routing, timing sign-off, power analysis, and physical verification, concluding with a manufacturable GDSII layout.

The goal is to understand backend integration, PPA (Power, Performance, Area) trade-offs, and sign-off closure in a real ASIC flow.

# Why This Project is Important

Hands-on with industry-standard flow → This project replicates exactly how real-world ASICs are designed in semiconductor companies.

Open-source RISC-V core → PicoRV32 is compact, well-documented, and ideal for academic/learning purposes.

Open PDK (Sky130) → The world’s first fully open-source 130nm CMOS PDK, enabling fabrication and research without proprietary restrictions.

Cadence Toolchain → Industry-standard EDA suite (Genus, Innovus, Tempus, Voltus, Pegasus) ensures realistic design experience.

Tapeout-ready methodology → Includes PPA (Power, Performance, Area) optimization and IR/EM sign-off, preparing the design for real silicon fabrication.


# High-Level Flow (Step-by-Step)

RTL Selection → Use PicoRV32 Verilog source as the CPU core.

Logic Synthesis (Genus) → Convert RTL into a gate-level netlist mapped to Sky130 standard cells.

Formal Verification (LEC with Conformal) → Compare RTL vs. synthesized netlist to ensure functional equivalence (no mismatches introduced by synthesis).

Floorplanning (Innovus) → Define die/core area, I/O placement, and power distribution network.

Placement → Place standard cells while optimizing congestion and timing.

Clock Tree Synthesis (CTS) → Insert clock buffers/inverters, balance skew and insertion delay.

Routing (Innovus) → Global + detailed routing of signals and power networks.

Power Analysis (Voltus) → Perform IR-drop and Electromigration checks for reliability.

Timing Closure (Tempus) → Run Static Timing Analysis across multiple process/voltage/temperature (PVT) corners.

Physical Verification (Pegasus / Assura / PVS) → Ensure design is DRC/LVS/ERC clean.

GDSII Export → Generate the final layout (.gds file) ready for tapeout.

# 🔹 What is PicoRV32?

PicoRV32 is a small, simple, and compact RISC-V CPU core written in Verilog RTL.It is an open-source RTL design available on GitHub.It implements the RV32IMC instruction set (32-bit integer + multiplication + compressed instructions).
It is highly configurable:

We can choose pipeline stages (1–4 stages).We can enable/disable multiplier, barrel shifter, compressed instructions, etc.Very lightweight — can run on FPGAs and ASICs.

# 🔹 Why PicoRV32 for my project?

Since I am targeting RTL-to-GDS flow on SKY130 with Cadence Innovus, PicoRV32 is perfect because:

It’s synthesizable RTL Verilog (works with Cadence Genus for synthesis).

It’s small enough to complete within university compute resources.

It has been used in several academic tapeouts (OpenLane, Sky130, TinyTapeout).

Gives me a real CPU core to test the full ASIC flow (synthesis → P&R → signoff).

# ⚙️ Detailed Design Flow
1. RTL Design & Simulation

RTL source: picorv32.v (PicoRV32 RISC-V processor core).

Verified using a testbench in Cadence nclaunch (or any Verilog simulator).

Outcome: Functionally correct RTL.

# 2. Logic Synthesis (Cadence Genus)

Inputs:

RTL (picorv32.v)

Standard cell library (sky130_fd_sc_hd__tt_025C_1v80.lib)

Constraints (picorv32.sdc)

Process:

RTL is elaborated and optimized.

Mapped onto Sky130 standard cells.

Generated timing/area/power reports

# 3. Logic Equivalence Check (Cadence Conformal LEC)

Purpose: Verify that the synthesized netlist is functionally equivalent to the RTL.

Inputs:

RTL (picorv32.v)

Synthesized netlist (picorv32_syn.v)

Standard cell models (sky130_fd_sc_hd.v)

Process:

Load RTL (golden) and gate netlist (revised).

Run equivalence check.

Example commands:

read design rtl/picorv32.v -golden
read design results/picorv32_syn.v -revised
set system mode lec
add compare point -all
compare
report verification -status


Outcome: RTL and netlist proven equivalent (no functional changes introduced by synthesis).

# 4. Floorplanning (Cadence Innovus)

Imported synthesized netlist + constraints.

Defined:

Core area and die size.

Pin placement.

Power rings and straps.

Outcome: DEF with floorplan and power grid.

# 5. Placement

Placed all standard cells inside core area.

Optimized to reduce congestion and improve timing.

Command:
place_opt_design

Outcome: Placed DEF + netlist.

# 6. Clock Tree Synthesis (CTS)

Inserted buffers/inverters for clock distribution.

Balanced skew and insertion delay.

Outcome: Post-CTS netlist with balanced clock tree.

# 7. Routing

Global routing + detailed routing.

Fixed any design rule violations.

Command:
routeDesign

Outcome: Fully routed design + parasitics (SPEF).

# 8. Static Timing Analysis (Cadence Tempus)

Checked setup/hold timing across multiple corners.

Outcome: Timing closed design (WNS ≥ 0, TNS = 0).

# 9. Power Integrity (Cadence Voltus)

Performed IR-drop and Electromigration (EM) analysis.

Verified power distribution network is reliable under load.

Outcome: IR/EM clean design.

# 10. Physical Verification (Cadence Pegasus / PVS)

Ran sign-off verification with Sky130 rule decks:

DRC (Design Rule Check)

LVS (Layout vs. Schematic)

ERC (Electrical Rule Check)

Outcome: Design is DRC/LVS/ERC clean.

# 11. GDSII Generation

Exported final layout for tapeout:

streamOut results/picorv32.gds -mapFile sky130.map

Outcome: Final GDSII file (picorv32.gds).


# 1️⃣ Timing Report

Source: reports/timing.rpt


| Metric                     | Value           |
| -------------------------- | --------------- |
| Target Clock Period        | 10 ns (100 MHz) |
| Worst Negative Slack (WNS) | **0.00 ns**     |
| Total Negative Slack (TNS) | **0.00 ns**     |
| Violating Paths            | 0               |

✅ Timing is clean – no setup/hold violations.

# 2️⃣ Area Report

Source: reports/area.rpt

| Metric               | Value                  |
| -------------------- | ---------------------- |
| Top Module           | `picorv32_main`        |
| Total Standard Cells | 6,459                  |
| Cell Area            | 69,708.105 µm²         |
| Wire Area            | 0 (from library model) |
| Total Area           | 69,708.105 µm²         |

ℹ️ Area reported at RTL synthesis level (pre-placement).

# 3️⃣ Power Report

Source: reports/power.rpt

| Component | Leakage (W) | Internal (W) | Switching (W) | Total (W) | Contribution |
| --------- | ----------- | ------------ | ------------- | --------- | ------------ |
| Registers | 4.57e-06    | 7.11e-03     | 4.05e-04      | 7.52e-03  | 83.6%        |
| Logic     | 4.84e-06    | 7.38e-04     | 7.35e-04      | 1.48e-03  | 16.4%        |
| Others    | \~0         | \~0          | \~0           | \~0       | 0%           |

Total Power: 8.998e-03 W ≈ 8.99 mW

Leakage: ~0.01 mW (0.1%)

Internal: ~7.85 mW (87.2%)

Switching: ~1.14 mW (12.7%)

# 4️⃣ QoR Report (Quality of Results)

Source: reports/report_qor.rpt

| Metric         | Value        |
| -------------- | ------------ |
| Target Clock   | 10 ns        |
| Achieved Slack | 0.00 ns      |
| Cell Count     | 6,459        |
| Area           | 69,708.1 µm² |
| Power          | 8.99 mW      |

✅ Design meets PPA (Performance, Power, Area) targets.

# 📌 Summary Table

| Metric           | Result      |
| ---------------- | ----------- |
| Frequency Target | 100 MHz     |
| WNS / TNS        | 0.00 / 0.00 |
| Standard Cells   | 6,459       |
| Total Area       | 69,708 µm²  |
| Total Power      | 8.99 mW     |

🔗 Full Reports

timing.rpt
area.rpt
power.rpt
report_qor.rpt

