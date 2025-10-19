<h1 align="center">🔳 RISC-V SoC Tapeout Program — Week 4️⃣</h1>

<p align="center"><img src=" DAY1/ASSETS/0.png" width="500" alt="image 0"/></p>



---

<div align="center">

# ⚡ Week 4 — CMOS Power Supply & Device Variation Robustness (Ngspice + Sky130)

🌟 This is **Week 4** of the **VSD RISC-V SoC Tapeout Program** —

I explored **power-supply variation** and **device-variation robustness** of a CMOS inverter

using **Ngspice simulation + Sky130 PDK models**.

This week was all about analyzing how **process variations, oxide thickness, etching irregularities, and supply fluctuations** impact the **static transfer characteristics (VTC)**,

and how to design for **robust CMOS behavior** under real-world manufacturing deviations. ⚙️

</div>

---

## 🎯 Week 4 Objectives

✔️ Analyze **CMOS inverter robustness** against **power-supply variations** 🔋

✔️ Simulate **VTC curves under VDD sweep** using Ngspice 📈

✔️ Explore **advantages/disadvantages of low-supply voltage** ⚖️

✔️ Study **device-level variations** → etching & oxide thickness impact 🧪

✔️ Perform **Smart Spice simulation for device variation models** 🧠

✔️ Compare **strong/weak device pairings (PMOS vs NMOS)** 🧩

✔️ Evaluate **noise margins, switching threshold (V_M)** across corners 🎚️

✔️ Run **Sky130 variation labs** to observe practical layout/process impact 🧷

---

## ✅ Tasks Completed

| 📝 Task | 📌 Description | 🎯 Status |
| --- | --- | --- |
| 1 | Simulated **power-supply variation** in CMOS inverter (Ngspice) | ✅ Done |
| 2 | Plotted **VTC curves for different VDD levels** | ✅ Done |
| 3 | Compared **noise margins vs VDD** | ✅ Done |
| 4 | Discussed **pros/cons of low-supply operation** | ✅ Done |
| 5 | Studied **etching process as a source of variation** | ✅ Done |
| 6 | Analyzed **oxide-thickness impact on V_T & I_D** | ✅ Done |
| 7 | Ran **Smart Spice device-variation simulations** | ✅ Done |
| 8 | Observed **V_M shifts due to PMOS/NMOS strength changes** | ✅ Done |
| 9 | Executed **Sky130 device variation labs** | ✅ Done |
| 10 | Summarized **robustness and tolerance findings** | ✅ Done |

---

## 📒 Key Learnings

### 📌 Day 1 & 2 — Power Supply Variation Analysis

- Varying **VDD** from 1.6 V → 1.8 V → 2.0 V affects inverter **switching point (V_M)** and **noise margins**.
- Higher VDD ⇒ faster transition, larger noise margin, but higher power.
- Lower VDD ⇒ energy efficient, but reduced logic swing & noise immunity.
- **Ngspice sweeps** show smooth VTC shifts proving design robustness over ±10 % supply variation.

### 📌 Day 3 — Device Variation Sources

- **Etching process variation** alters channel dimensions (W/L), impacting drive current ID∝(W/L)I_D ∝ (W/L)ID∝(W/L).
- **Oxide-thickness variation (ΔT_ox)** affects gate capacitance and threshold voltage.
- These physical variations cause functional mismatch between nominally identical devices.

### 📌 Day 4 — Smart Spice Device Variation Simulation

- Simulated cases:
    - **Strong PMOS / Weak NMOS**
    - **Weak PMOS / Strong NMOS**
    - **Nominal devices**
- Observed V_M shifts toward stronger device’s transfer characteristic.
- Despite these shifts, **Sky130 inverter remains functionally robust**, proving good device matching and process control.

### 📌 Day 5 — Sky130 Device Variation Lab

- Used Sky130 PDK device models to observe practical impact of ΔW, ΔL, ΔT_ox.
- Verified that small process changes (±10 %) lead to minor VTC shifts — well within noise margin limits.
- Concluded that **the CMOS inverter is inherently robust to moderate supply and device variation**.

---

## 🛠️ Tools in Use

- **Ngspice** → Analog simulation of CMOS inverter VTC ⚙️
- **Sky130 PDK** → Process device models for NMOS/PMOS 🧱
- **Smart Spice** → Variation simulation and parameter sweeps 📉
- **Python + Matplotlib** → Custom plotting of VTC curves 📊

---

> 💡 “Week 4 gave me a deep appreciation for how device physics and manufacturing variations affect circuit-level behavior.
From supply voltage sweeps to oxide thickness deviation, I learned how to analyze and design for robustness using Ngspice and Sky130.” 🚀
>
---
## 🙏 Special Thanks 👏  
I sincerely thank all the organizations and their key members for making this program possible 💡:  

- 🧑‍🏫 **VLSI System Design (VSD)** – [Kunal Ghosh](https://www.linkedin.com/in/kunal-ghosh-vlsisystemdesign-com-28084836/) for mentorship and vision.  
- 🤝 **Efabless** – [Michael Wishart](https://www.linkedin.com/in/mike-wishart-81480612/) & [Mohamed Kassem](https://www.linkedin.com/in/mkkassem/) for enabling collaborative open-source chip design.  
- 🏭 **[Semiconductor Laboratory (SCL)](https://www.scl.gov.in/)** – for PDK & foundry support.  
- 🎓 **[IIT Gandhinagar (IITGN)](https://www.linkedin.com/school/indian-institute-of-technology-gandhinagar-iitgn-/?originalSubdomain=in)** – for on-site training & project facilitation.  
- 🛠️ **Synopsys** – [Sassine Ghazi](https://www.linkedin.com/in/sassine-ghazi/) for providing industry-grade EDA tools under C2S program.  
--- 
👉 Main Repo Link : [[https://github.com/madhavanshree2006/RISC-V-SoC-Tapeout-Program](https://github.com/madhavanshree2006/RISC-V-SoC-Tapeout-Program)]
