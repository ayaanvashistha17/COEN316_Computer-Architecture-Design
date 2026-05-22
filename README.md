Here is the complete text formatted perfectly in **Markdown**. You can copy everything inside the block below and paste it directly into your GitHub `README.md` file. It will look incredibly professional, clean, and highly readable for anyone visiting your profile!

```markdown
# MIPS 32-Bit Microprocessor Design & Architecture

A complete single-cycle MIPS 32-bit microprocessor datapath and control unit, designed and synthesized in VHDL using Xilinx Vivado and ModelSim.

---

## 📋 Course Overview

[cite_start]This repository contains a comprehensive, multi-stage hardware engineering project completed for **COEN 316 (Computer Architecture)** at **Concordia University**[cite: 4, 166, 264, 447]. 

[cite_start]The goal of this sequence of laboratories is to design, model, simulate, and physically implement a fully functional, custom subset of a **32-bit MIPS CPU**[cite: 9, 10, 450]. [cite_start]Each lab builds hierarchically upon the previous modules, culminating in a complete hardware execution datapath capable of running assembly language programs directly on physical hardware via a **Digilent Nexys A7 FPGA board**[cite: 94, 261, 514, 681].

### Key Architecture Details
* [cite_start]**Word Width:** 32-bit internal data channels, registers, and memory words[cite: 9, 190, 395].
* [cite_start]**Addressing Model:** Word-addressable main memory layout, which simplifies standard byte-addressing down to direct 1-word increments for architectural clarity[cite: 395, 396].
* [cite_start]**Instruction Formats:** Complete hardware decoding for **R-Type** (Register), **I-Type** (Immediate), and **J-Type** (Jump) instructions[cite: 450, 451].

---

## 🗺️ Architectural Roadmap

```text
                  +---------------------------------------+
                  |            Next-Address Unit          | <-- [PC_sel, branch_type]
                  +---------------------------------------+
                                      | (next_pc)
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
         | (rs)                       | (rt)                       | (rd / immediate)
         v                            v                            v
  +---------------+            +---------------+            +--------------+
  |  Register A   |            |  Register B   |            | Sign-Extend  |
  |  (Read Port)  |            |  (Read Port)  |            +--------------+
  +---------------+            +---------------+                   |
         |                            |                            |
         |                            +-------------\              | (extended)
         |                                           |             |
         v                                           v             v
  +---------------+                           +-----------------------+
  |    Operand X  |                           |     MUX (alu_src)     |
  +---------------+                           +-----------------------+
         |                                                 |
         |                                                 v
         |                                          +--------------+
         |                                          |   Operand Y  |
         |                                          +--------------+
         |                                                 |
         +--------------------> +-------+ <----------------+
                                |  ALU  | <-- [func, logic_func, add_sub]
                                +-------+
                                 |     |
                        (Result) |     +---> [Zero, Overflow Flags]
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
                                 +---> (Write-back Loop to Register File)

