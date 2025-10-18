# week4 DAY1- Basics of NMOS Drain current (Id) vs Drain-to-source Voltage (Vds)

<details>
<summary><h2> 🌟 THEORY </h2> </summary>

### **1️⃣ Circuit Design Basics**

- Circuit design revolves around creating logic gates such as **AND, OR, NOR, Inverter, and Buffer**.
- All gates are built using **PMOS and NMOS transistors** connected in specific configurations.
- Example:
    - **Inverter** – one PMOS (top) and one NMOS (bottom).
        
        <p align="center"><img src="./ASSETS/1.png" width="350" alt="image 1"/></p>
        
    
    - The **drains** are tied together → output node.
    - The **gates** are connected → input node.
    - This combination inverts the logic level.
- By varying how PMOS and NMOS are connected, we obtain other gates (AND, NOR, etc.).
- Thus, **circuit design = choosing transistor types and connections to achieve desired logic.**

---

### **2️⃣ SPICE Simulations Overview**

- Once the circuit is designed, it must be **fed with input waveforms** to analyze output behavior.
- SPICE simulates the electrical characteristics of each transistor to produce **voltage–time waveforms**.
    
    <p align="center"><img src="./ASSETS/2.png" width="700" alt="image 2"/></p>

    

- These waveforms show:
    - **Propagation delay**
    - **Rise and fall times**
    - **Drive strength** of the transistors
- The **W/L ratio (width/length)** of each transistor affects the **drain current**, which determines waveform shape and delay.
- SPICE allows us to **tune the W/L ratio** to optimize delay performance.

---

### **3️⃣ Why SPICE Matters**

<p align="center"><img src="./ASSETS/3.png" width="700" alt="image 3"/></p>


- In VLSI flow (CTS, STA, Crosstalk, Physical Design), the term “SPICE” isn’t explicitly used — but its results form the **foundation of all timing models**.
- Without SPICE characterization:
    - There would be **no accurate delay data**,
    - Hence, no valid clock tree, timing, or crosstalk analysis.
- SPICE provides the **characterization data** from which **delay tables** (used in STA tools) are generated.

---

### **4️⃣ Example: Delay Table Generation**

- Example of two buffer cells: **CBuff1** and **CBuff2.**
    
<p align="center"><img src="./ASSETS/4.png" width="500" alt="image 4"/></p>
    

- Each has a **delay table** with parameters:
    - **Input Slew (transition time at input)**
    - **Output Load (capacitance at output)**
- Table entries contain **delay values** for various combinations of slew and load.
- For example:
    
<p align="center"><img src="./ASSETS/5.png" width="700" alt="image 5"/></p>
    

- If input slew = 80 ps, output load = 70 fF → delay = x22 (CBuff1).
- For the same conditions in CBuff2 → delay = y24.
- Differences arise due to transistor sizing (**W/L ratio differences**) → different drive strengths → different delays.
- Hence, **circuit design choices directly affect delay tables.**

---





## **NMOS: Structure → Terminals → Threshold idea**

### 1️⃣ What is an NMOS?

<p align="center"><img src="./ASSETS/6.png" width="500" alt="image 6"/></p>

- **N-channel MOSFET** built on a **p-type substrate** (pMOS is the mirror: p-channel on **n-type**).
- **Four terminals:** **G**ate (G), **D**rain (D), **S**ource (S), **B**ody/Substrate (B).
    - In logic cells, **B is usually tied to ground (0 V)**.
    - ⚠️ Body terminal **matters** (body effect → threshold tuning).
    

---

### 2️⃣ Physical structure (cross-section mental model)

<p align="center"><img src="./ASSETS/7.png" width="400" alt="image 7"/></p>

- **Isolation regions** on left/right → electrically separates neighboring transistors.
- **n+ diffusion** regions placed with a gap → become **Source** and **Drain**.
- Over the substrate gap: **Gate stack** = thin **gate oxide** + **poly/metal gate**.
- Gate–oxide–substrate behave like a **capacitor** (metal–oxide–semiconductor).

> 🧭 Where this helps later: SPICE models use this geometry (W, L, oxide thickness) to predict Id–V behavior & delay.
> 

---

### 3️⃣ Role of each terminal (quick intuition)

- **Gate (G):** voltage-controlled “knob”; sets surface charge via the MOS **capacitor**.
- **Source (S) & Drain (D):** n+ regions between which current flows when a channel forms.
- **Body (B):** p-substrate reference; **V_SB** shifts threshold (**body effect**). Usually at 0 V.

---

### 4️⃣ Zero-bias (V_GS = 0, all nodes at 0 V)

<p align="center"><img src="./ASSETS/8.png" width="400" alt="image 8"/></p>

