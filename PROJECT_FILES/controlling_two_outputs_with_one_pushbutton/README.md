# LOGO! PLC Project: Dual-Function Pushbutton Control

**Control two outputs (Q1 & Q2) with one momentary pushbutton using press duration logic.**
*(Siemens LOGO!Soft Comfort V8.4 - FBD Programming)*

---

## 📌 Overview
This project toggles **Q1** on a **short press** (<1s) and **Q2** on a **long press** (≥1s) of Input 1 (momentary PB). Ideal for multi-function controls.

---

## 🧩 Required Blocks & Their Roles

| Block Type          | Symbol | Purpose                                                |
|---------------------|--------|--------------------------------------------------------|
| **Digital Input**   | `I1`   | Momentary pushbutton input (NO contact)                |
| **Edge Detection**  | `B005` | Detects rising edge of I1 to trigger short-press logic |
| **AND Gate**        | `B004` | Combines edge detection + PB state for clean signal    |
| **Pulse Relay**     | `RS`   | Toggles Q1 on short presses (reset on release)         |
| **On-Delay Timer**  | `TON`  | Measures press duration; toggles Q2 if ≥1s             |
| **Output Coils**    | `Q1/Q2`| Physical/logical outputs controlled by the logic       |

---

## 🔌 Connection Guide

### 1. **Short-Press Path (Q1 Toggle)**
I1 → Edge Detector (B005) → AND (B004) → RS Flip-Flop → Q1
- *Why?* The edge detector ensures only the initial press (not hold) triggers Q1.

### 2. **Long-Press Path (Q2 Toggle)**
I1 → On-Delay Timer (TON, 1s) → Q2
- *Why?* The timer ignores short presses but activates after 1 second.

### 3. **Interlocking Logic**
- The **AND gate** prevents Q1 toggle if the timer is active (long press).
- The **Pulse Relay** resets on button release (`Rem = off`).

---

## ⚙️ Gate/Block Explainer

### 🔹 **Edge Detector (B005 - NAND)**
- **Function**: Outputs a pulse only when I1 changes from 0→1 (rising edge).
- **Why?** Ensures Q1 toggles exactly once per press, not during hold.

### 🔹 **AND Gate (B004)**
- **Inputs**: Edge pulse (B005) + I1 state.
- **Why?** Combines edge and PB state to filter noise/debounce.

### 🔹 **Pulse Relay**
- **Set (S)**: AND gate output.
- **Reset (R)**: Button release (inverse of I1).
- **Why?** Maintains Q1 state until next short press.

### 🔹 **On-Delay Timer (TON)**
- **Preset**: 1000 ms (1s).
- **Why?** Delays Q2 activation until long-press is confirmed.

---

## 🖥️ Screenshot of FBD
![FBD Logic Diagram](FBD_screenshot.png)

*LOGO! FBD program for dual-output pushbutton control.*