# 🔋 2S Battery Management System (BMS) with Power Output Module

This out of pcb components implements a **2S (2-cell) Lithium-ion Battery Management System** with integrated:
- Charging via USB-C
- Cell balancing & protection
- Voltage regulation
- Power output (USB / external load)

It is designed for portable electronics applications requiring **stable 5V output** from Li-ion batteries.

---

## ⚙️ System Architecture

### 🔹 Key Functional Blocks
1. **Battery Pack**
   - 2 × 3.7V Li-ion cells (2S configuration)
   - Nominal voltage: 7.4V
   - Fully charged: 8.4V

2. **Charging Module**
   - USB-C input
   - 2S battery charging module

3. **BMS Protection Circuit**
   - Overcharge protection
   - Over-discharge protection
   - Short-circuit protection
   - Cell balancing

4. **Voltage Regulation**
   - Buck converter to step down voltage
   - Output regulated to **5V**

5. **Output Module**
   - USB output / load connection
   - AMS1117 regulator for stable 5V rail

---

## 🔌 Circuit Description

### 🔋 Battery Section
- Two 18650 Li-ion cells connected in **series**
- Provides:
  - B+ (8.4V max)
  - B- (ground)
  - Midpoint for balancing

---

### 🔌 Charging Section
- USB-C provides input power
- Charging module manages:
  - Constant Current (CC)
  - Constant Voltage (CV)

---

### 🛡️ BMS Section
The BMS monitors:
- Individual cell voltages
- Current flow

**Protections:**
- Overcharge cutoff (~4.2V per cell)
- Over-discharge cutoff (~2.7–3.0V per cell)
- Overcurrent & short-circuit protection

---

### 🔻 Voltage Conversion
- Buck converter reduces voltage from:
  - **7.4–8.4V → 5V**
- Ensures stable output for electronics

---

### 🔌 Output Stage
- AMS1117 regulator used for fine voltage regulation
- Output options:
  - USB port (for powering MCU/devices)
  - Direct DC output

---

## 🧰 Components Used

| Component | Description |
|----------|------------|
| 18650 Li-ion Cells | Rechargeable batteries |
| 2S BMS Module | Battery protection |
| USB-C Charging Module | Battery charging |
| Buck Converter | Voltage step-down |
| AMS1117 (5V) | Linear voltage regulator |
| Switch | Power control |
| Wires & Connectors | Interconnections |

---

## 🔄 Working Principle

### 1. Charging Mode
- USB-C supplies power
- Charging module charges battery pack safely
- BMS ensures protection during charging

### 2. Discharging Mode
- Battery supplies power through BMS
- Buck converter steps down voltage to 5V
- Output powers connected devices

### 3. Protection Mode
- BMS disconnects load if:
  - Voltage too low/high
  - Current too high

---

## ⚠️ Safety Notes

- Never short-circuit battery terminals  
- Use **matched cells** (same capacity & health)  
- Ensure proper insulation of wiring  
- Avoid overcharging beyond 8.4V  
- Use heat-resistant casing  

---