- Two back-to-back **p–n junctions** (p-substrate ↔ n+ source/drain) are **reverse-biased/off**.
- No conduction path **S ↔ D** → **very high resistance** channel (device “OFF”).

---

### 5️⃣ Turn the gate up: capacitor view → charge choreography

<p align="center"><img src="./ASSETS/9.png" width="500" alt="image 9"/></p>

- Apply small **+V_G**: gate plate gets **+ charge** → repels **holes** (majority carriers) from the surface.
- Near the surface: holes deplete → leave behind **negatively charged acceptor ions** → **depletion region** forms.
- Increase **V_G** further: attract **electrons** (minority carriers in p-substrate) to the surface → eventually create an **n-type inversion layer** bridging **S**–**D**.
- The gate voltage at which a **strong, continuous channel** forms is the **threshold voltage, V_T**.

---

### 6️⃣ Why **V_T** is the star of SPICE 🌟

- In compact models, **V_T = f(tech/process, oxide, doping, V_SB, temperature, etc.)**
- Changing **B** (substrate potential) changes **V_T** (**body effect**).
- Accurate **V_T** ⇒ accurate **Id–V** ⇒ accurate **cell delay** tables.

> 📌 we’ll first internalize what sets V_T (qualitatively), then connect it to Id–V in linear/sat regions, and finally to gate delays.
> 

---






## ⚡ NMOS Operation — From Depletion to Inversion

### Step 1: What Happens When We Increase Gate Voltage (V<sub>GS</sub>)

<p align="center"><img src="./ASSETS/10.png" width="700" alt="image 10"/></p>

At the end of the previous discussion, we had:

- A **p-type substrate**
- **n⁺ source** and **n⁺ drain** regions
- A small **positive gate potential** → repelling holes (positive charges) and leaving **negative charges** near the oxide–substrate interface

Now let’s go deeper 👇

---

### Formation of the Depletion Region

<p align="center"><img src="./ASSETS/11.png" width="550" alt="image 11"/></p>

- As soon as a **positive gate voltage** is applied, holes are pushed deeper into the substrate.
- This leaves behind a region **depleted** of its majority carriers (holes).
- The area behaves like a **reverse-biased p–n junction** — a depletion region is formed.
- This region expands as V<sub>GS</sub> increases

🧠 *Key idea:* Depletion region = portion of substrate depleted of majority carriers (holes).

---

### Step 2: The Birth of an n-Channel (Inversion)

<p align="center"><img src="./ASSETS/12.png" width="700" alt="image 12"/></p>

As V<sub>GS</sub> continues to increase:

1. More holes are repelled 
2. The **depletion region widens** until…
3. A thin surface layer under the gate becomes **n-type** (inverted region)

This process is called **strong inversion** 

👉 The gate voltage at which this occurs = **Threshold Voltage (V<sub>T</sub>)**

VGS=VT  ⟹  Strong Inversion Begins!V_{GS} = V_T \implies \text{Strong Inversion Begins!}

VGS=VT⟹Strong Inversion Begins!

At this point, the transistor **turns ON** — an n-type channel forms, connecting **source → drain**.

---

### Channel Formation Dynamics

- Once the inversion layer appears, **electrons** from the n⁺ source are attracted toward the channel region.
- The **channel width (W<sub>ch</sub>)** increases as V<sub>GS</sub> rises.
- The **depletion width (W<sub>d</sub>)**, however, saturates — it doesn’t increase further because the substrate is already depleted.

📈 Result:

- Wider channel → Easier current flow (when V<sub>DS</sub> is applied).
- At V<sub>GS</sub> < V<sub>T</sub> → Cutoff region (no conduction).

---

### Step 3: The Role of the Body Terminal (Substrate Effect)

Normally, the **body (B)** terminal is grounded. But changing its potential affects **V<sub>T</sub>** — this is called the **Body Effect** or **Substrate Bias Effect**.

Let’s consider two cases:

| Case | Description | Observation |
| --- | --- | --- |
| **1️⃣** | V<sub>SB</sub> = 0 (body grounded) | Normal depletion width |
| **2️⃣** | V<sub>SB</sub> > 0 (source positive wrt body) | Larger depletion width due to extra reverse bias |

⚡ Because of the **increased reverse bias**, the depletion region widens, requiring a **higher V<sub>GS</sub>** to reach inversion.

Thus, **threshold voltage increases** with positive substrate bias.

🧠 *Summary:*

$$
↑VSB⇒↑VT↑ V_{SB} \Rightarrow ↑ V_T
$$

$$
↑VSB⇒↑VT
$$

---








## 🌩️ NMOS — Threshold Voltage and Body Effect

<p align="center"><img src="./ASSETS/13.png" width="700" alt="image 13"/></p>

