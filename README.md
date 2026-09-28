# IAS_Proc — IAS Processor Design

A C++ simulator of the classic **IAS (Institute for Advanced Study) machine** architecture, with a companion assembler, built as a Computer Architecture course project (EG 212, Processor Design).

Authors: M S Dheeraj Murthy, Mathew Joseph, Priyanshu Pattnaik.

## What's implemented

- **Assembler** (`imt2023008_assembler.cpp`) — converts a custom IAS-style assembly language into binary machine code via string parsing. Each input line is 20 characters wide and packs two instructions (7 characters for the mnemonic, 3 for the operand).
- **Processor** (`imt2023008_processor.cpp`) — simulates the IAS fetch/execute cycle: `PC`, `AC` (accumulator), `MQ` (multiplier-quotient), `MBR`/`IBR` (memory/instruction buffer registers), `IR`/`MAR` (instruction/address registers), and main memory as a vector of bit vectors. It `#include`s the assembler directly, so the two run as a single pipeline: assembly text in, execution out.

### Instruction set (15 opcodes)

| Opcode | Mnemonic | Opcode | Mnemonic |
|---|---|---|---|
| `00000000` | NOP | `00000001` | LOAD ### |
| `00000010` | LOAD MQ### | `00000011` | STOR ### |
| `00000100` | ADD ### | `00000101` | SUB ### |
| `00000110` | MUL ### | `00000111` | DIV ### |
| `00001000` | JUMP ### | `00001001` | CJUMP ### |
| `00001010` | EQUA ### | `00001011` | LESS ### |
| `00001100` | MORE ### | `00001101` | DISP |
| `00001110` | END | | |

`CJUMP` jumps only if `AC` is positive; `EQUA`/`LESS`/`MORE` compare `AC` against a memory operand and set `AC` to `1`/`0`; `DISP` prints `AC`; `END` halts execution.

### Demo program

A primality-check program (does `n` have a nontrivial divisor?) is assembled and run on the simulator as the worked example in `ProjectReport.pdf`.

## Repo contents

```
IAS_Processor.zip     # source: imt2023008_assembler.cpp, imt2023008_processor.cpp,
                       # imt2023008_Assembly.txt (sample program)
ProjectReport.pdf      # write-up: ISA reference, processor design, instruction semantics, usage
```

## Building & running

```bash
unzip IAS_Processor.zip
g++ -std=c++17 -O2 imt2023008_processor.cpp -o ias_proc   # pulls in the assembler via #include
./ias_proc < imt2023008_Assembly.txt
```

Input is assembly source fed on stdin (blank line terminates input); the program prints results (e.g. `DISP` output) to stdout.
