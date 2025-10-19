# Week4 DAY5 –**CMOS power supply and device variation robustness evaluation**


<details>
<summary><h2> 🌟 THEORY </h2> </summary>



## CMOS Inverter Robustness — Power Supply Variation

*(Static behavior evaluation under varying supply voltages)*

---

### 🧩 **Theory Section**

### Smart SPICE Simulation for Power Supply Variations

As CMOS technology scales down (from **250 nm → 20 nm** and below), the **supply voltage (VDD)** also scales down proportionally.

For instance:

- A design that once operated at **2.5 V** may now operate at **1.0 V or even 0.7 V**.

This scaling ensures device reliability and reduced power dissipation.

However, it also affects inverter performance — hence the need to **evaluate robustness under power supply scaling**.

**Objective:**

To observe how CMOS inverter characteristics vary when **VDD is swept** from **2.5 V → 0.5 V**.

---

### ⚙️ **Simulation Setup**

| Parameter | Description |
| --- | --- |
| PMOS Width | 0.975 µm |
| NMOS Width | 0.375 µm |
| Technology Node | 250 nm |
| Initial VDD | 2.5 V |
| Sweep Range | 2.5 V → 0.5 V (step = 0.5 V) |
| Load Capacitance | Same as previous inverter experiments |

**PMOS** is made wider to balance resistance with **NMOS** and maintain symmetric transfer characteristics.

---

### **Smart SPICE Control Script**

Inside `.control` and `.endc`, **TCL-style scripting** allows automated looping over multiple VDD values:

```
let power_supply = 2.5
let power_supply_variation = 0

dowhile (power_supply_variation < 5)
  alter vdd = power_supply
  dc vin 0 power_supply 0.01
  plot v(out) vs v(in)
  let power_supply = power_supply - 0.5
  let power_supply_variation = power_supply_variation + 1
end

```

Each loop iteration:

- Alters `VDD` dynamically.
- Executes **DC sweep** for `Vin : 0 → VDD`.
- Plots **Vout vs Vin** (VTC curve).

---

### 📊 **Observation**

The resulting VTCs for multiple voltages (2.5 V, 2.0 V, 1.5 V, 1.0 V, 0.5 V) show:

- Even at **0.5 V**, CMOS inverter retains correct switching behavior.
- However, **transition slope** and **gain** vary notably.

These curves form the foundation for analyzing **advantages and disadvantages** of low-VDD operation.

---

### Advantages and Disadvantages of Using Low Supply Voltage

Now that we have VTCs at multiple VDD levels, let’s evaluate **performance trade-offs.**

---

### ✅ **Advantages of Lowering VDD**

1. **Improved Gain**Av=ΔVinΔVout
    
    Gain is defined as:
    
    Av=ΔVoutΔVinA_v = \frac{\Delta V_{out}}{\Delta V_{in}}
    
    - For VDD = 2.5 V → **Gain ≈ 7.38**
    - For VDD = 0.5 V → **Gain ≈ 11.5**
    
    ⇒ Around **+56% gain improvement**.
    
    A sharper transition region enhances logic integrity and analog amplification.
    

---

1. **Reduced Energy Consumption**E=21CVDD2
    
    Dynamic energy per transition:
    
    E=12CVDD2E = \tfrac{1}{2} C V_{DD}^2
    
    Energy ∝ $V_{DD}^2$
    
    - At 2.5 V → $E \propto (2.5)^2$
    - At 0.5 V → $E \propto (0.5)^2$
    
    ⇒ ~**96% energy reduction** 🔋
    
    Ideal for **mobile, wearable, and IoT applications** where low-power operation is critical.
    

---

### ❌ **Disadvantages of Lowering VDD**

1. **Performance Degradation**
    - Rise delay @ 2.5 V → **66 ps**
    - Fall delay @ 2.5 V → **70 ps**
    - Rise delay @ 1.0 V → **> 220 ps**
    - Fall delay @ 1.0 V → **165 ps**
    
    Reduced voltage lowers **drive strength** → slower transitions.
    
    ⛔ At **0.5 V**, load capacitor may not charge/discharge fully — incomplete logic swing.
    

---

1. **Reduced Robustness**
    - At very low VDD, **noise margin** and **device matching** worsen.
    - Transistors may not enter full conduction region.

---

### ⚖️ **Summary**

| Aspect | High VDD (2.5 V) | Low VDD (0.5 V) |
| --- | --- | --- |
| Gain | 7.38 | 11.5 ↑ |
| Energy | High | 96% ↓ |
| Delay | Fast | Slow (×3–4) |
| Reliability | Strong | Marginal at very low voltage |

---

## CMOS Inverter Robustness – Device Variation

### Sources of Variation – Etching Process

Before simulating variation in a CMOS inverter, we must first **identify the sources of variation** that alter device parameters such as width (W) and length (L).

### Etching Overview

📌 Etching is a **critical fabrication step** that defines transistor geometry — it determines the **shape, height, and width** of active regions and interconnects.