When a positive substrate bias (**V<sub>SB</sub> > 0**) is applied:

- The **depletion region** widens further.
- **Negative charges** accumulate near the gate-oxide interface.
- Because the substrate is at a positive potential, it **pulls these charges** towards itself.
- This makes it harder for inversion to occur → we need a **higher V<sub>GS</sub>** to form a channel.

---

### Observations

| Condition | Description | Result |
| --- | --- | --- |
| V<sub>SB</sub> = 0 | Normal NMOS – channel forms at V<sub>GS</sub> = V<sub>T0</sub> | Easier inversion |
| V<sub>SB</sub> > 0 | Substrate attracts electrons → delays inversion | Needs extra ΔV<sub>1</sub> to invert |

So,

$$
VT=VT0+ΔV1
$$

That additional potential (ΔV₁) is due to the **body effect**.

---

### Threshold Voltage Equation

<p align="center"><img src="./ASSETS/14.png" width="400" alt="image 14"/></p>

$$
VT=VT0+γ(2φF+VSB−2φF)
$$

Where:

- **V<sub>T0</sub>** → Threshold voltage when V<sub>SB</sub> = 0
- **γ (Gamma)** → Body effect coefficient
- **φ<sub>F</sub>** → Fermi potential (from semiconductor physics)
- **V<sub>SB</sub>** → Substrate to source bias

---

### ⚡ Body Effect Coefficient

$$
γ=√(2qεsiNA)/Cox

$$

- **q** → Electronic charge
- **ε<sub>si</sub>** → Permittivity of silicon
- **N<sub>A</sub>** → Substrate doping concentration
- **C<sub>ox</sub>** → Oxide capacitance (depends on oxide thickness)

💡 *All these are foundry constants*, supplied through the SPICE model card.

---

### 📐 Fermi Potential

$$
φF=−VTln(NA/ni)
$$

This represents the energy difference between intrinsic and Fermi levels in the substrate.

---

### 🔬 SPICE Perspective

- Foundry provides **V<sub>T0</sub>**, **γ**, and **φ<sub>F</sub>**.
- These constants define your **NMOS device model** in SPICE.
- When you run simulation, SPICE uses these to compute **V<sub>T</sub>** dynamically as V<sub>SB</sub> changes.

---
<br><br>




## ⚙️ NMOS — Resistive (Linear / Ohmic) Region

### 1️⃣ Transition from Cutoff → Linear

<p align="center"><img src="./ASSETS/15.png" width="400" alt="image 15"/></p>

- In **cutoff region**, $V_{GS} < V_T$ → no inversion → transistor **OFF**.  
- When $V_{GS} \ge V_T$ → an **n-type inversion channel** forms.  
- Apply a small $V_{DS}$ → current starts flowing → device behaves **like a resistor** → *Resistive (Linear) Region*.


---

### 2️⃣ Channel Formation & Charge Control

- Channel charge density ∝ $(V_{GS} - V_T)$  
- As $V_{GS}$ increases → more inversion charge → stronger conduction path 🧠  
- Effective channel width (conduction area) expands with $V_{GS}$  

---

### 3️⃣ Voltage Variation Along Channel

- When **$V_{DS} > 0$**:  
  - Source end → $V(0) = 0$  
  - Drain end → $V(L) = V_{DS}$  

- Local gate-to-channel voltage at any point **$x$**:  

$$
V_{GC}(x) = V_{GS} - V(x)
$$

⇒ **Effective overdrive:**  

$$
V_{GS} - V(x) - V_T
$$


### 4️⃣ Channel Length Reality

- The **effective channel length** $(L_{\text{eff}})$ ≠ **drawn length** due to fabrication effects (diffusion, overlap, etc.)  
- Always consider **$L_{\text{eff}}$** for accurate modeling ✨  

---

### 5️⃣ Drain Current (Linear Region Approximation)

After integrating across the channel:

$$
I_D \approx \mu C_{\text{ox}} \frac{W}{L} \left( V_{GS} - V_T - \frac{V_{DS}}{2} \right) V_{DS}
$$

- Valid for **small $V_{DS}$**  
- Device acts like a **voltage-controlled resistor** ⚡

---









## NMOS — Voltage, Induced Charge & Drift Current

## 🔹 Voltage Along the Channel — $V(x)$

<p align="center"><img src="./ASSETS/16.png" width="500" alt="image 16"/></p>

- $V(x)$ is the local potential at position $x$ along the MOSFET channel (from source to drain).  
- At the source: $V(0) = 0\ \text{V}$  
- At the drain: $V(L) = V_{DS}$

**Effective channel voltage:**

$$
V_\text{eff}(x) = V_{GS} - V(x)
$$

