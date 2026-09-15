# README: Design and Simulation of a Digital Data Transmission Error Detection System

## Overview

This repository / project package contains the resources, documentation, and simulation details for the academic DDCA project: **"Design and Simulation of a Digital Data Transmission Error Detection System"**, implemented in Logisim-evolution.

The project demonstrates a transmitter-receiver communication model in which additional check bits are generated for an 8-bit data word and a receiver-side checking circuit identifies corrupted data by producing a non-zero syndrome and asserting an **ERROR** output.

---

## Slide Deck Overview (11 Slides)

1. **Slide 1: Title Slide**
   - Project title, student names, roll numbers, guide name, department, college, and academic year.
2. **Slide 2: Why Error Detection?**
   - Digital communication background, transmission errors, and the need to verify received data.
3. **Slide 3: Problem Statement**
   - Defines the problem of corrupted bits during transmission and the need for an automatic detection mechanism.
4. **Slide 4: Project Objectives**
   - Generation of check bits, receiver-side verification, controlled error injection, Logisim simulation, and logic validation.
5. **Slide 5: Literature Survey / Existing Methods**
   - Overview of parity, checksum, and CRC-style error-detection approaches with their strengths and limitations.
6. **Slide 6: Proposed System**
   - Proposed transmitter-channel-receiver model and key improvements over a simple parity-only approach.
7. **Slide 7: System Architecture / Block Diagram**
   - Complete flow: `8-bit Data -> Encoder -> Check Bits -> Transmission Channel -> Checker -> Syndrome -> Error Flag`.
8. **Slide 8: Methodology / Implementation**
   - Logic equations, XOR-based combinational implementation, receiver-side syndrome generation, and error decision logic.
9. **Slide 9: Logisim Simulation**
   - Circuit implementation, input pins, check-bit logic, received-codeword inputs, syndrome outputs, and ERROR indicator.
10. **Slide 10: Results and Discussion**
    - Verification cases for valid data, corrupted data, and controlled bit changes.
11. **Slide 11: Conclusion & GitHub Submission**
    - Project outcome, key observations, future scope, and required repository deliverables.

---

## Logisim Simulation Setup Instructions

To run and verify the circuit in **Logisim-evolution**:

1. **Prerequisites:**
   - Install Logisim-evolution 3.8.x or a compatible version.
   - Open the supplied `error_detection.circ` file.

2. **Transmitter / Encoder:**
   - Enter an 8-bit data word using the input pins `D7` through `D0`.
   - The encoder uses XOR-based combinational logic to generate three check bits: `C2`, `C1`, and `C0`.
   - The resulting transmitted word is formed as the 8 data bits plus the three check bits.

3. **Transmission Channel / Error Injection:**
   - The receiver section provides inputs `R7` through `R0` and `RC2` through `RC0`.
   - To demonstrate transmission noise, change one or more received bits relative to the transmitted word.

4. **Receiver / Checker:**
   - The receiver generates syndrome bits `S2`, `S1`, and `S0` using XOR networks.
   - A zero syndrome represents a valid received codeword.
   - A non-zero syndrome causes the `ERROR` output to assert.

5. **Observation:**
   - `ERROR = 0` → **VALID / No Error Detected**
   - `ERROR = 1` → **ERROR DETECTED**

---

## Working Principle

The project uses an **XOR-based CRC-style error detection architecture** for the DDCA demonstration.

For an 8-bit data word, three check bits are generated using the implemented XOR equations:

```text
C2 = D6 ⊕ D3 ⊕ D2 ⊕ D1
C1 = D7 ⊕ D5 ⊕ D2 ⊕ D1 ⊕ D0
C0 = D7 ⊕ D4 ⊕ D3 ⊕ D2 ⊕ D0
```

At the receiver, the received data and received check bits are processed to obtain syndrome bits:

```text
S2 = R6 ⊕ R3 ⊕ R2 ⊕ R1 ⊕ RC2
S1 = R7 ⊕ R5 ⊕ R2 ⊕ R1 ⊕ R0 ⊕ RC1
S0 = R7 ⊕ R4 ⊕ R3 ⊕ R2 ⊕ R0 ⊕ RC0
```

The final error indicator is formed from the syndrome outputs:

```text
ERROR = S2 ⊕ S1 ⊕ S0
```

