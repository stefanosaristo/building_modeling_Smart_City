# Smart Cities - Building Energy Modeling - Session 1
## Single room with 3 concrete and a double-glazed window wall - Thermal analysis

---

> **Building:** 4 m × 6 m × 3 m room — 3 vertical concrete walls, 1 horizontal concrete ceiling, 1 south wall fully double-glazed (glass– air gap – glass), adiabatic floor.

---

## Table of Contents

1. [Description of the Building](#1-description-of-the-building)
2. [Hypotheses](#2-hypotheses)
3. [Thermo-physical & Radiative Properties](#3-thermo-physical--radiative-properties)
4. [Thermal Circuit](#4-thermal-circuit)
5. [Values of Conductances](#5-values-of-conductances)
6. [Mathematical Model](#6-mathematical-model)
7. [Steady-State Thermal Load](#7-steady-state-thermal-load)
8. [Yearly Energy Consumption](#8-yearly-energy-consumption)
9. [Discussion & Key Findings](#9-discussion--key-findings)

---

## 1. Description of the Building

The room analysed is a rectangular single-zone space with the following configuration:

| Parameter | Value |
|---|---|
| Room dimensions | 4.0 m (E–W) × 6.0 m (N–S) × 3.0 m (height) |
| Floor area | 24 m² |
| Room volume | 72 m³ |
| **Ceiling** | Concrete, horizontal, 24 m² |
| **North wall** | Concrete, vertical, 12 m² |
| **East wall** | Concrete, vertical, 18 m² |
| **West wall** | Concrete, vertical, 18 m² |
| **South wall** | Double-glazed window (glass – 16 mm air – glass), 18 m² |
| **Floor** | Adiabatic |
| Total vertical concrete area | 48 m² |

The south wall is entirely glazed and the floor is treated as adiabatic hence no branch is included in the thermal circuit for it.

---

## 2. Hypotheses

### Boundary temperatures

| Parameter | Value |
|---|---|
| Winter design outdoor temperature | 0 °C |
| Summer design outdoor temperature | +35 °C |
| Heating setpoint | 20 °C |
| Cooling setpoint | 26 °C |
| Ground / floor temperature | — (adiabatic floor, not used) |

### Air infiltration

Air changes per hour: **ACH = 0.5 h⁻¹** (windows slightly tilted, blinds closed — after Recknagel et al. 2013).

### Internal heat gains

| Source | Power |
|---|---|
| 4 occupants × 80 W | 320 W |
| Electrical equipment | 200 W |
| **Total Q_int** | **520 W** |

### Simplifying assumptions

- The 3 vertical concrete walls are **lumped** into a single element (same material, same boundary conditions).
- The floor is **adiabatic**: no branch is included (zero heat flux at floor).
- Long-wave radiation is **linearised** around a mean indoor temperature T̄ = 20 °C = 293 K.
- The HVAC system is modelled as a **proportional controller** with gain Kp = 10⁴ W/K (approximation of a perfect controller, θ₈ → T_sp).
- Thermal properties are **constant** (no temperature dependence).
- Steady-state calculation uses **monthly mean** outdoor temperatures and solar irradiances.

---

## 3. Thermo-physical & Radiative Properties

### Materials

| Material | λ [W/(m·K)] | ρ [kg/m³] | c_p [J/(kg·K)] | e [m] |
|---|---|---|---|---|
| Concrete (walls & ceiling) | 1.40 | 2300 | 880 | 0.20 |
| Glass (each pane) | 1.00 | 2500 | 750 | 0.006 |
| Air gap (effective) | 0.025 | 1.2 | 1000 | 0.016 |
| Indoor air | — | 1.2 | 1000 | — |

The air gap conductivity λ = 0.025 W/(m·K) is an **effective value** that accounts for natural convection within the 16 mm gap (pure conduction would give ~0.026 W/(m·K); the effective value is similar due to the narrow gap suppressing convection).

### Radiative & optical properties

| Property | Symbol | Value |
|---|---|---|
| LW emissivity — concrete | ε_w | 0.85 |
| LW emissivity — glass | ε_g | 0.90 |
| SW absorptivity — concrete | α_w | 0.60 |
| SW absorptivity — glass (each pane) | α_g | 0.06 |
| SW transmittance — each glass pane | τ_g | 0.85 |
| SW transmittance — double glazing (total) | τ_dg = τ_g² | 0.722 |
| Stefan–Boltzmann constant | σ | 5.67 × 10⁻⁸ W/(m²·K⁴) |

### Surface convection coefficients

Conventional values from Dal Zotto et al. (2014), Table 1.12.1-4:

| Surface type | Location | Heat flow direction | h [W/(m²·K)] |
|---|---|---|---|
| Vertical wall | Indoor | Horizontal | 7.7 |
| Ceiling | Indoor | Downward (warm air below) | 5.9 |
| All surfaces | Outdoor | Wind-driven | 25.0 |

---

## 4. Thermal Circuit

### Node map (9 temperature unknowns)

The thermal network has **9 nodes** (temperature unknowns θ₀ … θ₈) and **14 branches** (heat flow rates q₀ … q₁₃).

```
  θ₀   outer surface, 3 vertical concrete walls (lumped)
  θ₁   inner surface, 3 vertical concrete walls
  θ₂   outer surface, concrete ceiling
  θ₃   inner surface, concrete ceiling
  θ₄   outdoor face of outer glass pane
  θ₅   indoor face of outer glass / outdoor boundary of air gap
  θ₆   indoor boundary of air gap / outdoor face of inner glass
  θ₇   indoor face of inner glass pane
  θ₈   indoor air
```

Temperature **sources** (known, not unknown nodes):

- **T_o** — outdoor air temperature
- **T_sp** — HVAC setpoint temperature

### Branch list & heat-loss paths

```
  q₀    θ₀ → T_o    outdoor convection, vertical walls
  q₁    θ₁ → θ₀    conduction through concrete walls
  q₂    θ₈ → θ₁    indoor convection, vertical walls
  q₃    θ₂ → T_o    outdoor convection, ceiling
  q₄    θ₃ → θ₂    conduction through concrete ceiling
  q₅    θ₈ → θ₃    indoor convection, ceiling
  q₆    θ₄ → T_o    outdoor convection, outer glass pane
  q₇    θ₅ → θ₄    conduction, outer glass pane
  q₈    θ₆ → θ₅    effective convection, air gap
  q₉    θ₇ → θ₆    conduction, inner glass pane
  q₁₀   θ₈ → θ₇    indoor convection, inner glass pane
  q₁₁   θ₈ → T_o    ventilation / infiltration
  q₁₂   θ₈ → T_sp   HVAC controller  ← THERMAL LOAD BRANCH
  q₁₃   θ₁ → θ₇    LW radiation: inner wall surface ↔ inner glass
```

**Heat-loss paths (winter direction):**

```
  Concrete walls  :  θ₈ → θ₁ → θ₀ → T_o
  Ceiling         :  θ₈ → θ₃ → θ₂ → T_o
  DG window       :  θ₈ → θ₇ → θ₆ → θ₅ → θ₄ → T_o
  Ventilation     :  θ₈ → T_o
  LW radiation    :  θ₁ ↔ θ₇   (bidirectional)
  HVAC            :  T_sp ↔ θ₈
```

The **floor** has no branch — the adiabatic condition means zero heat flux, which is automatically satisfied by omission.

---

## 5. Values of Conductances

### Sign convention

All branch conductances obey:

```
  q_j = G_j · (A[j, from_node]·θ_from − A[j, to_node]·θ_to)  = G_j · (θ_from − θ_to)
```

Positive q_j means heat flows in the arrow direction (outward in winter). The HVAC load q₁₂ < 0 in heating mode (heat flows from T_sp into the room).

### 5.1 Conduction

Formula: **G_cd = (λ / e) · S**

| Branch | Element | λ [W/(m·K)] | e [m] | S [m²] | G [W/K] |
|---|---|---|---|---|---|
| G₁ | Concrete walls | 1.40 | 0.20 | 48 | **336.0** |
| G₄ | Concrete ceiling | 1.40 | 0.20 | 24 | **168.0** |
| G₇ | Outer glass pane | 1.00 | 0.006 | 18 | **3000.0** |
| G₈ | Air gap (effective) | 0.025 | 0.016 | 18 | **28.1** |
| G₉ | Inner glass pane | 1.00 | 0.006 | 18 | **3000.0** |

The air gap G₈ = 28.1 W/K is the thermal bottleneck of the double-glazed window — 107 times lower conductance than each glass pane.

### 5.2 Convection

Formula: **G_cv = h · S**

| Branch | Element | h [W/(m²·K)] | S [m²] | G [W/K] |
|---|---|---|---|---|
| G₀ | Outdoor conv., vert. walls | 25.0 | 48 | **1200.0** |
| G₂ | Indoor conv., vert. walls | 8.0 | 48 | **369.6** |
| G₃ | Outdoor conv., ceiling | 25.0 | 24 | **600.0** |
| G₅ | Indoor conv., ceiling | 8.0 | 24 | **141.6** |
| G₆ | Outdoor conv., outer glass | 25.0 | 18 | **450.0** |
| G₁₀ | Indoor conv., inner glass | 8.0 | 18 | **138.6** |

### 5.3 Long-wave radiation (linearised)

Linearisation around T̄ = 20 °C = 293 K:

```
  T₁⁴ − T₂⁴  ≈  4·σ·T̄³ · (T₁ − T₂)

  4·σ·T̄³ = 4 × 5.67×10⁻⁸ × 293³  ≈  5.73 W/(m²·K)
```

**View factor** (simplified formula, walls → window):

```
  S_total_indoor = S_cv + S_cl + S_w = 48 + 24 + 18 = 90 m²
  F_wg = S_w / (S_total − S_cv) = 18 / 42 = 0.429
```

Three conductances in series (emittance at wall → space → emittance at glass):

```
  G_w   = 4σT̄³ · [ε_w/(1−ε_w)] · S_cv  = 5.73 · (0.85/0.15) · 48  = 1554 W/K
  G_wg  = 4σT̄³ · F_wg · S_cv           = 5.73 · 0.429 · 48        =  118 W/K
  G_g   = 4σT̄³ · [ε_g/(1−ε_g)] · S_w   = 5.73 · (0.90/0.10) · 18  =  927 W/K
```

Equivalent series conductance (branch G₁₃):

```
  G₁₃ = 1 / (1/1554 + 1/118 + 1/927) = 1 / 0.01020  =  97.7 W/K
```

### 5.4 Advection (ventilation)

```
  Va     = l_x × l_y × h = 4 × 6 × 3 = 72 m³
  V̇_a   = (ACH / 3600) × Va = (2 / 3600) × 72 = 0.04 m³/s
  G₁₁   = ρ_a · c_a · V̇_a = 1.2 × 1000 × 0.04  =  48.0 W/K
```

### 5.5 HVAC proportional controller

```
  G₁₂ = Kp = 10,000 W/K   (approximation of a perfect controller)
```

### 5.6 Complete conductance table

| j | Branch | G [W/K] |
|---|---|---|
| 0 | G₀ — outdoor conv., vert. walls | 1200.00 |
| 1 | G₁ — conduction, concrete walls | 336.00 |
| 2 | G₂ — indoor conv., vert. walls | 369.60 |
| 3 | G₃ — outdoor conv., ceiling | 600.00 |
| 4 | G₄ — conduction, concrete ceiling | 168.00 |
| 5 | G₅ — indoor conv., ceiling | 141.60 |
| 6 | G₆ — outdoor conv., outer glass | 450.00 |
| 7 | G₇ — conduction, outer glass pane | 3000.00 |
| 8 | G₈ — effective conv., air gap | 28.13 |
| 9 | G₉ — conduction, inner glass pane | 3000.00 |
| 10 | G₁₀ — indoor conv., inner glass | 138.60 |
| 11 | G₁₁ — ventilation / infiltration | 12.00 |
| 12 | G₁₂ — HVAC controller (Kp) | 10000.00 |
| 13 | G₁₃ — LW radiation, walls ↔ glass | 97.74 |

### 5.7 Equivalent overall U-values (series combination)

For each envelope element, the series of branches gives an overall thermal transmittance:

| Element | U_eq = 1/(1/G_out + 1/G_cd + ... + 1/G_in) | U-value [W/(m²·K)] |
|---|---|---|
| 3 concrete walls (total) | 1/(1/1200 + 1/336 + 1/369.6) = 153.5 W/K | **3.20 W/(m²·K)** |
| Concrete ceiling | 1/(1/600 + 1/168 + 1/141.6) = 68.1 W/K | **2.84 W/(m²·K)** |
| Double-glazed window | 1/(1/450 + 1/3000 + 1/28.1 + 1/3000 + 1/138.6) = 21.9 W/K | **1.22 W/(m²·K)** |

The double-glazed window achieves a significantly better U-value than the uninsulated concrete walls (3.20 W/(m²·K)).

---

## 6. Mathematical Model

### 6.1 Governing equations (DAE system)

The thermal network yields a system of Differential-Algebraic Equations:

```
  Branch equations:    q  = G · (A·θ − b)
  Node energy balance: C·θ̇ = −Aᵀ·q + f
```

Substituting the branch equations into the node balance:

```
  C·θ̇ = −Aᵀ·G·(A·θ − b) + f
  C·θ̇ = −Aᵀ·G·A·θ + Aᵀ·G·b + f
```

**At steady state (θ̇ = 0):**

```
  [Aᵀ·G·A] · θ  =  Aᵀ·G·b + f
         M · θ  =  rhs
```

This is a **9×9 linear system** solved by `numpy.linalg.solve(M, rhs)`.

### 6.2 Incidence matrix A (14 × 9)

Convention: `A[j, from_node] = +1`,  `A[j, to_node] = −1`. Branches to temperature sources only have one non-zero entry (the node column); the source goes into vector b.

```
       θ₀  θ₁  θ₂  θ₃  θ₄  θ₅  θ₆  θ₇  θ₈
q₀      +1   .   .   .   .   .   .   .   .      (θ₀ → T_o)
q₁      −1  +1   .   .   .   .   .   .   .      (θ₁ → θ₀)
q₂       .  −1   .   .   .   .   .   .  +1      (θ₈ → θ₁)
q₃       .   .  +1   .   .   .   .   .   .      (θ₂ → T_o)
q₄       .   .  −1  +1   .   .   .   .   .      (θ₃ → θ₂)
q₅       .   .   .  −1   .   .   .   .  +1      (θ₈ → θ₃)
q₆       .   .   .   .  +1   .   .   .   .      (θ₄ → T_o)
q₇       .   .   .   .  −1  +1   .   .   .      (θ₅ → θ₄)
q₈       .   .   .   .   .  −1  +1   .   .      (θ₆ → θ₅)
q₉       .   .   .   .   .   .  −1  +1   .      (θ₇ → θ₆)
q₁₀      .   .   .   .   .   .   .  −1  +1      (θ₈ → θ₇)
q₁₁      .   .   .   .   .   .   .   .  +1      (θ₈ → T_o)
q₁₂      .   .   .   .   .   .   .   .  +1      (θ₈ → T_sp)
q₁₃      .  +1   .   .   .   .   .  −1   .      (θ₁ → θ₇)
```

Each row has at most two non-zero entries. The matrix is sparse.

### 6.3 Conductance matrix G (14 × 14 diagonal)

```
  G = diag([1200.0, 336.0, 369.6, 600.0, 168.0, 141.6,
            450.0, 3000.0, 28.1, 3000.0, 138.6,
            12.0, 10000.0, 97.7])   W/K
```

G is diagonal — conductances are independent of one another.

### 6.4 Capacity matrix C (9 × 9 diagonal)

Formula: `C_i = ρ · c_p · e · S` (concrete split equally between outer and inner nodes).

```
  C = diag([9715.2, 9715.2, 4857.6, 4857.6,
            202.5, 0.17, 0.17, 202.5, 86.4])   kJ/K
```

| Node | Element | C [kJ/K] |
|---|---|---|
| θ₀ | Outer concrete walls (½ mass) | 9715.2 |
| θ₁ | Inner concrete walls (½ mass) | 9715.2 |
| θ₂ | Outer ceiling (½ mass) | 4857.6 |
| θ₃ | Inner ceiling (½ mass) | 4857.6 |
| θ₄ | Outer glass pane | 202.5 |
| θ₅ | Air gap outer boundary | 0.17 |
| θ₆ | Air gap inner boundary | 0.17 |
| θ₇ | Inner glass pane | 202.5 |
| θ₈ | Indoor air | 86.4 |
| **Total** | | **29,637 kJ/K = 29.6 MJ/K** |

The **concrete walls dominate** thermal mass (19,430 kJ/K out of 29,637 kJ/K). The indoor air holds almost nothing (86 kJ/K).

> **Note:** C is irrelevant for steady-state calculations (θ̇ = 0). It governs the time constant for dynamic response: τ ≈ C/G_total.

### 6.5 Temperature source vector b (length 14)

Non-zero entries only where a branch connects to a temperature source:

```
  b[0]  = T_o    (branch q₀:  θ₀ → T_o,   outdoor conv. walls)
  b[3]  = T_o    (branch q₃:  θ₂ → T_o,   outdoor conv. ceiling)
  b[6]  = T_o    (branch q₆:  θ₄ → T_o,   outdoor conv. outer glass)
  b[11] = T_o    (branch q₁₁: θ₈ → T_o,   ventilation)
  b[12] = T_sp   (branch q₁₂: θ₈ → T_sp,  HVAC setpoint)
  all others = 0
```

### 6.6 Heat flow source vector f (length 9)

Non-zero where solar radiation is absorbed at a surface or where internal gains are present:

```
  f[0] = α_w · E_south · S_cv       (solar on outer concrete wall surfaces)
  f[2] = α_w · E_horiz · S_cl       (solar on outer ceiling surface)
  f[4] = α_g · E_south · S_w        (solar absorbed in outer glass pane)
  f[8] = τ_dg · E_south · S_w + Q_int   (solar transmitted + internal gains)
  all others = 0
```

where E_south and E_horiz are the solar irradiances [W/m²] on the south-facing vertical surface and on the horizontal surface respectively.

**Winter example** (E_south = 300 W/m², E_horiz = 100 W/m²):

| Node | Source | f [W] |
|---|---|---|
| θ₀ | Solar on outer walls | 0.60 × 300 × 48 = **8640** |
| θ₂ | Solar on ceiling | 0.60 × 100 × 24 = **1440** |
| θ₄ | Absorbed in outer glass | 0.06 × 300 × 18 = **324** |
| θ₈ | Solar through DG + internal | 0.722 × 300 × 18 + 520 = **4429** |

### 6.7 System matrix M and solution

The 9 × 9 system matrix is assembled as:

```
  M = Aᵀ · G · A
```

Key structural properties of M:

- **Symmetric**: M = Mᵀ
- **Positive definite**: guaranteed because the network is fully grounded by temperature sources
- **Sparse**: M[i,k] ≠ 0 only if nodes i and k share a branch
- **Diagonal dominant**: M[i,i] = Σⱼ Gⱼ · A[j,i]² (sum of all conductances at node i)
- **Off-diagonal**: M[i,k] = −G_branch_connecting(i,k)

The right-hand side and solution:

```python
M     = A.T @ G_mat @ A          # 9×9 system matrix
rhs   = A.T @ G_mat @ b + f      # 9-element right-hand side
theta = np.linalg.solve(M, rhs)  # 9 unknown temperatures
q     = G_mat @ (A @ theta - b)  # 14 branch heat flow rates
Q_HVAC = q[12]                   # thermal load [W]
                                  # q[12] < 0 → heating
                                  # q[12] > 0 → cooling
```

---

## 7. Steady-State Thermal Load

### Most unfavorable winter conditions

**T_o = −10 °C, T_sp = 20 °C, E_south = 300 W/m², E_horiz = 100 W/m²**

#### Node temperatures

| Node | Description | Temperature (°C) |
|---|---|---|
| θ₀ | Outer surf. concrete walls | 0.18 |
| θ₁ | Inner surf. concrete walls | 10.84 |
| θ₂ | Outer surf. ceiling | −4.49 |
| θ₃ | Inner surf. ceiling | 6.62 |
| θ₄ | Outer glass — outdoor face | −7.96 |
| θ₅ | Outer glass — indoor face | −7.76 |
| θ₆ | Inner glass — outdoor face | 13.38 |
| θ₇ | Inner glass — indoor face | 13.58 |
| **θ₈** | **Indoor air** | **19.80** |

The **large temperature jump** between θ₅ (−7.8 °C) and θ₆ (+13.4 °C) — a difference of 21 °C across just 16 mm of air — confirms that the air gap is the principal thermal resistance of the window (as expected: G₈ = 28 W/K, the bottleneck).

#### Branch heat flows

| Branch | Description | q [W] |
|---|---|---|
| q₀ | Outdoor conv., vert. walls | +12220 |
| q₁ | Conduction, concrete walls | +3580 |
| q₂ | Indoor conv., vert. walls | +3313 |
| q₃ | Outdoor conv., ceiling | +3307 |
| q₄ | Conduction, ceiling | +1867 |
| q₅ | Indoor conv., ceiling | +1867 |
| q₆ | Outdoor conv., outer glass | +919 |
| q₇ | Conduction, outer glass | +595 |
| q₈ | Effective conv., air gap | +595 |
| q₉ | Conduction, inner glass | +595 |
| q₁₀ | Indoor conv., inner glass | +862 |
| q₁₁ | Ventilation (θ₈ → T_o) | +358 |
| **q₁₂** | **HVAC — thermal load** | **−1978** |
| q₁₃ | LW radiation, walls ↔ glass | −268 |

**★ Heating load = 1978 W**

#### Physical consistency check

```
  Heat losses from indoor air:
    Walls (q₂):        3313 W
    Ceiling (q₅):      1867 W
    Window (q₁₀):       862 W
    Ventilation (q₁₁):  358 W
    Total losses:      6400 W

  Heat gains to indoor air:
    Solar through DG + gains (f[8]):  4429 W
    HVAC (|q₁₂|):                    1978 W
    Total gains:                      6407 W  ✓ (≈ 6400, balance satisfied)
```

---

### Most unfavorable summer conditions

**T_o = +36 °C, T_sp = 26 °C, E_south = 700 W/m², E_horiz = 800 W/m²**

#### Node temperatures

| Node | Description | Temperature (°C) |
|---|---|---|
| θ₀ | Outer surf. concrete walls | 49.40 |
| θ₁ | Inner surf. concrete walls | 37.27 |
| θ₂ | Outer surf. ceiling | 52.06 |
| θ₃ | Inner surf. ceiling | 40.87 |
| θ₄ | Outer glass — outdoor face | 37.36 |
| θ₅ | Outer glass — indoor face | 37.32 |
| θ₆ | Inner glass — outdoor face | 32.24 |
| θ₇ | Inner glass — indoor face | 32.19 |
| **θ₈** | **Indoor air** | **27.58** |

**★ Cooling load = 15,825 W**

The cooling demand is 8× higher than the heating demand due to the combination of high outdoor temperature, high solar irradiance, and the large south-facing window transmitting 0.722 × 700 × 18 ≈ **9090 W** of solar radiation directly into the room.

---

## 8. Yearly Energy Consumption

### Climate data — Grenoble, France

| Month | T_out (°C) | E_south (W/m²) | E_horiz (W/m²) |
|---|---|---|---|
| Jan | 0.0 | 130 | 60 |
| Feb | 5.0 | 190 | 100 |
| Mar | 10.0 | 280 | 170 |
| Apr | 15.0 | 360 | 250 |
| May | 20.0 | 420 | 340 |
| Jun | 25.0 | 460 | 400 |
| Jul | 30.0 | 480 | 430 |
| Aug | 30.0 | 420 | 370 |
| Sep | 20.0 | 320 | 260 |
| Oct | 15.0 | 220 | 150 |
| Nov | 10.0 | 140 | 70 |
| Dec | 5.0 | 110 | 50 |

### Monthly HVAC energy

Energy calculation: `E_month = |Q_HVAC| × hours_in_month / 1000` [kWh].

Logic:
- If Q_HVAC < 0 (heating needed): `E_heat = |Q_HVAC| × hours / 1000`
- If Q_HVAC ≥ 0 with T_sp=20°C (room overheats): re-solve with T_sp=26°C; if still Q_HVAC > 0: `E_cool = Q_HVAC × hours / 1000`

| Month | T_out (°C) | Q_HVAC (W) | E_heat (kWh) | E_cool (kWh) |
|---|---|---|---|---|
| Jan | 0.0 | −2312 | 1720.0 | 0.0 |
| Feb | 1.5 | −882 | 592.6 | 0.0 |
| Mar | 5.5 | +1721 | 0.0 | 152.6 |
| Apr | 9.5 | +4175 | 0.0 | 1914.8 |
| May | 14.0 | +6443 | 0.0 | 3665.6 |
| Jun | 17.5 | +8081 | 0.0 | 4726.7 |
| Jul | 20.5 | +9216 | 0.0 | 5728.6 |
| Aug | 20.0 | +8007 | 0.0 | 4829.0 |
| Sep | 15.5 | +5049 | 0.0 | 2543.9 |
| Oct | 10.5 | +1965 | 0.0 | 334.3 |
| Nov | 4.5 | −994 | 715.9 | 0.0 |
| Dec | 1.0 | −2404 | 1788.7 | 0.0 |
| **TOTAL** | | | **4817.2** | **23895.5** |

### Summary

| Metric | Value |
|---|---|
| Total heating energy | **4,817 kWh/year** |
| Total cooling energy | **23,896 kWh/year** |
| **Total HVAC energy** | **28,713 kWh/year** |
| Floor area | 24 m² |
| **Specific consumption** | **1196 kWh/(m²·year)** |

---

## 9. Discussion & Key Findings

### U-values

| Element | U-value [W/(m²·K)] | Reference value |
|---|---|---|
| 3 concrete walls (uninsulated) | **3.20** | Typical uninsulated ~2.5–3.5 |
| Concrete ceiling (uninsulated) | **2.84** | Typical uninsulated ~2.5–3.5 |
| Double-glazed window | **1.22** | EU new-build max ~1.0–1.4 |

The double-glazed window actually outperforms the uninsulated concrete walls (1.22 vs 3.20 W/(m²·K)). Adding insulation to the concrete walls would significantly reduce heating demand.

### Thermal loads

| Condition | Load | Specific load |
|---|---|---|
| Winter peak (T_o = −10 °C) | **1,978 W heating** | 82 W/m² |
| Summer peak (T_o = +36 °C) | **15,825 W cooling** | 660 W/m² |

The cooling load is **8× larger** than the heating load. The dominant cause is the 18 m² south-facing double-glazed window transmitting ~9090 W of solar in summer (τ_dg × 700 W/m² × 18 m²).

### Energy balance

The extreme specific consumption (**1196 kWh/(m²·year)**) is explained by three compounding factors:

1. **Uninsulated concrete envelope** — U = 3.2 W/(m²·K) for the walls and 2.84 W/(m²·K) for the ceiling; massive conductive heat gains in summer and losses in winter.
2. **Large south-facing glazing** (75% window-to-floor ratio) — unshaded DG window transmits enormous solar gains in spring through autumn.
3. **Low thermal inertia of air** vs. heavy concrete mass — the concrete walls (29.6 MJ/K thermal mass) store heat but the indoor air (86 kJ/K) responds instantly to outdoor conditions.

### Recommendations

| Measure | Expected benefit |
|---|---|
| Add exterior insulation (e.g. 8 cm, λ=0.04 W/(m·K)) | Reduce U_walls from 3.2 → ~0.4 W/(m²·K); cut heating by ~60% |
| Add external shading to south window | Cut summer solar gains by 50–80%; reduce cooling by ~4000–8000 kWh/yr |
| Reduce ACH from 0.5 to 0.3 (improved sealing) | Reduce ventilation losses by ~40%; saving ~300–500 kWh/yr |
| Triple glazing (U ≈ 0.7 W/(m²·K)) | Marginal improvement vs. DG on heating (DG already better than walls) |

---

## References

- Dal Zotto, L. et al. (2014). *Manuel de thermique du bâtiment*. Paris: Dunod. (Convection coefficient table)
- Recknagel, H., Sprenger, E., & Schramek, E.R. (2013). *Génie climatique*. (Ventilation rate table)
- Widén, J., & Munkhammar, J. (2019). *Solar Radiation Theory*. Uppsala. (View factor formulas)
- Ghiaus, C. (2003). Free-running building temperature and HVAC climatic suitability. *Energy and Buildings*, 35(4), 405–411.
- Cengel, Y.A. (2014). *Heat and Mass Transfer: Fundamentals & Applications*. McGraw-Hill.

---

*Report generated from Python thermal analysis code — dm4bem methodology (C. Ghiaus, INSA Lyon).*