**Examples (inline):**

- At source: $V_{GS}=1\ \text{V},\ V(0)=0 \Rightarrow V_\text{eff}(0)=1\ \text{V}$  
- At drain: $V(L)=0.05\ \text{V} \Rightarrow V_\text{eff}(L)=0.95\ \text{V}$

👉 Because $V(x)$ varies from $0$ to $V_{DS}$, the **channel sees a voltage gradient**.

If $V_{DS}=0$, the channel voltage would be constant.

---

## 🔹 Induced Channel Charge

The local inversion charge density is proportional to the local gate overdrive $(V_{GS} - V(x) - V_T)$.

Inline:

$$
\text{Charge} \propto (V_{GS} - V(x) - V_T)
$$

Main equation:

$$
Q(x) = C_\text{ox} \cdot \bigl(V_{GS} - V(x) - V_T \bigr)
$$

Where:

$$
C_\text{ox} = \frac{\varepsilon_\text{ox}}{t_\text{ox}}
$$

and

- $\varepsilon_\text{ox} = 3.97 \cdot \varepsilon_0$  
- $t_\text{ox}$ = oxide thickness (technology-dependent)

> These parameters ($C_\text{ox}$, $t_\text{ox}$, etc.) form the foundation for SPICE models.
--- 

## 🔹 Current Mechanisms in Device Physics

Two types of current exist in MOSFETs:

- **Drift current ⚡** — caused by electric field / potential difference.  
- **Diffusion current 🌊** — caused by carrier concentration gradients (analogy: full tank → empty tank).

In this NMOS case (source at $0\ \text{V}$, drain at $V_{DS}$), **drift current dominates**.

---

## 🔹 Drift Current — Concept to Equation

Drift current is determined by charge motion under the electric field:

**Conceptual form:**

$$
I = (\text{carrier velocity}) \times (\text{charge density}) \times (\text{area})
$$

For total drain current (2D channel):

$$
I_D = \iint_\text{channel area} v(x) \, q(x) \, dA
$$

For a uniform width $W$:

$$
I_D = W \int_0^L v(x) \, q(x) \, dx
$$

Where:

- $v(x)$ = carrier drift velocity  
- $q(x) = C_\text{ox} \, (V_{GS} - V(x) - V_T)$  
- $W$ = channel width, $L$ = channel length  

---

## 🔹 Substitution for Electric Field

Drift velocity: $v(x) = \mu E(x)$, where $E(x) = -\dfrac{dV(x)}{dx}$

Substitute into $I_D$:

$$
I_D = W \int_0^L \mu \, E(x) \, \bigl[ C_\text{ox} (V_{GS} - V(x) - V_T) \bigr] \, dx
$$

With $E(x) = -\dfrac{dV(x)}{dx}$, transform the integral over $x$ to a voltage integral:

$$
I_D = W \, \mu \, C_\text{ox} \int_0^{V_{DS}} (V_{GS} - V - V_T) \, dV
$$

---

## 🔹 Linear Region (Low $V_{DS}$ Approximation)

Evaluating the integral:

$$
I_D \approx \mu \, C_\text{ox} \, \frac{W}{L} \, \left[ (V_{GS} - V_T) V_{DS} - \frac{V_{DS}^2}{2} \right]
$$

Simplified for small $V_{DS}$ (linear/resistive region):

$$
I_D \approx \mu \, C_\text{ox} \, \frac{W}{L} \, \left( V_{GS} - V_T - \frac{V_{DS}}{2} \right) V_{DS}
$$

✅ Valid when $V_{GS} > V_T$ and $V_{DS}$ is small.
---









## NMOS — Drain Current Model Derivation (Linear Region)

In this section, we derive a **simple working model for the NMOS drain current ($I_D$)** suitable for SPICE simulations.

We start from the theoretical expression:

$$
I_D = v_n(x) \cdot Q_i(x) \cdot W
$$

Where:

- $v_n(x)$ = electron drift velocity at position $x$  
- $Q_i(x)$ = inversion charge density at position $x$  
- $W$ = channel width  

---

## 🔹 Velocity and Charge Substitution

Electron velocity is given by the **mobility × electric field**:

$$
v_n(x) = \mu_n E(x) = \mu_n \frac{dV(x)}{dx}
$$

The inversion charge density is:

$$
Q_i(x) = C_\text{ox} \, (V_{GS} - V(x) - V_T)
$$

Substituting these into the expression for $I_D$:

$$
I_D = W \int_0^L v_n(x) \, Q_i(x) \, dx 
      = W \int_0^L \mu_n \frac{dV(x)}{dx} \, C_\text{ox} \, (V_{GS} - V(x) - V_T) \, dx
$$

