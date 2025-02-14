## Overview

This project is a custom 16-bit processor designed and implemented in Logisim as part of an Advanced Computer Science final project. The processor is inspired by the MIPS architecture but features custom specifications and a unique instruction set. The core processes binary-encoded instructions and executes them sequentially, utilizing components such as a register file, instruction memory, data memory, and an ALU.

## Features
- **16-bit word-addressable architecture**
- **Custom instruction set** with R-type, I-type, and J-type instructions
- **8 general-purpose registers (r0 to r7)**
  - r0 is read-only and always contains 0
  - r6 is designated as the stack pointer
  - r7 is used as the return address
- **Instruction Memory (IMEM)** (16-bit address width, 16-bit data width)
- **Data Memory (DMEM)** (16-bit address width, 16-bit data width)
- **Program Counter (PC)** that increments by 1 per instruction
- **Arithmetic Logic Unit (ALU)** with basic operations
- **Control Unit** for instruction decoding and execution
- **Keyboard input (ASCII support)**
- **TTY output for ASCII characters**

## Project Structure
- `core.circ` - The main Logisim circuit file containing the processor
- `coretest1.txt`, `coretest2.txt` - Sample test programs for execution
- `explainer_document.pdf` - A document detailing the design decisions and architecture

## Instruction Set Architecture (ISA)
### Instruction Formats
1. **R-type**: `0000 000 000 000 000` (opcode, rs, rt, rd, function)
2. **I-type**: `0000 000 000 000000` (opcode, rs, rd, immediate)
3. **J-type**: `0000 000000000000` (opcode, address)

### Supported Instructions
| Type | Instruction | Opcode (Func) | Operation |
|------|------------|--------------|------------|
| R    | Add       | 0000 (000)   | rd = rs + rt |
| R    | Subtract  | 0000 (001)   | rd = rs - rt |
| R    | SLT       | 0000 (010)   | rd = 1 if rs < rt, else 0 |
| R    | SRA       | 0000 (011)   | rd = rs >> rt |
| R    | Multiply  | 0000 (100)   | rd = rs * rt |
| R    | AND       | 0000 (101)   | rd = rs & rt |
| R    | OR        | 0000 (110)   | rd = rs | rt |
| R    | NOT       | 0000 (111)   | rd = ~rs |
| I    | Addi      | 0001         | rd = rs + imm |
| I    | Subi      | 0010         | rd = rs - imm |
| I    | ANDi      | 0011         | rd = rs & imm |
| I    | ORi       | 0100         | rd = rs | imm |
| I    | Load Word | 0111         | rd = DMEM[rs + imm] |
| I    | Store Word| 1000         | DMEM[rs + imm] = rd |
| I    | Input     | 1001         | rd = keyboard input |
| I    | Output    | 1010         | TTY displays value in rs |
| I    | BEQ       | 1011         | If rs == rd, PC = {PC[15:6], imm} |
| I    | BEQZ      | 1100         | If rs == 0, PC = {PC[15:6], imm} |
| I    | Jump Reg  | 1101         | PC = rs |
| J    | Jump Link | 1110         | r7 = PC + 1, PC = {PC[15:12], addr} |
| J    | Jump      | 1111         | PC = {PC[15:12], addr} |

## Components
### 1. Register File (`regFile`)
- Stores 8 general-purpose registers (16-bit each)
- Register 0 is hardcoded to 0
- r6 serves as the stack pointer, and r7 is the return address

### 2. Program Counter (PC)
- Stores the address of the current instruction
- Increments by 1 each cycle unless modified by a branch or jump

### 3. Instruction Memory (IMEM)
- Read-only memory storing program instructions
- 16-bit address and data width

### 4. Data Memory (DMEM)
- Read/write memory for data storage
- Uses separate read and write databus

### 5. ALU (Arithmetic Logic Unit)
- Performs arithmetic and logical operations based on the instruction
- Supports add, subtract, multiply, bitwise operations, and comparisons

### 6. Control Unit
- Decodes instructions and generates control signals
- Directs data flow within the processor

### 7. Keyboard and TTY
- **Keyboard** allows ASCII character input
- **TTY** displays ASCII character output

## How to Use
### Running the Processor in Logisim
1. Open `core.circ` in Logisim.
2. Load a program into IMEM using `Load Image` (example test cases provided).
3. Set the `clk` (clock) to oscillate or manually pulse it.
4. Observe register values, memory updates, and TTY output.

### Writing Custom Programs
- Instructions should be written in hexadecimal and stored in `.txt` files.
- Example program:
  ```
  9080 1040 a400 2485 b288 045a c608 f002 0000
  ```
  
## Future Enhancements
- Implement pipelining for increased efficiency
- Add an assembler to convert assembly code to binary
- Expand the instruction set with more complex operations
- Optimize the ALU and control unit for better performance

## Acknowledgments
This project was developed for the **Advanced Computer Science 2023-24** course under the guidance of **Mr. Pratt**. Inspired by the MIPS architecture, it serves as an educational tool for learning computer architecture and processor design in Logisim.
