# QuAC CPU Design — COMP2300 Assignment 1 🚀

A single‑cycle **QuAC** (Quad‑Arithmetic‑Condition) CPU implemented in [*Digital*](https://github.com/hneemann/Digital) and extended beyond the base ISA with multiplication/division, flags, and robust error handling.

> **Course** : Australian National University – COMP2300  
> **Year**    : 2024 
> **Author**  : Jugraj Singh (@Jugraj1)

---

## 1. Project Highlights

| Module | Description |
|--------|-------------|
| `src/basic-cpu.dig` | Vanilla QuAC datapath & control for the mandatory spec |
| `src/extended-cpu.dig` | Adds MUL, DIV, overflow/undef flags and ALUOPEX control |
| `quac.json` | Assembler spec for the extended ISA (Mnemonics, encodings) |
| `demo.quac` | Demo program stressing every instruction incl. edge cases |
| `test/` | Automated “golden‑trace” checks run by GitHub Actions CI |
| `report.md` | Design rationale, trade‑offs and timing analysis |

### Key Extensions
* **ALUOPEX (2‑bit)** — distinguishes normal ALU ops, MUL, DIV without expanding the main `ALUOP` bus  
* **UNDEF flag** — clean detection of divide‑by‑zero  
* **Parallel multiplier** — three‑layer carry‑save adder tree keeps the critical path short  
* **Modular flag muxing** — legacy flags and new flags funnelled through a single multiplexer

---

## 2. Quick Start

### Prerequisites
* Java 11+ (for Digital)  
* `make` (optional, convenience targets)

### Local Simulation
```bash
# 1.  Clone
git clone https://github.com/Jugraj1/quac-cpu-design.git
cd quac-cpu-design

# 2.  Launch Digital
./tools/run-digital.sh          # or open Digital.jar manually

# 3.  Load the circuit
#    a) basic‑cpu.dig  – baseline
#    b) extended‑cpu.dig – with MUL/DIV etc.