## 🔹 Changing Variables (Voltage Integral)

Using $dV = \frac{dV}{dx} dx$, the integral becomes:

$$
I_D = W \, \mu_n \, C_\text{ox} \int_0^{V_{DS}} (V_{GS} - V - V_T) \, dV
$$

Integrating:

$$
I_D = W \, \mu_n \, C_\text{ox} \Big[ (V_{GS} - V_T)V_{DS} - \frac{V_{DS}^2}{2} \Big]
$$

This is the **first-order drain current equation**.

---

## 🔹 Introducing Process Transconductance

Define the **process transconductance**:

$$
K_N = \mu_n \, C_\text{ox} \frac{W}{L}
$$

Then the drain current equation becomes:

$$
I_D = K_N \Big[ (V_{GS} - V_T) V_{DS} - \frac{V_{DS}^2}{2} \Big]
$$

- $K_N$ depends on **mobility**, **oxide capacitance**, and **device geometry**  
- This is the **basic SPICE model form**

---


## 🔹 Linear Region Approximation

<p align="center"><img src="./ASSETS/17.png" width="500" alt="image 17"/></p>

For **$V_{DS} \ll V_{GS} - V_T$** (small drain voltage), the quadratic term is negligible:

$$
\frac{V_{DS}^2}{2} \approx 0
$$

Thus, the drain current simplifies to a **linear relation**:

$$
\boxed{
I_D \approx K_N (V_{GS} - V_T) V_{DS}
}
$$

> ✅ Confirms the linear (resistive) region operation: $V_{DS} < V_{GS} - V_T$
---















# NMOS — Linear Region Drain Current & $V_{GS}/V_{DS}$ Sweep

## 🔹 Linear Region Condition

For a MOSFET to operate in the **linear (resistive) region**, the drain-source voltage must satisfy:

$$
V_{DS} \le V_{GS} - V_T
$$

In this region, the drain current is given by the simplified linear equation:

$$
I_D = K_N \, (V_{GS} - V_T) \, V_{DS}
$$

Where:

- $K_N$ = process transconductance constant (technology-dependent)  
- $V_T$ = threshold voltage  

---

## 🔹 Impact of $V_{GS}$ on $I_D$

The MOSFET drain current depends **linearly on $V_{GS} - V_T$** for small $V_{DS}$.

Applications may use low or high $V_{GS}$, so we need to analyze how $I_D$ varies for different $V_{GS}$ levels.

**Example Sweep:**

| $V_{GS}$ (V) | Max $V_{DS}$ (V) for Linear Region |
|---------------|------------------------------------|
| 0.5           | 0.05                               |
| 1.0           | 0.55                               |
| 1.5           | 1.05                               |
| 2.0           | 1.55                               |
| 2.5           | 2.05                               |

**Observation:** At each $V_{GS}$, $V_{DS}$ must be swept from 0 up to $V_{GS}-V_T$ to remain in the linear region.

---

## 🔹 Why Manual Calculation is Impractical

Calculating $I_D$ manually for all combinations of $V_{GS}$ and $V_{DS}$ is tedious.  

A more efficient approach is to use **SPICE simulations**:

1. Input the circuit into SPICE.  
2. Specify a $V_{GS}$ sweep (0 → 2.5 V in steps, e.g., 0.5 V).  
3. Specify a $V_{DS}$ sweep (0 → $V_{GS}-V_T$).  
4. SPICE calculates $I_D$ and plots the waveforms automatically.
---











## NMOS — Pinch-Off & Saturation Region

### 🔹 Recap: Linear Region ⚡

- Previously:
  - $V_{GS} = 1\ \text{V}$
  - $V_{DS} = 0.05\ \text{V}$
  - $V_T = 0.45\ \text{V}$

- Channel voltage:

$$
V_\text{channel} = V_{GS} - V(x)
$$

- Condition for linear region:

$$
V_{GS} - V_{DS} > V_T
$$

---

### 🔹 Increasing Drain-to-Source Voltage ⬆️

- Gradually increase $V_{DS}$ from **0.05 → 0.95 V** (steps of 0.1 V)
- Channel voltage at drain end:


<p align="center"><img src="./ASSETS/18.png" width="500" alt="image 18"/></p>


$$
V_{\text{channel, drain}} = V_{GS} - V_{DS}
$$

- Table for clarity:

| $V_{DS}$ (V) | $V_{\text{channel, drain}}$ (V) | Status |
|---------------|---------------------------------|--------|
| 0.05          | 0.95                            | ✅ Linear, channel present |
| 0.15          | 0.85                            | ✅ Linear |
| 0.25          | 0.75                            | ✅ Linear |
| 0.35          | 0.65                            | ✅ Linear |
| 0.45          | 0.55                            | ✅ Linear |
| 0.55          | 0.45                            | ⚠️ Pinch-off starts |
| 0.65          | 0.35                            | 🔴 Saturation begins |
| 0.75          | 0.25                            | 🔴 Saturation |
| 0.85          | 0.15                            | 🔴 Saturation |
| 0.95          | 0.05                            | 🔴 Saturation |