- Defines diffusion boundaries
- Defines gate length
- Directly impacts **W/L ratio → Drain Current (ID)**

### 🧩 Inverter Structure View

![3.png](week4%202915f99c9dcb80978dfbec52a6df43c7/3.png)

- PMOS at top, NMOS at bottom
- Polysilicon gate (red)
- Diffusion areas (green / yellow)
- Metal interconnects (blue)
- Contacts (black crosses)

The etching process determines how precisely these patterns are transferred.

If etching is imperfect, the **actual W & L** differ from designed values.

### ⚙️ Etching Induced Variation

![4.png](week4%202915f99c9dcb80978dfbec52a6df43c7/4.png)

![5.png](week4%202915f99c9dcb80978dfbec52a6df43c7/5.png)

In an ideal case → clean rectangular geometry

In a real case → distorted edges, rounded corners, misalignment

So,

ID=μCoxWL(VGS−VT)2I_D = \mu C_{ox} \frac{W}{L} (V_{GS} - V_T)^2

ID=μCoxLW(VGS−VT)2

Any ΔW or ΔL → direct impact on $I_D$ → impact on **cell delay**.

For a **chain of inverters**, small local W/L changes accumulate → delay mismatch and non-uniform timing across the path.

---

### Sources of Variation – Oxide Thickness

![6.png](week4%202915f99c9dcb80978dfbec52a6df43c7/6.png)

### 🧩 Cross-Section View of a MOSFET

Now we shift from the layout to the **cross-section** of a transistor:

| Region | Description |
| --- | --- |
| Gate Oxide | Thin insulating SiO₂ layer between gate and channel |
| Polysilicon Gate | Conductive control electrode |
| Source / Drain | n⁺ or p⁺ diffused regions |
| Channel | Path between source & drain controlled by V<sub>GS</sub> |

### ⚙️ Ideal vs Real Oxidation Process

- *Ideal:* Uniform oxide thickness throughout channel
- *Real:* Non-uniform oxide due to process imperfections

This variation affects **C<sub>ox</sub>**, and hence $I_D$:

Cox=εoxtoxC_{ox} = \frac{\varepsilon_{ox}}{t_{ox}}

Cox=toxεox

ID∝μ Cox WL(VGS−VT)2I_D \propto \mu\,C_{ox}\,\frac{W}{L}(V_{GS}-V_T)^2

ID∝μCoxLW(VGS−VT)2

So → variation in $t_{ox}$ changes $C_{ox}$ → changes $I_D$ → affects switching threshold and delay.

Even small oxide thickness fluctuations across devices lead to local mismatch in current and hence slight shifts in VTC curves.

---

### Smart SPICE Simulation for Device Variations

### 🎯 Objective

To verify how robust a CMOS inverter is when PMOS and NMOS device sizes are swept across their extremes.

![7.png](week4%202915f99c9dcb80978dfbec52a6df43c7/7.png)

### ⚙️ Simulation Setup

| Parameter | Description |
| --- | --- |
| PMOS Width | Sweep 1.875 µm → 0.375 µm |
| NMOS Width | Sweep 0.375 µm → 1.875 µm |
| L (P & N) | 0.25 µm |
| Steps | 5 (ΔW = 0.375 µm each) |
| VDD | 2.5 V |
| Sweep | Vin 0 → 2.5 V (step 0.01 V) |

### 🧠 Concepts

- **Strong PMOS / Weak NMOS** → High drive on rising edge.
- **Weak PMOS / Strong NMOS** → High drive on falling edge.

### 💻 SPICE Smart Loop

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description

XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

.control

let powersupply = 1.8
alter Vdd = powersupply
	let voltagesupplyvariation = 0
	dowhile voltagesupplyvariation < 6
	dc Vin 0 1.8 0.01
	let powersupply = powersupply - 0.2
	alter Vdd = powersupply
	let voltagesupplyvariation = voltagesupplyvariation + 1
      end
 
plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "input voltage(V)" ylabel "output voltage(V)" title "Inveter dc characteristics as a function of supply voltage"

.endc

.end

