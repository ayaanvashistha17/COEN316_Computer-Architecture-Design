# MIPS 32-Bit Microprocessor Design & Architecture

A complete single-cycle MIPS 32-bit microprocessor datapath and control unit, designed and synthesized in VHDL using Xilinx Vivado and ModelSim.

---

# 📋 Course Overview

This repository contains a comprehensive hardware engineering project completed for **COEN 316 (Computer Architecture)** at **Concordia University**.

The goal of this project was to design, model, simulate, and implement a functional subset of a **32-bit MIPS CPU** using VHDL. Each laboratory builds on the previous one, eventually creating a complete processor datapath capable of executing assembly programs on a **Digilent Nexys A7 FPGA board**.

## Key Architecture Details

- **Word Width:** 32-bit internal registers, memory, and datapath
- **Architecture Type:** Single-cycle MIPS processor
- **Instruction Formats Supported:**
  - R-Type
  - I-Type
  - J-Type
- **Hardware Platform:** Digilent Nexys A7 FPGA
- **HDL Language:** VHDL
- **Simulation Tools:** ModelSim
- **Synthesis Tool:** Xilinx Vivado

---

# 🗺️ CPU Datapath Overview

```text
                  +---------------------------------------+
                  |            Next-Address Unit          |
                  +---------------------------------------+
                                      |
                                      v
                             +-----------------+
                             | Program Counter |
                             +-----------------+
                                      |
                                      v
                             +-----------------+
                             |Instruction Cache|
                             +-----------------+
                                      |
         +----------------------------+----------------------------+
         |                            |                            |
         v                            v                            v
  +---------------+            +---------------+            +--------------+
  |  Register A   |            |  Register B   |            | Sign-Extend  |
  +---------------+            +---------------+            +--------------+
         |                            |                            |
         |                            +-------------\              |
         |                                           |             |
         v                                           v             v
  +---------------+                           +-----------------------+
  |   Operand X   |                           |     MUX (alu_src)     |
  +---------------+                           +-----------------------+
         |                                                 |
         |                                                 v
         |                                          +--------------+
         |                                          |   Operand Y  |
         |                                          +--------------+
         |                                                 |
         +--------------------> +-------+ <----------------+
                                |  ALU  |
                                +-------+
                                 |     |
                        (Result) |     +---> [Zero / Overflow Flags]
                                 v
                        +----------------+
                        |   Data Cache   |
                        +----------------+
                                 |
                                 v
                        +----------------+
                        | MUX (reg_src)  |
                        +----------------+
                                 |
                                 +---> Register Write Back
```

---

# 🛠️ Laboratory Breakdown

## 🎛️ Lab 1 — Arithmetic Logic Unit (ALU)

### Objective
Design and simulate a 32-bit ALU capable of performing arithmetic and logical operations.

### Features
- Addition and subtraction
- Logical AND, OR, XOR, NOR
- Set-less-than operations
- Zero flag generation
- Overflow detection using two’s complement arithmetic

### Control Signals
| Signal | Description |
|---|---|
| `func` | Main ALU operation selector |
| `logic_func` | Logic operation selector |
| `add_sub` | Addition/Subtraction control |

### Flags
- `zero` → Activated when output equals zero
- `overflow` → Detects signed arithmetic overflow

---

## 🗄️ Lab 2 — Register File

### Objective
Implement a 32 × 32-bit register file.

### Features
- Two asynchronous read ports
- One synchronous write port
- Active-high reset
- Register `R0` permanently fixed to `0x00000000`

### Supported Operations
- Simultaneous dual-register reading
- Clocked write operations
- Register reset and initialization

---

## 🗺️ Lab 3 — Next Address Unit

### Objective
Create the CPU control flow unit responsible for program execution sequencing.

### Supported Behaviors
- Sequential execution (`PC = PC + 1`)
- Jump instructions
- Branch instructions
- Register-based jumps (`jr`)
- Relative branching with offsets

### Supported Branch Types
- `beq`
- `bne`
- `bltz`
- `j`
- `jr`

---

## 🏗️ Lab 4 — Integrated CPU Datapath

### Objective
Combine all previous modules into a complete processor datapath.

### Integrated Components
- ALU
- Register File
- Next Address Unit
- Instruction Cache
- Data Cache
- Sign Extension Unit

### Memory Components
| Component | Purpose |
|---|---|
| Instruction Cache | Stores machine instructions |
| Data Cache | Stores runtime data |

### Additional Features
- Sign extension for immediate values
- Support for multiple instruction formats
- Full datapath write-back loop

---

# 🕹️ Supported Instruction Set (ISA)

| Type | Instruction | Description |
|---|---|---|
| R-Type | `add rd, rs, rt` | Addition |
| R-Type | `sub rd, rs, rt` | Subtraction |
| R-Type | `slt rd, rs, rt` | Set less than |
| R-Type | `and rd, rs, rt` | Bitwise AND |
| R-Type | `or rd, rs, rt` | Bitwise OR |
| R-Type | `xor rd, rs, rt` | Bitwise XOR |
| R-Type | `nor rd, rs, rt` | Bitwise NOR |
| I-Type | `addi rt, rs, imm` | Add immediate |
| I-Type | `andi rt, rs, imm` | AND immediate |
| I-Type | `ori rt, rs, imm` | OR immediate |
| I-Type | `xori rt, rs, imm` | XOR immediate |
| I-Type | `slti rt, rs, imm` | Set less than immediate |
| I-Type | `lui rt, imm` | Load upper immediate |
| I-Type | `lw rt, imm(rs)` | Load word |
| I-Type | `sw rt, imm(rs)` | Store word |
| I-Type | `beq rs, rt, offset` | Branch if equal |
| I-Type | `bne rs, rt, offset` | Branch if not equal |
| I-Type | `bltz rs, offset` | Branch if less than zero |
| J-Type | `j target` | Jump |
| R-Type | `jr rs` | Jump register |

---

# 💻 Toolchain & Deployment

## Software Tools

- **VHDL** for hardware description
- **ModelSim** for simulation
- **Xilinx Vivado** for synthesis and FPGA implementation

---

# 🚀 FPGA Deployment Steps

1. Open Xilinx Vivado
2. Create a new FPGA project
3. Import all VHDL source files
4. Add the `.xdc` constraints file
5. Run:
   - Synthesis
   - Implementation
6. Generate the `.bit` bitstream file
7. Connect the Nexys A7 FPGA board
8. Program the board using Vivado Hardware Manager

---

# 📘 References

## Textbook
**Computer Architecture: From Microprocessors to Supercomputers**  
Dr. Behrooz Parhami  
Oxford University Press

## Course
**COEN 316 — Computer Architecture**  
Department of Electrical & Computer Engineering  
Concordia University

---
