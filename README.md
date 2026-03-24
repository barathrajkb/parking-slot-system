# 🚗 Hardware-Only Automated Car Parking Slot Management System

> A fully combinational, hardware-based 4-slot parking management system — **no microcontroller, no FPGA, no firmware**. Pure 74XX-series logic gates.

---

## 📌 Overview

This project implements a real-time car parking slot management system entirely using discrete digital logic. Four IR sensors monitor individual parking slots and drive a multi-output system that:

- Indicates each slot's status via **RGB LEDs** (Free / Reserved / Occupied)
- Counts available slots using a **combinational encoder circuit**
- Displays the available slot count on a **7-segment display** via a BCD decoder

No microcontroller, no programmable logic device, no software — just gates, wires, and logic.

---

## 🧠 System Architecture

```
IR Sensors (×4)
      │
      ▼
┌─────────────────────┐        ┌───────────────────────────┐
│   LED Status Logic  │        │  Slot Counter (DCD)       │
│  (ParkingLED.png)   │        │  Combinational Encoder    │
│                     │        │  → 3-bit output (Y0,Y1,Y2)│
│  Per-slot:          │        │  (7SegmentCircuit.png)    │
│  RED  = Occupied    │        └────────────┬──────────────┘
│  GREEN = Free       │                     │
│  BLUE = Reserved    │                     ▼
└─────────────────────┘          ┌───────────────────────┐
                                 │  BCD → 7-Segment      │
                                 │  Decoder              │
                                 └───────────┬───────────┘
                                             │
                                             ▼
                                    [ 7-Segment Display ]
                                    Shows count of free slots
```

---

## 🔌 Circuit Modules

### 1. LED Status Indicator (`ParkingLED.png`)

Each of the 4 parking slots has its status encoded and displayed via RGB LEDs using combinational logic built from 74XX gates.

| Slot State  | LED Color |
|-------------|-----------|
| Free        | 🟢 Green  |
| Reserved    | 🔵 Blue   |
| Occupied    | 🔴 Red    |

- **Inputs:** 4 slot sensor signals + 4 reservation signals (total 8 inputs)
- **Outputs:** 4 RGB LED pairs driven by AND/OR gate logic
- **Logic:** Each LED color is driven by a minimal Boolean expression derived from the slot's occupancy and reservation state

### 2. Free Slot Counter — Combinational Decoder (`DCD.png`)

A combinational logic network that encodes the number of free slots (0–4) into a **3-bit binary output** (Y2, Y1, Y0).

- **Inputs:** 4 IR sensor signals (S0–S3), where `0 = Free`, `1 = Occupied`
- **Outputs:** 3-bit binary count of free slots → fed to BCD-to-7-segment decoder
- **Implementation:** Multi-level AND-OR combinational network (see `DCD.png`)

| Free Slots | Y2 | Y1 | Y0 |
|------------|----|----|----|
| 0          | 0  | 0  | 0  |
| 1          | 0  | 0  | 1  |
| 2          | 0  | 1  | 0  |
| 3          | 0  | 1  | 1  |
| 4          | 1  | 0  | 0  |

The 3-bit output feeds a standard **BCD-to-7-segment decoder IC** (e.g., 74LS47 / CD4511), which directly drives the 7-segment display.

---

## 🧩 Components Used

| Component | Part / Description |
|---|---|
| Logic Gates | 74LS00 (NAND), 74LS08 (AND), 74LS32 (OR), 74LS04 (NOT) |
| IR Sensors | IR obstacle detection modules (active-low output) |
| LEDs | 5mm RGB LEDs (common cathode) |
| 7-Segment Decoder | 74LS47 BCD-to-7-segment |
| 7-Segment Display | Common anode (matched to decoder) |
| Resistors | Current-limiting resistors for LEDs and display |
| Power Supply | 5V DC regulated |

---

## 📐 Circuit Diagrams

### LED Status Logic
![LED Status Circuit](ParkingLED.png)

*Combinational circuit controlling RGB LEDs for each of the 4 parking slots based on sensor and reservation inputs.*

### Free Slot Counter
![Counter Circuit](7SegmentCircuit.png)

*AND-OR combinational network encoding the number of free slots as a 3-bit value (Y0, Y1, Y2) for the 7-segment display.*

---

## ⚙️ How It Works

1. **Sensing:** IR sensors at each slot output a logic signal — `LOW` when a car is present, `HIGH` when free.
2. **LED Logic:** The LED status circuit processes each sensor output (along with a reservation flag) through gate logic to select the correct LED color per slot.
3. **Counting:** The DCD circuit takes all 4 sensor outputs and computes the number of free slots via a combinational sum-of-products expression, producing a 3-bit binary result.
4. **Display:** The 3-bit output connects to a BCD-to-7-segment decoder, which drives the 7-segment display to show the live count of available slots.

---

## 🎯 Key Design Highlights

- **Fully hardware combinational** — no clock, no sequential logic, no microcontroller
- **Real-time response** — propagation delay only; output updates instantly with input changes
- **Logic minimization** — Boolean expressions reduced using Karnaugh maps for minimal gate count
- **Modular design** — LED control and slot counting are independent, composable sub-circuits
- **Scalable approach** — design methodology extends to more slots by expanding the combinational encoder

---

## 📁 Repository Structure

```
.
├── README.md
├── circuits/
│   ├── ParkingLED.png       # LED status indicator circuit
│   └── DCD.png              # Free slot counter / combinational decoder
├── docs/
│   ├── truth_tables.md      # Full truth tables for both circuits
│   └── boolean_expressions.md  # Minimized SOP expressions (K-map derived)
└── simulation/              # CircuitVerse simulation files
```

---

## 🚀 Getting Started

### Building the Circuit

1. Refer to `ParkingLED.png` to wire the LED status logic for all 4 slots
2. Refer to `7SegmentCircuit.png` to build the free-slot counter combinational network
3. Connect the 3-bit output (Y0, Y1, Y2) to your BCD-to-7-segment decoder input
4. Connect decoder output to the 7-segment display (with appropriate current-limiting resistors)
5. Power the circuit at 5V DC; connect IR sensor outputs to the logic inputs

### Simulation (Recommended First Step)

Open the circuit in [CircuitVerse](https://circuitverse.org/) to verify logic before building on hardware.


---

## 👤 Author
**Barath Raj KB**  
B.E. Electronics & Communication Engineering

---

> *"The elegance of hardware — where logic is physics, and timing is everything."*