```

---

## 🛠️ Deep-Dive Portfolio: Laboratory Breakthroughs

### 🎛️ Lab 1: Arithmetic and Logic Unit (ALU)

* 
**Core Objective:** Design and simulate a high-performance 32-bit processing core capable of processing mathematical and Boolean logic equations.


* **Control Schemes:**
* 
`func (2 bits)`: High-level operation selection (`00` = Load Upper Immediate, `01` = Set Less Than Zero, `10` = Arithmetic, `11` = Logic).


* 
`logic_func (2 bits)`: Bitwise logic configuration (`00` = AND, `01` = OR, `10` = XOR, `11` = NOR).


* 
`add_sub`: High-speed add/subtract polarity bit (`0` = Addition, `1` = Subtraction).




* **Mathematical Boundary Tracking:**
* 
**Zero Detection (`zero` flag):** Asserts high (`1`) instantly if the underlying adder result collapses completely to zero.


* 
**Two's Complement Overflow (`overflow` flag):** Monitors mathematical sign inversions (e.g., adding two positive integers resulting in a negative number, or subtracting across opposite polarities). Evaluated using precise boolean K-Map optimization or sign-bit carry XOR logic: $C_{\text{in}} \oplus C_{\text{out}}$.




* 
**Physical Prototyping Wrapper:** Employs a specific physical VHDL board wrapper mapping lower-order 4-bit data interfaces straight to on-board sliding switches and micro-LED lines for real-time verification.



---

### 🗄️ Lab 2: Multi-Port Register File

* 
**Core Objective:** Establish a high-speed $32 \times 32$-bit internal storage matrix representing CPU structural registers $R_0$ through $R_{31}$.


* **Access Mechanics:**
* 
**Dual Asynchronous Read Ports (`out_a`, `out_b`):** Purely combinational lookups retrieving register content instantly based on independent 5-bit array coordinates (`read_a`, `read_b`).


* 
**Single Synchronous Write Port (`din`):** Regulated via an active-high `write` enable latch linked tightly to positive clock edges.




* 
**Hardware Boundary Constraints:** Registers are initialized simultaneously via an active-high asynchronous master `reset`. Crucially, register **$R_0$ is hardwired to a constant `0x00000000**` in alignment with industry-standard MIPS design specifications, ignoring writing cycles directed to its index.



---

### 🗺️ Lab 3: Next-Address Program Sequencer

* 
**Core Objective:** Construct an intelligent execution router responsible for monitoring instruction locations and formulating upcoming execution paths.


* **Routing Strategy:** Handles automatic sequencing based on control registers:
* 
**Straight-Line Incrementing:** Normal execution progression incrementing the current pointer ($PC = PC + 1$).


* 
**Absolute Pseudo-Direct Jumps:** Instant redirect via a 26-bit immediate address padded with higher-order zeroes ($PC = [000000] \mathbin{\Vert} \text{target\_address}$).


* 
**Dynamic Register Branching (`jr`):** Jumps dynamically to memory locations directly held inside raw source register files ($PC = R_s$).


* 
**Relative Offsets:** Conditional branching evaluations (`beq`, `bne`, `bltz`) executing math operations ($PC = PC + 1 + \text{SignExtendedOffset}$) on matching register comparative assertions.





---

### 🏗️ Lab 4: Integrated CPU Datapath

* 
**Core Objective:** Structural aggregation combining the ALU, Register File, and Next-Address Unit alongside functional memory caches into a cohesive computational unit.


* **Core Features:**
* **Instruction Cache (I-Cache):** A read-only memory (ROM) module preloaded with native 32-bit binary instruction words.


* **Data Cache (D-Cache):** A high-speed synchronous RAM block acting as the core scratchpad for variable state allocation (`lw` and `sw` operations).


* **Multi-Format Sign-Extension Core:** An automated sign management pipeline converting 16-bit instructions into a complete 32-bit data stream, adapting dynamically to upper padding (`lui`), sign bit replication (`slti`), and logical zero padding.





---

## 🕹️ Supported CPU Instruction Set Architecture (ISA)

The complete pipeline interprets and processes 20 core instructions natively across three structural layouts:

Instruction Layout,Assembly Format,Operational Description,Architectural Class
R-Type   PDF,"add rd, rs, rt   PDF",Rd​←Rs​+Rt​   PDF,Arithmetic Addition   PDF
R-Type   PDF,"sub rd, rs, rt   PDF",Rd​←Rs​−Rt​   PDF,Arithmetic Subtraction   PDF
R-Type   PDF,"slt rd, rs, rt   PDF",Rd​←1 if Rs​<Rt​ else 0   PDF,Set Less Than   PDF
R-Type   PDF,"and rd, rs, rt   PDF",Rd​←Rs​⋅Rt​   PDF,Bitwise Logical AND   PDF
R-Type   PDF,"or rd, rs, rt   PDF",Rd​←Rs​+Rt​   PDF,Bitwise Logical OR   PDF
R-Type   PDF,"xor rd, rs, rt   PDF",Rd​←Rs​⊕Rt​   PDF,Bitwise Logical XOR   PDF
R-Type   PDF,"nor rd, rs, rt   PDF",Rd​←Rs​+Rt​​   PDF,Bitwise Logical NOR   PDF
I-Type   PDF,"lui rt, immediate   PDF",Rt​←immediate∥0x0000   PDF,Load Upper Immediate   PDF
I-Type   PDF,"slti rt, rs, immediate   PDF",Rt​←1 if Rs​<immediate else 0   PDF,Set Less Than Immediate   PDF
I-Type   PDF,"addi rt, rs, immediate   PDF",Rt​←Rs​+immediate   PDF,Add Immediate   PDF
I-Type   PDF,"andi rt, rs, immediate   PDF",Rt​←Rs​⋅immediate   PDF,AND Immediate   PDF
I-Type   PDF,"ori rt, rs, immediate   PDF",Rt​←Rs​+immediate   PDF,OR Immediate   PDF
I-Type   PDF,"xori rt, rs, immediate   PDF",Rt​←Rs​⊕immediate   PDF,XOR Immediate   PDF
I-Type   PDF,"lw rt, immediate(rs)   PDF",Rt​←Memory[Rs​+immediate]   PDF,Load Word from RAM   PDF
I-Type   PDF,"sw rt, immediate(rs)   PDF",Memory[Rs​+immediate]←Rt​   PDF,Store Word to RAM   PDF
I-Type   PDF,"beq rs, rt, offset   PDF","If Rs​=Rt​,PC←PC+1+offset   PDF+ 1",Branch on Equal   PDF
I-Type   PDF,"bne rs, rt, offset   PDF","If Rs​=Rt​,PC←PC+1+offset   PDF+ 1",Branch on Not Equal   PDF
I-Type   PDF,"bltz rs, offset   PDF","If Rs​<0,PC←PC+1+offset   PDF+ 1",Branch on Less Than Zero   PDF
J-Type   PDF,j target   PDF,PC←[000000]∥target   PDF+ 1,Unconditional Jump   PDF
R-Type   PDF,jr rs   PDF,PC←Rs​   PDF+ 1,Jump Register   PDF

---

## 💻 Technical Toolchain & Deployment Guide

### Software Ecosystem

* 
**HDL Modeling:** Synthesizable VHDL (IEEE 1164 standards).


* 
**Simulation Engine:** ModelSim (TCL execution via custom `.do` batch macro scripts).


* 
**Synthesis & Implementation Suite:** Xilinx Vivado Design Suite.



### Physical Board Hardware Deployment

To test these designs natively on a **Digilent Nexys A7 FPGA Development Board**:

1. Open Xilinx Vivado and initialize a new project targeting your specific FPGA board part number.


2. Import the structural VHDL files along with the provided board-level wrappers (`*_board.vhd`).


3. Link the target hardware physical map using the constraints configuration file (`.xdc`) to cleanly assign I/O signals to the physical switches, clock modules, and LEDs.


4. Execute **Synthesis** and run **Implementation**. Review the output reports in the `synth_1` and `impl_1` subdirectories via the `runme.log` files to ensure zero critical synthesis warnings or unexpected hardware latch formations exist.


5. Generate the physical `.bit` stream file, connect your Nexys A7 development board, and flash the hardware image using the Vivado Hardware Manager.



---

## 📘 Academic Attribution & References

* 
**Textbook Reference:** *Computer Architecture: From Microprocessors to Supercomputers*, Dr. Behrooz Parhami, Oxford University Press (ISBN: `0-19-515455-X`).


* 
**Course Assignment Lab Framework:** Originally authored and structural criteria established by **Professor Ted Obuchowicz** (Department of Electrical and Computer Engineering, Concordia University).



```

```
