# 8-bit CPU Design in Logisim

An **8-bit single-cycle CPU** designed and implemented in Logisim, progressing from fundamental digital components to a complete processor with memory, register file, control logic, branching, and jump instructions.

The project demonstrates the design and integration of core computer architecture components including a custom ALU, register file, instruction memory, data memory, program counter, and control unit.

## Overview

The CPU was developed incrementally in three stages:

1. **ALU and Digital Components**
2. **CPU Core with Memory and Register File**
3. **Extended CPU with Branch and Jump Instructions**

This approach demonstrates how individual digital components can be integrated into a functioning processor architecture.

## Project Structure

```text
8-bit-cpu-logisim/
│
├── part-1-alu/
│   ├── circuits/
│   │   ├── alu8.circ
│   │   ├── full_adder_8bit.circ
│   │   ├── comparator_8bit.circ
│   │   ├── rotate_left.circ
│   │   ├── rotate_right.circ
│   │   └── twos_complement.circ
│   │
│   └── tests/
│       ├── alu8_test.txt
│       ├── full_adder_8bit_test.txt
│       ├── comparator_8bit_test.txt
│       ├── rotate_left_test.txt
│       ├── rotate_right_test.txt
│       └── twos_complement_test.txt
│
├── part-2-cpu-core/
│   └── circuits/
│       └── 8bit_cpu_part2.circ
│
├── part-3-extended-cpu/
│   └── circuits/
│       └── 8bit_cpu_final.circ
│
├── README.md
└── .gitignore
```

# Part 1 – ALU and Digital Components

The first stage focuses on implementing the fundamental components required by the processor.

## Two's Complement Converter

An 8-bit two's complement circuit is implemented by:

1. Inverting all input bits
2. Adding `1` to the inverted result

Conceptually:

```text
Two's Complement = NOT(X) + 1
```

The circuit produces an 8-bit two's complement representation of the input.

## Rotate Left

An 8-bit rotate-left circuit performs circular bit rotation.

Instead of discarding the most significant bits, they are wrapped around to the least significant positions.

The rotation amount is controlled using a 3-bit input, allowing rotation values from:

```text
0 – 7 bits
```

## Rotate Right

The rotate-right circuit performs circular rotation in the opposite direction.

The least significant bits are wrapped around to the most significant positions instead of being discarded.

## 8-bit Full Adder

An 8-bit ripple-carry full adder is constructed using eight 1-bit full adders.

Each stage receives:

```text
X
Y
Carry In
```

and produces:

```text
Sum
Carry Out
```

The carry output of each bit is connected to the carry input of the next bit.

## 8-bit Comparator

The comparator checks whether two 8-bit inputs are equal.

Each corresponding bit is compared using XNOR logic.

The eight comparison results are combined using an AND operation:

```text
X == Y → EQUAL = 1
X != Y → EQUAL = 0
```

## 8-bit ALU

The components are integrated into an 8-bit Arithmetic Logic Unit.

The ALU accepts:

```text
X[8]
Y[8]
opcode[2]
```

and produces:

```text
RESULT[8]
```

It also generates processor status flags:

```text
EQ  - Equal Flag
OVF - Overflow Flag
CF  - Carry Flag
ZF  - Zero Flag
SF  - Sign Flag
```

Test vectors are included for the ALU and its individual sub-circuits.

# Part 2 – CPU Core

The second stage integrates the ALU into a simple **8-bit CPU**.

The processor contains:

- 8-bit ALU
- 8-register register file
- Program Counter
- Instruction Memory
- Data Memory
- Control Unit
- Multiplexers
- Clock
- Control signals

## Register File

The CPU contains eight 8-bit registers:

```text
R0
R1
R2
R3
R4
R5
R6
R7
```

Two multiplexers select source registers while a decoder selects the destination register.

Register writes are controlled using the **Write Enable (WE)** signal.

## Instruction Memory

Instructions are stored in ROM.