```

### 📈 Result

- Each iteration → VTC curve for a different W<sub>p</sub>/W<sub>n</sub> ratio.
- VTC shifts left or right depending on dominant device.
- Despite large W/L changes, inverter retains clear logic transition region.

---

### 🏁 Conclusion

From all device variation experiments we observe:

| Parameter | Observation | Impact |
| --- | --- | --- |
| Switching Threshold (V<sub>M</sub>) | 0.2 V → 1.4 V shift across extremes | Acceptable (≈ ±1.2 V variation on 2.5 V supply) |
| Noise Margin | ~0.4 V (High) → ~0.3 V (Low) | Still within robust range |
| VTC Shape | Retained sharp transition | Logic integrity maintained |
| Functionality | Unaffected | Inverter still behaves as ideal logic gate |

Even under extreme device sizing mismatch, the inverter operates correctly.

Hence, **CMOS logic is intrinsically robust to fabrication and device variations.**

---

### 🧩 Final Understanding

- **CMOS Inverter is Process-Tolerant**: Even under etching / oxide / device variation, logic operation remains stable.
- **Drain Current and VTC** are sensitive but functional region persists.
- **Noise Margins and Switching Thresholds** stay within safe limits.
- This makes CMOS the most reliable building block for digital logic design.

---
</details>



  
<details>
<summary><h2> 🌟 LAB </h2> </summary>


# Sky130 Supply Variation Lab

**Objective:** Verify theoretical trends on Sky130 PDK using NGSPICE.

**Setup Summary:**

- (W/L)_p / (W/L)_n = 2.77
- VDD = 1.8 V → reduced by 0.2 V per step (6 iterations)
- Vin swept 0 → 1.8 V, step = 0.01 V

---

**Procedure:**

1. Run:
    
    ```bash
    nano day5_inv_supplyvariation_Wp1_Wn036.spice 
    
    ngspice day5_inv_supplyvariation_Wp1_Wn036.spice 
    
    ```
    
    ![1.png](week4-lab%202805f99c9dcb80e48e4ee8a3457c6f65/1.png)
    
    ```bash
    
    // observed values fo the 1.8 volt 
    x0 = 0.787387, y0 = 1.678
    
    x0 = 0.992793, y0 = 0.086
    ```
    
2. Observe **VTC curves** for:
    - 1.8 V, 1.6 V, 1.4 V, 1.2 V, 1.0 V, 0.8 V

![2.png](week4-lab%202805f99c9dcb80e48e4ee8a3457c6f65/2.png)

1. Compute **gain** using:Av=ΔVinΔVout
    
    Av=ΔVoutΔVinA_v = \frac{ΔV_{out}}{ΔV_{in}}
    

---

**Observation Table:**

| VDD (V) | Gain |
| --- | --- |
| 1.8 | 7.72 |
| 1.6 | 8.9 |
| 1.4 | 9.3 |
| 1.2 | 9.5 |
| 1.0 | ↓ (~7.0) |
| 0.8 | ↓ (< 7.0) |

At very low VDD, PFET/NFET fail to turn ON fully → gain drops again.

---

**Lab Insight:**

- Gain ↑ as VDD ↓ (until transistor drive becomes too weak).
- Optimum operation typically lies around **1.0–1.2 V** for Sky130.

---

### 🧪 **Lab Summary**

**Experiment:** CMOS Inverter under power supply variation

**Tool:** NGSPICE + Sky130 PDK

| Parameter | Variation | Key Observation |
| --- | --- | --- |
| VDD | 2.5 → 0.5 V | Functional even at 0.5 V |
| Gain | Increases then drops | Optimum near 1 V |
| Energy | Decreases ∝ VDD² | ~90–96% saving |
| Delay | Increases with lower VDD | Performance trade-off |

---

### ⚙️ **Core Understanding**

- **Power scaling** improves efficiency but impacts **speed and noise immunity**.
- There exists a **sweet spot voltage** where gain, delay, and energy are well balanced.
- Smart SPICE scripting enables efficient multi-VDD characterization in one run.

---

# Sky130 Device Variation Lab

### 🎯 Objective

To validate device variation robustness using Sky130 PDK in NGSPICE.

### ⚙️ Setup

| Parameter | Value |
| --- | --- |
| PFET Width (Wp) | 7 µm |
| NFET Width (Wn) | 0.42 µm |
| L (P & N) | 0.15 µm (default for Sky130) |
| VDD | 1.8 V |
| Vin Sweep | 0 → 1.8 V (step 0.01 V) |

### 💻 Run

```bash
ngspice inv_device_variation.spice
plot v(out) vs v(in)
```

![8.png](week4-lab%202805f99c9dcb80e48e4ee8a3457c6f65/8.png)

![9.png](week4-lab%202805f99c9dcb80e48e4ee8a3457c6f65/9.png)

### 📊 Observation

- Output holds at **VDD (1.8 V)** for longer duration → strong PFET drive.
- **Switching Threshold ≈ 0.988 V** vs ideal 0.9 V.
    
    → Δ ≈ 80 mV only (≈ 4.4 % of VDD).
    

### ✅ Inference

Even for large device mismatch (PFET × 16.6 stronger than NFET),

variation in switching threshold is minor (< 100 mV).

Hence, **CMOS inverter in Sky130 is highly robust to device variation.**

---

### 🧭 **Summary of Device Variation Robustness**

| Aspect | Observation | Conclusion |
| --- | --- | --- |
| Etching Process | Alters W/L | Impacts delay & $I_D$ linearly |
| Oxide Thickness | Alters $C_{ox}$ | Impacts $I_D$ & threshold slightly |
| Device Size Sweep | VTC shifts but logic intact | CMOS remains robust |
| Sky130 Lab | ΔV<sub>M</sub> ≈ 80 mV (max) | Very robust device behavior |
|  |  |  |

---

</details>