> Observation: As $V_{DS}$ increases, the channel near the drain disappears when $V_{GS} - V_{DS} \le V_T$.

---

### 🔹 Pinch-Off Phenomenon ✂️

<p align="center"><img src="./ASSETS/19.png" width="500" alt="image 19"/></p>


- Occurs when:

$$
V_{GS} - V_{DS} \le V_T
$$

- Channel near the **drain** vanishes, but the **source side channel** remains.
- Current continues to flow — **linearity breaks**.
- Marks the **start of the saturation region**.

---

### 🔹 Saturation Region Condition 🔴

- Device enters saturation when:

$$
V_{DS} \ge V_{GS} - V_T
$$

- **Drain current no longer increases linearly** with $V_{DS}$.  
- SPICE simulations can automatically capture this behavior.

---











# NMOS — Saturation Region & Drain Current

## 🔹 Recap: Linear vs Saturation Region

<p align="center"><img src="./ASSETS/21.png" width="700" alt="image 21"/></p>

- In the **linear region ⚡**:
  - Channel voltage: $V_\text{channel} = V_{GS} - V_{DS}$
  - Drain current $I_D$ is **linearly dependent on $V_{DS}$**
- As $V_{DS}$ increases ⬆️:
  - Channel near the drain **pinches off ✂️** when $V_{GS} - V_{DS} \le V_T$
  - This marks the **start of the saturation region **

---

## 🔹 Channel Behavior in Saturation

- In the **saturation region 🔴**:
  - The channel voltage near the drain **remains constant**:

  $$
  V_\text{channel} \approx V_{GS} - V_T
  $$
  
  - Drain-side channel vanishes, leaving a **small, stable conduction path near the source**
- Unlike linear region, drain current **no longer strongly depends on $V_{DS}$**
- Effective channel length may still be **slightly modulated** by $V_{DS}$ and $V_{GS}$ (channel length modulation )

---

## 🔹 Saturation Drain Current Equation

- Starting from linear-region current:

$$
I_D = K_N \left[ (V_{GS} - V_T) V_{DS} - \frac{V_{DS}^2}{2} \right]
$$

- Replace $V_{DS}$ with $V_{GS} - V_T$ for saturation:

$$
I_{D(\text{sat})} = K_N \frac{(V_{GS} - V_T)^2}{2} ⚡
$$

- **Key points:**
  - $I_D$ becomes **independent of $V_{DS}$**
  - Acts as a **constant current source ** for fixed $V_{GS}$
  - $K_N$ = process transconductance parameter (depends on $\mu_n$, $C_\text{ox}$, $W/L$)

---

## 🔹 Channel Length Modulation (λ) 📏

<p align="center"><img src="./ASSETS/20.png" width="500" alt="image 20"/></p>

- Effective channel length decreases slightly as $V_{DS}$ or $V_{GS}$ increases
- Introduce **channel length modulation factor** $(1 + \lambda V_{DS})$:

$$
I_{D(\text{sat})} = \frac{K_N}{2} (V_{GS} - V_T)^2 (1 + \lambda V_{DS})
$$

- Accounts for **minor increase in $I_D$** with $V_{DS}$ in saturation
  
> ✅ The MOSFET in saturation behaves **almost like a constant current source **, controlled primarily by $V_{GS}$
---

</details>
































<details>
<summary><h2> 🌟 LAB </h2> </summary>

# SPICE Model Parameters — What Are They? ⚡

- Think of a **MOSFET in SPICE** as not just a symbol on a schematic, but as a **mathematical model**.
- To simulate it, SPICE needs **numbers** that describe how the real device behaves physically. These numbers are called **SPICE model parameters**.

📌Some of the important SPICE Model parameter 

### **1️⃣ Threshold and Gate Control**

| Parameter | Description |
| --- | --- |
| **VTO / VT0** | Threshold voltage (V). Voltage at which the MOSFET starts conducting. |
| **KP / μCox(W/L)** | Transconductance parameter (A/V²). Determines current drive capability. |
| **GAMMA** | Body effect coefficient (V^0.5). Modulates VT with substrate bias. |



### **2️⃣ Channel Modulation & Length Effects**

| Parameter | Description |
| --- | --- |
| **LAMBDA (λ)** | Channel-length modulation. Causes slight ID increase in saturation with VDS. |
| **THETA (θ)** | Mobility degradation factor. Reduces carrier mobility at high VGS. |
| **VMAX / VSAT** | Velocity saturation. Limits current in short-channel devices. |