The CPU uses **21-bit instructions** and supports R-type and I-type instruction formats.

### R-Type

```text
Opcode | Rs | Rt | Rd | unused
```

### I-Type

```text
Opcode | Rs | Rt | Rd | Immediate
```

## Data Memory

RAM is used as data memory.

The memory system supports load and store operations using control signals for:

```text
Read
Write
Address
Data
```

## Initial Instruction Set

The processor supports instructions including:

```text
ADD
AND
OR
ROTL
LOAD
STORE
LOADI
```

Example:

```text
ADD Rd, Rs, Rt
```

performs:

```text
Rd = Rt + Rs
```

# Part 3 – Extended CPU

The final stage extends the processor with additional instructions and control-flow functionality.

New instructions include:

```text
J
JAL
JR
BEQ
BNE
SLT
```

## Program Counter Enhancements

The Program Counter was updated to support:

- Sequential execution
- Conditional branches
- Unconditional jumps
- Jump-and-link
- Jump-register

Additional multiplexing and control logic determine the next PC value.

## Extended Control Unit

The Control Unit generates signals used by the ALU, memory, register file, multiplexers, and PC logic.

Additional control signals include:

```text
PCmux
Branch
Jal
```

The control logic was derived using Boolean logic and Karnaugh-map simplification.

## Instruction Execution

The final CPU was tested using a program containing **15 instructions** covering arithmetic, logical, memory, branch, and jump operations.

Example instructions include:

```assembly
li r3, 7
li r5, 2
jal 9
slt r1, r3, r4
add r6, r3, r4
ld r2, 1(r0)
bne r6, r2, 6
rotl r4, r5(r3)
st r4, 1(r0)
jr $ra
and r6, r3, r2
beq r6, r7, 2
j 12
```

The design was validated by inspecting:

- Register contents
- RAM contents
- Instruction execution
- Branch behavior
- Jump behavior
- Control signals

# Testing

Test-vector files are included for the Part 1 components.

Examples include:

```text
alu8_test.txt
full_adder_8bit_test.txt
comparator_8bit_test.txt
rotate_left_test.txt
rotate_right_test.txt
twos_complement_test.txt
```

These test vectors verify circuit behavior for multiple binary input combinations.

For example, the ALU tests verify:

```text
RESULT
EQ
OVF
CF
ZF
SF
```

for different inputs and operation codes.

# Concepts Demonstrated

- Computer Architecture
- Digital Logic Design
- CPU Datapath Design
- Arithmetic Logic Units
- Register Files
- Program Counters
- Instruction Encoding
- ROM and RAM
- Control Units
- Multiplexers
- Decoders
- Boolean Logic
- Karnaugh Maps
- Two's Complement
- Ripple-Carry Addition
- Bit Rotation
- Status Flags
- Branch Instructions
- Jump Instructions
- Single-Cycle Processor Design

# Tools

- Logisim / Logisim Evolution
- Digital Logic
- Computer Architecture

# Running the Project

1. Install **Logisim** or **Logisim Evolution**.
2. Clone or download this repository.
3. Open the desired `.circ` file.

For the complete processor, open:

```text
part-3-extended-cpu/circuits/8bit_cpu_final.circ
```

For individual ALU components, open the circuits under:

```text
part-1-alu/circuits/
```

Test vectors can be loaded from:

```text
part-1-alu/tests/
```

# Academic Context

This project was developed as part of:

**CNG331 / EEE445 – Computer Organisation and Architecture I**

at **METU Northern Cyprus Campus**.

The project was developed incrementally from basic combinational circuits to an extended 8-bit processor architecture.

## Contributors

- Furkan Sağlam
- Fatih Sağlam
- Eda İslam

## Keywords

`Logisim` `CPU` `8-bit CPU` `Computer Architecture` `ALU` `Digital Logic` `Register File` `Control Unit` `RAM` `ROM` `Branching` `Single-Cycle CPU`
