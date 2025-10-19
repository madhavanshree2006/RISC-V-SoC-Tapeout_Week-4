# Week4 DAY4 –**CMOS Inverter Noise Margin – Concept, Behavior & Lab Evaluation**


<details>
<summary><h2> 🌟 THEORY </h2> </summary>


## 🔹 What is Noise Margin? (Core Idea)

Every digital gate (including a CMOS inverter) must tolerate certain variations in voltage due to **glitches, crosstalk, and supply noise**. The **noise margin** quantifies how much noise a gate can withstand without logic failure.

![1.png](week4%202915f99c9dcb80978dfbec52a6df43c7/1.png)

To understand this, you imagine the **VTC (Voltage Transfer Curve)** of the inverter:

- X-axis: Vin
- Y-axis: Vout
- As Vin goes from 0 → VDD, Vout drops from VDD → 0.

In an **ideal case**, the transition is vertical at VDD/2 (infinite slope), but practically:

- Due to series resistances & device capacitances,
- The curve slopes gradually in the switching region.

So we extract practical voltage thresholds:

- **VOH** → Maximum output high (≈ near VDD)
- **VOL** → Minimum output low (≈ near 0V)
- **VIL** → Max input voltage still treated as logic 0
- **VIH** → Min input voltage treated as logic 1

![2.png](week4%202915f99c9dcb80978dfbec52a6df43c7/2.png)

---

## 🔹 Logical Interpretation

- Input between **0 → VIL** → Output stays high (VOH)
- Input between **VIH → VDD** → Output goes low (VOL)
- Any region **between VIL and VIH** → Undefined behavior

This undefined window is where logic can become unstable if noise/glitch lands there.

---

## 🔹 Noise Margin Equations

From the voltage map:

```
0        VOL    VIL    VIH    VOH      VDD
|---------|------|------|------|---------|
  Logic 0  |undef|      undef   | Logic 1

```

We define:

✅ **Noise Margin Low (NML)**

NML=VIL−VOL\text{NML} = V_{IL} - V_{OL}

NML=VIL−VOL

✅ **Noise Margin High (NMH)**

NMH=VOH−VIH\text{NMH} = V_{OH} - V_{IH}

NMH=VOH−VIH

These indicate how much noise can be tolerated at logic-0 and logic-1 levels respectively.

Any glitch/bump:

- If it stays inside **NML / NMH region** → SAFE
- If it enters **undefined region** → Risky
- If it crosses into opposite logic window → Logic failure

---

## 🔹 PMOS Width Impact on Noise Margin

As PMOS size increases (keeping NMOS same):

| PMOS Size vs NMOS | Effect on NMH | Effect on NML |
| --- | --- | --- |
| Equal sizing | Balanced | Balanced |
| 2× PMOS | NMH ↑ | NML ~ same |
| 3× PMOS | NMH ↑ more | NML ~ same |
| 4× PMOS | NMH ↑ slightly | NML ↓ small |
| 5× PMOS | NMH peaks | NML small drop |

Key takeaways:

- PMOS controls how strongly logic ‘1’ is preserved.
- Larger PMOS improves **NMH**.
- Beyond a limit, further increase reduces **NML** (NMOS becomes weaker in comparison).

This variation demonstrates **CMOS inverter robustness** — even with fabrication variations, noise margins stay in safe ranges.

---

## ✅ Final Understanding

✔ CMOS inverter noise robustness is determined using NMH & NML

✔ These come from the DC transfer curve by locating threshold transitions

✔ PMOS sizing directly affects noise margins

✔ Lab results verified the theoretical expectations

✔ Even with sizing variations, CMOS logic maintains safe operational stability
---

</details>
<details>
<summary><h2> 🌟 LAB </h2> </summary>


## 🔹 Lab Validation (Ngspice)

Using **Sky130 PDK** and `ngspice`, a DC sweep was done:

- Input (Vin): 0 → 1.8V in steps of 0.01
- Plot: `Vout vs Vin`

By clicking on slopes where derivative ≈ −1, we identified:

![3.png](week4%202915f99c9dcb80978dfbec52a6df43c7/3.png)

![4.png](week4%202915f99c9dcb80978dfbec52a6df43c7/4.png)

Example values:d

- VOH ≈ 1.75 V
- VOL ≈ 0.084 V
- VIL ≈ 0.737 V
- VIH ≈ 0.996 V

From this:

```
NMH = VOH − VIH  ≈ 1.75  − 0.996 = 0.754 V
NML = VIL − VOL  ≈ 0.737 − 0.084 = 0.653 V
```

These confirm:

- Wide tolerance for both low and high noise
- CMOS inverter remains immune to glitches in most ranges

---

</details>