### **3️⃣ Capacitances (for AC / transient analysis)**

| Parameter | Description |
| --- | --- |
| **Cgs, Cgd, Cgb** | Gate-source, gate-drain, gate-bulk capacitances. |
| **PB** | Bulk junction potential (V). |
| **MJ / MJSW** | Junction grading coefficients (controls junction capacitance behavior). |
| **CJSW, CJ** | Sidewall and bottom junction capacitances. |


### **4️⃣ Leakage and Subthreshold**

| Parameter | Description |
| --- | --- |
| **IS** | Reverse saturation current of junctions. |
| **N** | Subthreshold slope factor. |
| **VBI / PB** | Built-in potential of source/drain junctions. |


### **5️⃣ Misc / Geometry**

| Parameter | Description |
| --- | --- |
| **L, W** | Channel length and width (μm). Directly affects drain current. |
| **TOX** | Oxide thickness. Affects gate capacitance. |
| **UO / μn, μp** | Carrier mobility. Affects transconductance. |

---
## SPICE Simulation Flow

<p align="center"><img src="./ASSETS/22.png" width="400" alt="image 22"/></p>

---

## How to Write a SPICE Model for a MOSFET

SPICE needs a **model card** — a short line of text that defines all the important parameters of your MOSFET.

It’s like giving SPICE a **personality file 🧠** for your transistor.

---

### 🔹 1. The Basic Syntax

```
.MODEL <model_name> <device_type> (parameter1=value1 parameter2=value2 ...)
```

For an example, let us take a circuit 
<p align="center"><img src="./ASSETS/23.png" width="600" alt="image 23"/></p>


# How to Write a SPICE Model for a MOSFET

SPICE needs a **model card** — a short line of text that defines all the important parameters of your MOSFET.

It’s like giving SPICE a **personality file 🧠** for your transistor.


```
Ml vdd n100 nmos W=1.8u L=1.2u
R1 in n1 55
Vdd vdd 0 2.5
Vin in 0 2.5
```

---

### 🔹 Line 1 — MOSFET Declaration

```
Ml vdd n100 nmos W=1.8u L=1.2u
```

✨ **Meaning:**

| Element | Node |
| --- | --- |
| `Ml` | → Name of the MOSFET |
| `vdd` | → Drain terminal |
| `n100` | → Gate terminal |
| (missing) | → Source terminal (maybe tied to ground) |
| `nmos` | → Model name (should be defined by a `.MODEL` statement) |
| `W=1.8u` | → Channel width = 1.8 µm |
| `L=1.2u` | → Channel length = 1.2 µm |

---

### 🔹 Line 2 — Load Resistor

```
R1 in n1 55
```

A simple resistor between nodes `in` and `n1` with resistance **55 Ω**.

Used as a **load or bias resistor** in this setup.

---

### 🔹 Line 3 — Supply Voltage Source

```
Vdd vdd 0 2.5
```

⚡ Connects the supply rail:

- Positive terminal → node `vdd`
- Negative terminal → ground (`0`)
- DC voltage = **2.5 V**

---

### 🔹 Line 4 — Input Voltage Source

```
Vin in 0 2.5

```

Drives the **input signal**:

- `in` = gate input
- DC level = **2.5 V**

If it’s meant to be a **pulse input**, you could replace with:

```
Vin in 0 PULSE(0 2.5 0 1n 1n 10n 20n)

```

(for transient analysis)

---

### 🔹 Missing Model Definition

You’ll need a model like this at the end:

```
.MODEL nmos NMOS (VTO=0.7 KP=50u LAMBDA=0.02)

```

---

### 🔹 Simulation Directive (Optional)

Add a simulation command such as:

```
.DC Vin 0 2.5 0.1
.PRINT DC I(Vdd)
.END

```
---

## Adding Technology File & Simulation Commands in SPICE

---

### 🔹 Step 1: Linking the Technology / Model File
<p align="center"><img src="./ASSETS/24.png" width="600" alt="image 24"/></p>



To make SPICE understand **how the MOSFET behaves**, we must include the **technology model file** (also known as the **process file**).

This file defines all device-level parameters like threshold voltage, mobility, oxide thickness, etc.

👉 Two main ways to include it:

```
* Option 1 — Include model directly
.INCLUDE "tsmc_025um_model.mod"

* Option 2 — Include a specific section from a library
.LIB "tsmc_025um_model.lib" TT

```


- `.INCLUDE` → pulls the entire file
- `.LIB` → loads a **specific process corner** (like TT, SS, FF)

 **TT** → Typical-Typical

 **SS** → Slow-Slow

 **FF** → Fast-Fast

