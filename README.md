<!-- Do Not Change the below line. It will affect your workflow badge -->
[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/9lqHKop1)

![Docs Added](https://github.com/edl-iitb/edl-26-project-submission-edl26_mon04/actions/workflows/classroom.yml/badge.svg)

<!-- ADD YOUR CHANGES AFTER THIS LINE -->

# IM-PULSE

## Team Information
**Team name:** IM-PULSE (Group MON-04)

**Team members:**
- Yashvardhan Khandegar — 23B1227
- Sudhindra Sahoo — 23B1312
- Jahnvi Sharma — 23B1926
- Vinay Hariharan — 23B1310
- Krunal Vaghela — 23B1262

## Project Abstract
IM-PULSE is a compact, portable biomedical monitoring device developed in the Electronic Device Lab (EDL) course at IIT Bombay. The system simultaneously acquires ECG and dual Arterial Pulse Waveforms (APW) from bioimpedance (IPG) measurements using six electrodes (chest + foot + abdomen reference). It performs onboard ECG R-peak detection and APW foot detection on an STM32F411 Black Pill, computes Pulse Transit Time (PTT), derives Pulse Wave Velocity (4–10 m/s), and estimates cuffless blood pressure via a pre-trained XGBoost model. Data is wirelessly streamed through an ESP32 bridge to a host laptop, where a PyQt6 desktop GUI provides live plotting and a Flask web dashboard delivers real-time vitals to any device with access to the internet.

The device solves the discomfort and bulkiness of conventional inflatable-cuff BP monitors and traditional ECG/Bio-Z instruments, offering a convenient solution for continuous or frequent cardiac health monitoring for cases where such devices might be inaccesible or unaffordable. This repository contains the complete embedded firmware, host software, hardware documentation, milestone reports.

## Deliverables in This Repository
This repository includes the following deliverables:

- STM32F411 Black Pill firmware for signal acquisition and on-board processing
- ESP32 firmware for wireless UART-to-UDP bridging
- Host-side Python software for desktop visualization and web dashboard delivery
- A pre-trained XGBoost model used for blood-pressure estimation
- Runtime state shared between the desktop app and the web server
- Setup notes and implementation notes for each software component
- Report PDFs documenting project progress (M0–M4)
- Images showing the PCB, CAD model, soldering, printed enclosure, simulation output, and final assembled device

## Repository Structure
### Root directory
- `README.md` — Main project overview and documentation entry point.
- `src/` — Source code and implementation files for the embedded and host software.
- `reports/` — PDF report submissions for the project.
- `others/` — Supporting media and supplementary documentation.

### `src/`
This folder contains the core implementation of the system.

#### `src/Display & Webserver/`
Host-side Python software running on a PC or laptop.

- `display.py` — PyQt6 desktop application, UDP receiver, packet parser, signal-processing engine, live plots, and vitals writer.
- `webserver.py` — Flask web server that reads the vitals state file and serves the browser dashboard.
- `bp_xgboost_model.pkl` — Pre-trained XGBoost model for cuffless blood pressure estimation.
- `vitals_state.json` — Shared JSON state updated by the desktop app and read by the web server.
- `README.md` — Component overview and architecture notes for the host-side software.
- `SETUP.md` — Setup and run instructions for the host-side software.

#### `src/ESP 32/`
ESP32 firmware used as the wireless communication bridge.

- `Firmware.ino` — Arduino sketch for receiving UART data from the STM32 and forwarding it over UDP.
- `README.md` — Overview of the ESP32 role, hardware connections, and design decisions.
- `SETUP.md` — Flashing and setup instructions for the ESP32 module.

#### `src/STM32F411 Black Pill/`
Main embedded firmware for the biomedical acquisition device.

- `Firmware.ino` — Arduino-based STM32 firmware for acquisition, processing, packet creation, and serial transmission.
- `README.md` — Overview of the STM32 role, hardware connections, and processing flow.
- `SETUP.md` — Flashing and setup instructions for the Black Pill board.

### `reports/`
This folder contains the submitted report files for the project.

- `MON-04-M0.pdf`
- `MON-04-M1.pdf`
- `MON-04-M2.pdf`
- `MON-04-M3.pdf`
- `MON-04-M4.pdf`

### `others/`
This folder contains supporting material that documents the physical build and testing process.

#### `others/images/`
Images of the PCB, CAD model, soldering process, enclosure, and final assembled device.
This folder contains PCB images, CAD images, soldering images, the printed box, and the final assembled device.

#### `others/simulation/`
Simulation-related image evidence.


## Notes
- The repository currently uses a mixed hardware/software structure, so the source tree is divided by platform.
- Folder names with spaces have been preserved exactly as they appear in the repository.


# Reflections, Challenges & Future Work

## 11. Learning, Experiences & Key Takeaways

### Project Status Summary

| Specification / Requirement | Status | Remarks / Justification |
|:----------------------------|:------:|:------------------------|
| 4-electrodes only, two on each wrist | ⚠️ Incomplete | Wrist APW unviable — instead we used 6 electrodes (chest (2) + foot (2) + abdomen reference (2)) |
| Compact portable table-top unit (bio-signal acquisition, processing & wireless) | ✅ Complete | |
| Onboard ECG R-peak & APW foot detection on STM32 | ✅ Complete | |
| Plot ECG + proximal & distal APW, verify reliability | 🔶 Partial | Chest ECG & APW reliable; foot APW less regular due to lower SNR — heart rate cross-verified with clinical device |
| Obtain Pulse Wave Velocity in typical range (4–10 m/s) | ✅ Complete | |
| Derive other health parameters from PTT and cross-verify them | 🔶 Partial | We focused on blood pressure as research has shown negative PTT–BP correlation; model trained on single subject (data limitation) |

---

### Challenges & How We Overcame Them

#### Dual-Purpose BioZ (Respiration + APW for PTT)

**Problem:** The MAX30001's BioZ channel conflates respiration and blood-related IPG signals. Respiration-focused acquisition introduced a DC offset and saturation, distorting the APW signal needed for PTT computation.

**Solution:** We reconfigured the channel for blood IPG (APW) by removing respiration-centric filtering, applying a band-pass filter (~0.8–5 Hz), and syncing the APW with ECG R-peaks to correctly compute PTT.

---

#### Electrode Placement & Signal Integrity

**Problem:** Accurate proximal–distal APW measurement required very precise electrode placement, and signals were frequently distorted by environmental interference from nearby charged sources.

**Solution:** After extensive experimentation, we finalized electrode positions over the chest (proximal) and left foot (distal) arteries to capture stable APW. Interference was eliminated by grounding nearby charged objects in the measurement environment.

---

#### PTT–BP Modeling with a Custom Dataset

**Problem:** No reliable publicly available dataset exists for heart-to-foot PTT vs blood pressure correlation. Our raw measurements were noisy and unstructured, making it difficult to train a meaningful model.

**Solution:** We collected approximately 200 samples using reference blood pressure measurements alongside our PTT readings, cleaned and processed the raw data, and trained a regression model to learn the PTT–BP mapping and extract meaningful patterns.

---

#### STM32 Chip Mismatch (F411CE vs F401CE)

**Problem:** The BlackPill boards purchased were labelled STM32F411CE, but many units in the market are actually equipped with an STM32F401CE chip. This silent mismatch caused repeated code upload failures and cost significant debugging time before the root cause was identified.

**What We Learned:** Always verify the actual chip ID using STM32CubeProgrammer before beginning development. Physical markings on a board cannot always be trusted, especially with generic suppliers.

---

### Key Takeaways

**Technical:**
- Bio-signal acquisition requires careful analog front-end tuning — default configurations are rarely sufficient for multi-purpose sensors like the MAX30001.
- Electrode placement has a profound impact on signal quality; this is as much a physiological problem as an electronics one.
- Building a custom dataset from scratch is time-consuming but necessary when the target measurement (heart-to-foot PTT) does not have existing benchmarks.
- Hardware debugging (chip mismatches, board variants) can consume a disproportionate amount of time — always validate components early.

**Team & Process:**
- A modular PCB and software architecture paid off: when individual components failed or needed replacement, the rest of the system remained unaffected.
- Separating the signal processing pipeline from the hardware layer early allowed the software team to develop and test algorithms in parallel with PCB bring-up.
- Time lost on hardware issues compressed the software iteration cycles — future projects should budget extra time for hardware validation in the first two weeks.

---

## 12. Incomplete Features & Future Work

### Features Not Completed Within the Course Timeline

- **Wrist-based APW measurement:** The original specification required only 4 electrodes placed on the wrists. Wrist APW turned out to be physiologically unviable for our sensing configuration — the signal was too weak and inconsistent for reliable PTT computation. This remains unresolved.
- **Multi-subject PTT–BP model:** The trained blood pressure model was built on data from a single subject due to time constraints. A generalised model requires a larger and more diverse dataset, which was not feasible within the semester.
- **Full cross-verification of derived health parameters:** While blood pressure estimation via PTT was partially achieved, other health parameters derivable from PTT (e.g., arterial stiffness index) were not implemented or verified.

---

### Concrete Future Work

**1. Expand the PTT–BP dataset and improve the model**
The current model is trained on a single subject (~200 samples), which limits its generalisability. Future work would involve collecting data across multiple subjects with varying age, fitness levels, and blood pressure ranges, and retraining the model. A more robust architecture (e.g., a subject-adaptive or personalised calibration model) could significantly improve accuracy.

**2. Improve foot APW signal quality with better analog filtering**
The foot APW signal had lower SNR compared to the chest signal. This could be addressed by adding a programmable gain stage or a narrower band-pass filter specifically tuned for the distal IPG signal, implemented either in hardware (op-amp filter stage on PCB) or through tighter DSP filtering on the STM32.

**3. Investigate wrist-based electrode configuration**
The wrist APW failure could be revisited by exploring different electrode configurations (e.g., radial artery targeting) or switching to PPG-based sensing at the wrist as a complementary approach for PTT. This would bring the device closer to the original wearable specification.

**4. Reduce wireless latency for real-time monitoring**
The current UDP-over-WiFi transmission introduces latency that makes real-time waveform monitoring less smooth. Switching to a lower-latency protocol (e.g., BLE with a custom GATT profile) or implementing a lightweight compression scheme before transmission could improve the live display experience without requiring a wired connection.

**5. Onboard blood pressure display**
Currently, BP estimation requires post-processing on a connected laptop. Integrating a small OLED display on the device and running the trained model directly on the ESP32 (after model quantisation) would make the device fully self-contained and usable without a computer.