Therefore:

```text
S2 S1 S0 = 000  ->  VALID
S2 S1 S0 ≠ 000  ->  ERROR DETECTED
```

> **Note:** The supplied Logisim circuit is an academic CRC-style / syndrome-based demonstration. The equations above describe the actual logic implemented in `error_detection.circ`.

---

## Demonstration / Verification

A convenient demonstration sequence is:

### Test 1 — Correct Transmission

1. Set a chosen 8-bit data word at `D7..D0`.
2. Observe the generated `C2..C0` check bits.
3. Apply the corresponding codeword to the receiver inputs `R7..R0` and `RC2..RC0`.
4. Verify that the receiver produces:

```text
S2 S1 S0 = 000
ERROR     = 0
```

**Result:** No error detected.

### Test 2 — Single-Bit Error

1. Start from a valid received codeword.
2. Flip one received bit, for example `R4`.
3. Observe the syndrome outputs.
4. Verify that the syndrome becomes non-zero and the `ERROR` output changes to `1`.

**Result:** Error detected.

---

## Truth Table / Verification Matrix

| **Test Case** | **Data Condition** | **Received Condition** | **Syndrome** | **ERROR Output** | **Status** |
|---|---|---|---|---:|---|
| 1 | Correct data | No bit changed | `000` | `0` | **No Error** |
| 2 | Correct data | One payload bit flipped | `≠000` | `1` | **Error Detected** |
| 3 | Correct data | One check bit flipped | `≠000` | `1` | **Error Detected** |
| 4 | Correct data | Multiple controlled changes | Depends on pattern | `0/1` | **Observed in Simulation** |

The final values should be recorded from the actual Logisim simulation screenshots before project submission.

---

## Circuit Components / Logic Used

- Digital input pins for `D7..D0`
- Digital receiver pins for `R7..R0`
- Check-bit inputs `RC2..RC0`
- XOR gates
- Combinational XOR networks
- Syndrome outputs `S2`, `S1`, `S0`
- `ERROR` output indicator
- Logisim-evolution simulation environment

---

## Project Repository Structure

```text
DDCA-Error-Detection-System/
├── README.md
├── error_detection.circ
├── project.pptx
└── screenshots/
    ├── circuit.png
    └── results.png
```

Replace `project.pptx` with the final PPT filename used in your GitHub repository.

---

## Advantages

- Simple XOR-based combinational implementation.
- Easy to visualize and demonstrate in Logisim.
- Provides a clear transmitter-channel-receiver model.
- Detects controlled bit corruption through syndrome checking.
- Suitable for explaining error-detection concepts in a DDCA mini-project.

## Limitations

- The project is intended as an educational simulation rather than a complete communication interface.
- The implemented detector indicates whether the received pattern is inconsistent with the expected check-bit relationship; it does not correct the corrupted data.
- Detection behaviour depends on the specific error pattern and implemented logic.

## Applications / Relevance

The concepts demonstrated are relevant to:

- Digital communication systems
- Computer networks
- Data buses and digital interfaces
- Memory and storage integrity checks
- Embedded and FPGA-based digital systems

------


| Test Case | Original Data | Transmitted | Received | ERROR | Status           |
| --------- | ------------- | ----------- | -------- | ----: | ---------------- |
| No Error  | 1010          | 1010011     | 1010011  |     0 | ✅ No Error       |
| Bit Error | 1010          | 1010011     | 1010001  |     1 | ❌ Error Detected |
| Bit Error | 1010          | 1010011     | 1000011  |     1 | ❌ Error Detected |
| Bit Error | 1100          | 1100001     | 1101001  |     1 | ❌ Error Detected |


---

## GitHub Submission Checklist

- [ ] Final PPT uploaded
- [ ] `error_detection.circ` uploaded
- [ ] Project implementation files uploaded
- [ ] Circuit screenshot uploaded
- [ ] Simulation/result screenshot uploaded
- [ ] README updated with actual student names and roll numbers where required
- [ ] All work committed and pushed to the repository
- [ ] Repository checked before final submission

---

## Academic Note

This repository is prepared for the **DDCA academic project** on the design and simulation of a digital data transmission error detection system. The documentation should be updated with the team's actual student details, screenshots, measured simulation outputs, and final implementation before submission.