These corners simulate different fabrication conditions.

---

### 🔹 Step 2: Writing the Netlist

Example (NMOS circuit):

```
*** NMOS Saturation Test Circuit ***

M1 vdd n1 0 0 nmos W=1.8u L=1.2u
R1 n1 0 55
Vdd vdd 0 2.5
Vin n1 0 2.5

.LIB "tsmc_025um_model.lib" TT

```

⚙️ **Explanation:**

- `M1` → NMOS transistor
- `R1` → Load resistor
- `Vdd` → Supply voltage
- `Vin` → Input gate voltage
- `.LIB` → Technology model reference

---

### 🔹 Step 3: Adding Simulation Commands

These commands tell SPICE *what kind of analysis to perform* 🧠

### 🔸 DC Sweep — For I–V Characteristics

```
.DC Vin 0 2.5 0.1
.PRINT DC I(Vdd)

```

### 🔸 Transient Analysis — For Time Response

```
.TRAN 1u 10u
.PRINT TRAN V(n1)

```

### 🔸 AC Analysis — For Frequency Response

```
.AC DEC 10 1Hz 10MegHz
.PRINT AC V(n1)

```

---

### 🔹 Step 4: Ending the Simulation

Always close the file properly 👇

```
.END

```
---





## Ngspice - Simulation

First step clone this git repo...

```bash
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
```
<p align="center"><img src="./ASSETS/25.png" width="700" alt="image 25"/></p>



➤ now the directory will be looks like this 

```bash
~/Desktop/open_source_tapout/week4/
└── sky130CircuitDesignWorkshop
    ├── README.md
    └── design
        ├── day1_nfet_idvds_L2_W5.spice
        ├── day2_nfet_idvds_L015_W039.spice
        ├── day2_nfet_idvgs_L015_W039.spice
        ├── day3_inv_tran_Wp084_Wn036.spice
        ├── day3_inv_vtc_Wp084_Wn036.spice
        ├── day4_inv_noisemargin_wp1_wn036.spice
        ├── day5_inv_devicevariation_wp7_wn042.spice
        ├── day5_inv_supplyvariation_Wp1_Wn036.spice
        └── sky130_fd_pr
            ├── LICENSE
            ├── README.rst
            ├── models
            │   ├── all.spice
            │   ├── parameters
            │   └── sky130.lib.spice
            └── cells
                ├── nfet_01v8
                │   ├── sky130_fd_pr__nfet_01v8.pm3.spice
                │   ├── sky130_fd_pr__nfet_01v8__ff.corner.spice
                │   ├── sky130_fd_pr__nfet_01v8__ff.pm3.spice
                │   ├── sky130_fd_pr__nfet_01v8__fs.corner.spice
                │   ├── sky130_fd_pr__nfet_01v8__fs.pm3.spice
                │   ├── sky130_fd_pr__nfet_01v8__mismatch.corner.spice
                │   ├── sky130_fd_pr__nfet_01v8__sf.corner.spice
                │   ├── sky130_fd_pr__nfet_01v8__sf.pm3.spice
                │   ├── sky130_fd_pr__nfet_01v8__ss.corner.spice
                │   ├── sky130_fd_pr__nfet_01v8__ss.pm3.spice
                │   ├── sky130_fd_pr__nfet_01v8__tt.corner.spice
                │   └── sky130_fd_pr__nfet_01v8__tt.pm3.spice
                └── pfet_01v8
                    (similar structure as nfet_01v8)

```

<p align="center"><img src="./ASSETS/26.png" width="700" alt="image 26"/></p>



🚩 lets explore what inside a actual spice model 

```bash
cd design 
ls

nano day1_nfet_idvds_L025_W065.spice
```
<p align="center"><img src="./ASSETS/27.png" width="700" alt="image 27"/></p>


<p align="center"><img src="./ASSETS/28.png" width="700" alt="image 28"/></p>


👉 Navigate to the design folder 

```bash
~/Desktop/open_source_tapout/week4/sky130CircuitDesignWorkshop/design: 
```

‼️ If you dont have the ngspice download it 

```bash
sudo apt install ngspice
```
📌 After installed the ngspice 

```
ngspice day1_nfet_idvds_L025_W065.spice
```

<p align="center"><img src="./ASSETS/29.png" width="700" alt="image 29"/></p>

---

📌 Then plot the vdd 
```
plot -vdd#branch
```
<p align="center"><img src="./ASSETS/30.png" width="700" alt="image 30"/></p>


- left click on the wave form to know waht is the x annd y parameter values at the exact point
<p align="center"><img src="./ASSETS/31.png" width="700" alt="image 31"/></p>

---


</details>




















