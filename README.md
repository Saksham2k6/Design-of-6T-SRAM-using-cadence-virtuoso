# ⚡ Cadence Virtuoso 6T SRAM Custom IC Design & Verification

![Cadence Virtuoso](https://img.shields.io/badge/Cadence-Virtuoso-blue.svg?logo=cadence)
![Technology Node](https://img.shields.io/badge/Node-45nm-lightgrey.svg)
![Status](https://img.shields.io/badge/Status-LVS_Clean-success.svg)
![DRC](https://img.shields.io/badge/DRC-Zero_Errors-brightgreen.svg)

## 📌 Project Overview
This repository contains the complete custom IC design, analysis, and physical implementation of a **6-Transistor (6T) Static Random-Access Memory (SRAM)** architecture. Designed from scratch in Cadence Virtuoso using a **45nm CMOS technology node**, this project covers the full custom VLSI design cycle. 

The implementation scales hierarchically from a highly optimized single bit-cell up to a fully functional 8x8 memory array, complete with integrated peripheral read/write control circuitry. 

## 📖 Table of Contents
1. [Design Specifications](#-design-specifications)
2. [Core 6T Cell & Stability Analysis](#-1-core-6t-cell--dc-stability-analysis)
3. [Peripheral Control Circuitry](#-2-peripheral-control-circuitry)
4. [Hierarchical Array Integration](#-3-hierarchical-array-integration)
5. [Transient System Verification](#-4-transient-system-verification)
6. [Physical Design (Layout)](#-5-physical-design-layout)
7. [Physical Verification & PEX](#-6-physical-verification-drc--lvs--pex)
8. [Tool Stack](#%EF%B8%8F-tool-stack)

---

## 📊 Design Specifications

| Parameter | Specification | Description |
| :--- | :--- | :--- |
| **Technology Node** | 45nm CMOS | Standard generic Process Design Kit (PDK). |
| **Architecture** | 6T Cell | Cross-coupled inverters with dual NMOS access transistors. |
| **Array Size** | 64-bit (8x8) | 8-bit word length with 8 addressable rows. |
| **Verification Status**| DRC / LVS Clean | Zero density, spacing, or connectivity violations. |

---

## 🏗️ 1. Core 6T Cell & DC Stability Analysis

The fundamental building block is the 6T SRAM cell. Sizing is hyper-critical in SRAM design to prevent destructive reads and ensure write-ability. 

* **Cell Ratio (CR):** `(W/L)_driver / (W/L)_access` — Optimized for Read Stability.
* **Pull-Up Ratio (PR):** `(W/L)_load / (W/L)_access` — Optimized for Write Margin.

* **Core 6T SRAM Schematic:** ![Schematic](assets/04_core_6t_sram_schematic_.png) 
* **Symbol:** ![Symbol](assets/08_sram_6t_symbol.png)

### DC Stability Analysis & VTC
To extract the Static Noise Margin (SNM), a DC sweep was performed on the internal storage nodes to generate the Butterfly Curves (Voltage Transfer Characteristics).

* **DC Testbench Schematic:** ![Testbench](Schematic_.png)
* **ADE L Simulation Setup:** ![ADE Setup](ADE%20L%20setup_.png)
* **Voltage Transfer Characteristics (VTC):** ![VTC Graph](Output%20Graph.png)

---

## ⚡ 2. Peripheral Control Circuitry

To facilitate high-speed, reliable Read/Write operations, custom transistor-level peripheral circuits were developed:

* **Sense Amplifier:** A high-gain differential amplifier that detects minute voltage differentials on the bitlines during a Read cycle.
    * ![Schematic](assets/01_sense_amp_schematic_.png) | ![Symbol](assets/11_sense_amp_symbol.png)
* **Precharge Circuit:** Responsible for equalizing both BL and BLB to VDD prior to any memory access.
    * ![Schematic](assets/02_precharge_schematic_.png) | ![Symbol](assets/10_precharge_symbol.png)
* **Row Decoder Slice:** Translates the incoming memory address to activate the precise Wordline (WL).
    * ![Schematic](assets/03_decoder_slice_schematic_.png) | ![Symbol](assets/09_decoder_slice_symbol.png)

---

## 🧩 3. Hierarchical Array Integration

The system follows a strict bottom-up hierarchical integration strategy, managing complexity by cascading validated sub-blocks.

* **8-Bit Word/Column:** ![Schematic](assets/05_sram_8bit_column_schematic.png) | ![Symbol](assets/12_sram_8bit_column_symbol.png)
* **8x8 Array Top Level:** The complete 64-bit integrated memory block.
    * ![Schematic](assets/06_sram_8x8_array_schematic.png) | ![Symbol](assets/13_sram_8x8_array_symbol.png)
* **System Testbench:** ![Schematic](assets/07_sram_system_testbench.png)

---

## 📈 4. Transient System Verification

To validate the timing, delay, and functional correctness of the integrated array, full transient simulations were executed. 

* **System Transient Response:**
    ![Transient Response](assets/22_system_transient_response_.png)
    > *Figure: Transient simulation verifying wordline assertion, bitline discharging, data latching, and sense-amp triggering over time. Validates successful state changes without read-disturb failures.*

---

## 🗺️ 5. Physical Design (Layout)

The physical layout of the 6T cell was painstakingly drawn to optimize silicon area (high packing density) while minimizing critical internal node capacitance.

* **Cell Layout (Full):** ![Full](assets/17_sram_cell_layout_wide.png)
* **Cell Layout (Zoom Detail):** ![Detail](assets/18_sram_cell_layout_zoom.png)

---

## 🔍 6. Physical Verification (DRC & LVS) & PEX

The physical layout was verified against strict foundry constraints to ensure tape-out readiness and electrical fidelity.

### Design Rule Check (DRC)
Passed all base geometry, spacing, width, and localized density rules.
![Density Checks](assets/16_drc_density_checks.png)

### Layout vs. Schematic (LVS)
Achieved a 100% clean topological match between the schematic netlist and the extracted layout netlist using **Cadence PVS**.
![Status](assets/14_lvs_run_status_.png) | ![Summary](assets/15_lvs_match_summary.png)

### Parasitic Extraction (PEX)
To prepare the design for highly accurate post-layout simulations, lumped RC parasitic networks were extracted from the physical layout geometries using **Quantus QRC**.
* **Setup:** ![Extraction Setup](assets/20_quantus_extraction_setup.png)
* **Run Details:** ![Details](assets/19_quantus_run_details.png)
* **Netlist Generation Success:** ![Result](assets/21_quantus_success_.png)

---

## 🛠️ Tool Stack
* **Schematic Capture:** Cadence Virtuoso Schematic Editor L
* **Simulation & Analysis:** ADE L / Virtuoso Visualization & Analysis XL / Spectre
* **Physical Layout:** Cadence Virtuoso Layout Suite GXL
* **Physical Verification:** Cadence Physical Verification System (PVS)
* **Parasitic Extraction:** Cadence Quantus QRC

