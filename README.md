# 🧠 Design and Verification of a Parameterized Memory Controller

> 📁 VLSI System Project  
> 🛠 RTL Design | Verification | FPGA Implementation  
> 🧑‍💻 By: Vinayak V P  
> 👨‍🏫 For the inspiration  of Deep Logic Labs  

---

## 📌 Table of Contents

- [🔍 Project Overview](#-project-overview)
- [🎯 Objective](#-objective)
- [💡 Key Concepts](#-key-concepts)
- [🧠 System Architecture](#-system-architecture)
  - [1. Single-Port RAM](#1-single-port-ram)
  - [2. Memory Controller](#2-memory-controller)
  - [3. Top-Level Integration](#3-top-level-integration)
  - [4. Testbench Logic](#4-testbench-logic)
- [🚦 Control Flow (How It Works)](#-control-flow-how-it-works)
- [💻 FPGA Implementation (Theory)](#-fpga-implementation-theory)
- [📊 Reports Summary](#-reports-summary)
- [📚 Learnings & Insights](#-learnings--insights)
- [🚀 Future Enhancements](#-future-enhancements)
- [📞 Contact](#-contact)

---

## 🔍 Project Overview

This project demonstrates the **design, verification, and FPGA implementation** of a **memory controller** that interfaces with a **parameterized single-port RAM**. The controller efficiently manages **read and write operations** using decoded addressing and handshake control signals, operating at a **100 MHz clock frequency**.

---

## 🎯 Objective

To design a **reliable, modular, and parameterizable memory controller** that:
- Supports 2D RAM access via row and column.
- Decodes a flat memory address into separate row/column.
- Manages read and write operations through control logic.
- Generates status signals (`ready`, `valid`) for safe synchronization.
- Is synthesizable and deployable on FPGA hardware.

---

## 💡 Key Concepts

| Concept                | Explanation |
|------------------------|-------------|
| **Single-Port RAM**    | A memory that supports one access per clock (either read or write). |
| **Parameterized Design** | Makes the design scalable — any size of memory (rows × columns × bit-width). |
| **Address Decoding**   | Converts a flat address into row and column values for 2D memory. |
| **Control Logic**      | Ensures data is read/written only when conditions are correct (using request, ready, and valid signals). |
| **Handshake Protocol** | Ensures safe communication between memory and external systems using `valid` and `ready`. |
| **Synchronous Operation** | All logic responds to a common clock signal — ensuring predictable behavior. |

---

## 🧠 System Architecture

### 1. 🧾 Single-Port RAM

- Acts as a **2D array** of memory elements.
- Stores data in cells addressed by row and column.
- Performs either a **read** or a **write** in one clock cycle (not both).
- Controlled using signals:
  - **req**: request to access memory
  - **rw**: read (1) or write (0)
  - **Qi**: input data
  - **Qa**: output data

---

### 2. 🕹 Memory Controller

- Accepts a **flat address** (like 4 bits for 16 locations).
- **Decodes** it into:
  - **Row (ar)**: higher-order bits
  - **Column (ac)**: lower-order bits
- Controls:
  - When memory is **ready** for next operation
  - Whether the access is **read** or **write**
- Also monitors the **valid signal** from the RAM to confirm successful reads.

---

### 3. 🧩 Top-Level Integration

- **Connects** the controller and datapath.
- External interface receives:
  - Clock, Reset
  - Address, Data input, Read/Write request
- Internally connects controller outputs to RAM inputs.
- Exposes final outputs: **data out**, **valid**, and **ready**.

---

### 4. 🧪 Testbench Logic

- Simulates realistic scenarios:
  - **Write** data to specific address.
  - **Read** from the same address and check if data matches.
- Helps verify:
  - Address decoding correctness.
  - Read/Write timing behavior.
  - Handshake signals (`ready`, `valid`).
- All actions happen in sync with clock cycles and reset behavior.

---

## 🚦 Control Flow (How It Works)

### 🔁 Write Operation
1. External system asserts **req = 1** and **rw = 0**
2. Controller decodes address → row + column
3. RAM writes **Qi** to specified memory cell
4. `valid` stays low; write is assumed complete in 1 cycle
5. Controller asserts `ready = 1` for next operation

### 🔄 Read Operation
1. External system asserts **req = 1** and **rw = 1**
2. Address is decoded → access cell
3. RAM outputs data at `Qa` and sets **valid = 1**
4. Controller sets `ready = 1` after confirming read
5. Data is available on the next cycle

---

## 💻 FPGA Implementation (Theory)

- Deployed on **Nexys 4 DDR FPGA board**
- Used physical switches for:
  - Control signals: `rw`, `req`, `cs`
  - Data input: `Qi`
  - Address input: `addr`
- Observed outputs:
  - LEDs display data from memory (`Qa`)
  - Indicator LED for `valid`
- Implementation steps:
  1. Assign FPGA pins in `.xdc` file
  2. Synthesize and implement design
  3. Program FPGA and test in real-time

---

## 📊 Reports Summary

| Report                | Highlights |
|------------------------|------------|
| **Timing Analysis**    | Achieved 100 MHz frequency with no violations. |
| **Power Consumption**  | Very low dynamic power due to small logic footprint. |
| **Resource Utilization** | Only a fraction of LUTs and FFs used — efficient and minimal. |

---

## 📚 Learnings & Insights

- 📌 **Address Decoding**: Learned how to map a flat address to 2D memory (row × column).
- 🧠 **Handshake Signals**: Realized the importance of `valid` and `ready` for preventing data corruption.
- ⚙️ **Modular Design**: Experienced the benefits of separating controller and datapath.
- 🧪 **Verification Flow**: Understood testbench structure and clock-driven simulation.
- 📉 **FPGA Constraints**: Learned how to map HDL signals to physical board pins.

---

## 🚀 Future Enhancements

| Improvement              | Benefit |
|--------------------------|---------|
| **Dual-Port RAM**        | Support simultaneous read and write |
| **Burst Read/Write Mode**| Higher throughput, pipelined memory access |
| **AXI/AHB Interface**    | Industry-standard protocol compatibility |
| **UVM Testbench**        | Scalable, reusable, professional-grade verification |
| **Built-In Self Test**   | Add DFT features to improve testability |
| **Python/TCL Automation**| Auto-run simulations and waveform capture |

---

## 📞 Contact

**Vinayak V P**  
🎓 Final-Year ECE Student  
🏫 Vidyavardhaka College of Engineering, Mysuru  
📧 vision.vinayak12@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/vinayakvision) | [GitHub](https://github.com/vinayakvision)

---

> ⭐ *If you liked this work or learned something from it, don’t forget to give this project a star on GitHub!*
