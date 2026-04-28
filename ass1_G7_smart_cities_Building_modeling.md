# Smart Cities - Building Energy Modeling - Session 1 - Group 7 (Aristodemou, Benito, Navarro, Guilherme)
## Single room with 3 concrete and a double-glazed window wall - Thermal analysis

---

> **Building:** 4 m × 6 m × 3 m room, 3 vertical concrete walls, 1 horizontal concrete ceiling, 1 south wall fully double-glazed (glass – air gap – glass), adiabatic floor.

---

## Table of Contents

1. [Description of the Building](#1-description-of-the-building)
2. [Hypotheses](#2-hypotheses)
3. [Thermal & Radiative Properties](#3-thermal-&-radiative-properties)
4. [Thermal Circuit](#4-thermal-circuit)
5. [Values of Conductances](#5-values-of-conductances)
6. [Mathematical Model](#6-mathematical-model)
7. [Steady-State Thermal Load](#7-steady-state-thermal-load)
8. [Yearly Energy Consumption](#8-yearly-energy-consumption)
9. [Summary of previous findings](#9-summary-of-previous-findings)

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

The south wall is entirely glazed and the floor is treated as adiabatic hence no branch is included in the thermal circuit for it. The modeled building has no insulation.

---

## 2. Hypotheses

### Boundary temperatures

| Parameter | Value |
|---|---|
| Winter design outdoor temperature | 0 °C |
| Summer design outdoor temperature | 35 °C |
| Heating setpoint | 20 °C |
| Cooling setpoint | 26 °C |
| Ground / floor temperature | —  |

### Air infiltration

Air changes per hour: **ACH = 2** 

### Internal heat gains

| Source | Power |
|---|---|
| 1 occupant(s) × 80 W | 80 W |
| Electrical equipment | 200 W |
| **Total Q_int** | **280 W** |

### Simplifying assumptions

- The 3 vertical concrete walls are assumed to be a single element (same material, same boundary conditions).
- Heat transfer is homogeneous from one surface to another. 
- Every point of every wall has same parameters (no thermal bridges at edges or where materials change).
- The floor is adiabatic: zero heat flux at floor.
- Long-wave radiation is linearised around a mean indoor temperature T̄ = 20 °C = 293 K.
- The HVAC system is modelled as a proportional controller with gain Kp = 10⁴ W/K (approximation of a perfect controller).
- Steady-state calculation use monthly mean outdoor temperatures and solar irradiances.

---

## 3. Thermal & Radiative Properties

### Materials

| Material | λ [W/(m·K)] | ρ [kg/m³] | c_p [J/(kg·K)] | e [m] |
|---|---|---|---|---|
| Concrete (walls & ceiling) | 1.40 | 2300 | 880 | 0.20 |
| Glass (one pane) | 1.00 | 2500 | 750 | 0.006 |
| Air gap | 0.025 | 1.2 | 1000 | 0.016 |
| Indoor air | — | 1.2 | 1000 | — |

Material properties were  taken either from the "Toy House" example and for air from: https://www.engineeringtoolbox.com/air-properties-viscosity-conductivity-heat-capacity-d_1509.html
### Radiative properties

| Property | Symbol | Value |
|---|---|---|
| LW emissivity ,concrete | ε_w | 0.85 |
| LW emissivity , glass | ε_g | 0.90 |
| SW absorptivity , concrete | α_w | 0.25 |
| SW absorptivity , each glass pane | α_g | 0.38 |
| SW transmittance , each glass pane | τ_g | 0.30 |
| Stefan–Boltzmann constant | σ | 5.67 × 10⁻⁸ W/(m²·K⁴) |


### Surface convection coefficients

| Surface type | Location | Heat flow direction | h [W/(m²·K)] |
|---|---|---|---|
| Vertical wall | Indoor | Horizontal | 8.0 |
| Ceiling | Indoor | Downward | 8.0 |
| All surfaces | Outdoor | Wind-driven | 25.0 |

---
Values where taken from "Toy house" example.
## 4. Thermal Circuit

*Thermal circuit scheme*
[![Captura-de-pantalla-2026-04-26-220109.png](https://i.postimg.cc/brs9LV9M/Captura-de-pantalla-2026-04-26-220109.png)](https://postimg.cc/BPfFngyc)
Figure 1: Diagram with flows and conductances

[![Real-simulation.png](https://i.postimg.cc/JhXhMrYy/Real-simulation.png)](https://postimg.cc/zLJNkrr5)
Figure 2: 2D representation of modeled building

In this scheme we show the locations of the temperatures of the thermal circuit diagram in a 2D diagram of the building, where the main structures are: roof, wall (in 3D there are 3 walls) and the glass wall; with its correspondent temperature in each surface. Below we explain the flows and temperature nodes in detail.

### Node map (9 temperature unknowns)

The thermal network has 9 nodes (temperature unknowns θ₀ … θ₈) and 14 branches (heat flow rates q₀ … q₁₃).

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

- **T_o** is the outdoor air temperature
- **T_sp** is the HVAC setpoint temperature for winter/summer

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
  q₁₁   θ₈ → T_o    ventilation
  q₁₂   θ₈ → T_sp   HVAC controller 
  q₁₃   θ₁ → θ₇    LW radiation: inner wall surface ↔ inner glass
```


---

## 5. Values of Conductances



### 5.1 Conduction

Formula: **G_cd = (λ / e) · S**

| Branch | Element | λ [W/(m·K)] | e [m] | S [m²] | G [W/K] |
|---|---|---|---|---|---|
| G₁ | Concrete walls | 1.40 | 0.20 | 48 | **336.0** |
| G₄ | Concrete ceiling | 1.40 | 0.20 | 24 | **168.0** |
| G₇ | Outer glass pane | 1.00 | 0.006 | 18 | **3000.0** |
| G₈ | Air gap (effective) | 0.025 | 0.016 | 18 | **28.1** |
| G₉ | Inner glass pane | 1.00 | 0.006 | 18 | **3000.0** |


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

View factor:

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
| 0 | G₀ , outdoor conv., vert. walls | 1200.00 |
| 1 | G₁ , conduction, concrete walls | 336.00 |
| 2 | G₂ , indoor conv., vert. walls | 384.00 |
| 3 | G₃ , outdoor conv., ceiling | 600.00 |
| 4 | G₄ , conduction, concrete ceiling | 168.00 |
| 5 | G₅ , indoor conv., ceiling | 192.00 |
| 6 | G₆ , outdoor conv., outer glass | 450.00 |
| 7 | G₇ , conduction, outer glass pane | 3000.00 |
| 8 | G₈ , effective conv., air gap | 29.25 |
| 9 | G₉ , conduction, inner glass pane | 3000.00 |
| 10 | G₁₀ , indoor conv., inner glass | 144.00 |
| 11 | G₁₁ , ventilation / infiltration | 48.00 |
| 12 | G₁₂ , HVAC controller (Kp) | 10000.00 |
| 13 | G₁₃ , LW radiation, walls ↔ glass | 97.74 |

### 5.7 Equivalent overall conductances (series combination)

For each "element", the series of branches gives an overall conductance:

| Element | G_eq = 1/(1/G_out + 1/G_cd + ... + 1/G_in) | G_eq [W/(m²·K)] |
|---|---|---|
| 3 concrete walls (total) | 1/(1/1200 + 1/336 + 1/384) = 155.0 W/K | **3.22 W/(m²·K)** |
| Concrete ceiling | 1/(1/600 + 1/168 + 1/192.0) = 78.0 W/K | **3.24 W/(m²·K)** |
| Double-glazed window | 1/(1/450 + 1/3000 + 1/29.5 + 1/3000 + 1/144.0) = 22.9 W/K | **1.27 W/(m²·K)** |

---

## 6. Mathematical Model

### 6.1 Governing equations (DAE system)

The thermal network makes a system of Differential-Algebraic Equations:

```
  Branch equations:    q  = G · (-A·θ + b)
  Node energy balance: C·θ̇ = −Aᵀ·G·A·θ + Aᵀ·G·b + f
```
Which in steady state with Θ_dot = 0 simplifies significantly.

### 6.2 Incidence matrix A (14 × 9)

This matrix shows which nodes and branches are connected as well as the direction of heat flow.

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


### 6.3 Conductance matrix G (14 × 14 diagonal)

```
  G =   G = diag([1200.00, 336.00, 384.00, 600.00, 168.00, 192.00, 450.00, 3000.00, 29.25, 3000.00, 144.00, 48.00, 10000.00, 97.74])   W/K
```



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


### 6.5 Temperature source vector b (length 14)

Which branches are connected to temperature sources:

```
  b[0]  = T_o    (branch q₀:  θ₀ → T_o,   outdoor conv. walls)
  b[3]  = T_o    (branch q₃:  θ₂ → T_o,   outdoor conv. ceiling)
  b[6]  = T_o    (branch q₆:  θ₄ → T_o,   outdoor conv. outer glass)
  b[11] = T_o    (branch q₁₁: θ₈ → T_o,   ventilation)
  b[12] = T_sp   (branch q₁₂: θ₈ → T_sp,  HVAC setpoint)
  all others = 0
```

### 6.6 Heat flow source vector f (length 9)

Heat flow by radiation in which branches:

```
  f[0] = α_w · E_south · S_cv       (solar on outer concrete wall surfaces)
  f[2] = α_w · E_horiz · S_cl       (solar on outer ceiling surface)
  f[4] = α_g · E_south · S_w        (solar absorbed in outer glass pane)
  f[8] = τ_dg · E_south · S_w + Q_int   (solar transmitted + internal gains)
  all others = 0
```

## 7. Steady-State Thermal Load

### Most unfavorable winter conditions

**T_o = 0 °C, T_sp = 20 °C, E_south = 300 W/m², E_horiz = 100 W/m²**
(Arbitrary selection of parameters)
#### Node temperatures

| Node | Description | Temperature (°C) |
|---|---|---|
| θ₀ | Outer surf. concrete walls |  5.23|
| θ₁ | Inner surf. concrete walls | 13.21|
| θ₂ | Outer surf. ceiling | 3.41|
| θ₃ | Inner surf. ceiling | 12.01|
| θ₄ | Outer glass , outdoor face | 5.23|
| θ₅ | Outer glass , indoor face | 5.33 |
| θ₆ | Inner glass , outdoor face | 15.63 |
| θ₇ | Inner glass , indoor face | 15.73 |
| **θ₈** | **Indoor air** | **19.54** |

#### Branch heat flows

| Branch | Description | q [W] |
|---|---|---|
| q₀ | Outdoor conv., vert. walls |  6279.2 |
| q₁ | Conduction, concrete walls |  2679.2 |
| q₂ | Indoor conv., vert. walls |   2432.2 |
| q₃ | Outdoor conv., ceiling |  2045.4|
| q₄ | Conduction, ceiling | 1445.4 |
| q₅ | Indoor conv., ceiling | 1445.4 |
| q₆ | Outdoor conv., outer glass |  2353.3 |
| q₇ | Conduction, outer glass | 301.3 |
| q₈ | Effective conv., air gap | 301.3 |
| q₉ | Conduction, inner glass | 301.3 |
| q₁₀ | Indoor conv., inner glass |  548.3 |
| q₁₁ | Ventilation (θ₈ → T_o) | 937.9 |
| **q₁₂** | **HVAC (heating load)** | **-4597.8** |
| q₁₃ | LW radiation, walls ↔ glass | −246.9 |

---

### Most unfavorable summer conditions

**T_o = +35 °C, T_sp = 26 °C, E_south = 700 W/m², E_horiz = 800 W/m²**

#### Node temperatures

| Node | Description | Temperature (°C) |
|---|---|---|
| θ₀ | Outer surf. concrete walls | 39.90 |
| θ₁ | Inner surf. concrete walls | 32.5 |
| θ₂ | Outer surf. ceiling | 40.8 |
| θ₃ | Inner surf. ceiling | 33.26 |
| θ₄ | Outer glass , outdoor face | 44.74 |
| θ₅ | Outer glass , indoor face | 44.61 |
| θ₆ | Inner glass , outdoor face | 30.81 |
| θ₇ | Inner glass , indoor face | 30.68 |
| **θ₈** | **Indoor air** | **26.6** |

#### Branch heat flows

| Branch | Description | q [W] |
|---|---|---|
| q₀ | Outdoor conv., vert. walls |  5923.7|
| q₁ | Conduction, concrete walls |  -2476.3 |
| q₂ | Indoor conv., vert. walls |    -2292.0|
| q₃ | Outdoor conv., ceiling |   3521.3|
| q₄ | Conduction, ceiling |  -1278.7|
| q₅ | Indoor conv., ceiling |  -1278.7|
| q₆ | Outdoor conv., outer glass |   4384.5|
| q₇ | Conduction, outer glass | -403.5|
| q₈ | Effective conv., air gap | -403.5 |
| q₉ | Conduction, inner glass | -403.5|
| q₁₀ | Indoor conv., inner glass |   -587.9|
| q₁₁ | Ventilation (θ₈ → T_o) | -403.3|
| **q₁₂** | **HVAC (cooling load)** | **5975.9** |
| q₁₃ | LW radiation, walls ↔ glass | 184.4|

## 8. Yearly Energy Consumption

### Climate data: Grenoble, France (From: https://www.holiday-weather.com/grenoble/averages/) and ( pvlib API)

| Month | T_out (°C) | E_south (W/m²) | E_horiz (W/m²) |
|---|---|---|---|
| Jan | 2 | 58 | 26 |
| Feb | 4 | 71 | 46 |
| Mar | 7 | 82 | 77 |
| Apr | 10 | 80 | 111 |
| May | 15 | 72 | 138 |
| Jun | 17 | 73 | 164 |
| Jul | 20 | 88 | 187 |
| Aug | 20| 105 | 168 |
| Sep | 17 | 96 | 104 |
| Oct | 12 | 82 | 60 |
| Nov | 6 | 59 | 30 |
| Dec | 3 | 49 | 19 |

### Monthly HVAC energy

Energy calculation: `E_month = |Q_HVAC| × hours_in_month / 1000` [kWh].

Logic:
- If Q_HVAC < 0 (heating needed): `E_heat = |Q_HVAC| × hours / 1000`
- If Q_HVAC ≥ 0 with T_sp=20°C: re-solve with T_sp=26°C; if still Q_HVAC > 0: `E_cool = Q_HVAC × hours / 1000`

| Month | T_out (°C) | Q_HVAC (W) | E_heat (kWh) | E_cool (kWh) |
|---|---|---|---|---|
| Jan | 2.0 | −4888.9 | 3637.4 | 0.0 |
| Feb | 4.0 | −4231.3 | 2843.4 | 0.0 |
| Mar | 7.0 | −3273.3 | 2435.4 | 0.0 |
| Apr | 10.0 | −2357.9 | 1697.7 | 0.0 |
| May | 15.0 | −870.6 | 647.7 | 0.0 |
| Jun | 17.0 | −249.7 | 179.8 | 0.0 |
| Jul | 20.0 | +715.9 | 0.0 | 0.0 |
| Aug | 20.0 | +760.1 | 0.0 | 0.0 |
| Sep | 17.0 | −215.9 | 155.5 | 0.0 |
| Oct | 12.0 | −1791.8 | 1333.1 | 0.0 |
| Nov | 6.0 | −3686.9 | 2654.6 | 0.0 |
| Dec | 3.0 | −4626.3 | 3442.0 | 0.0 |
| **TOTAL** | | | **19026.5** | **0.0** |

| Metric | Value |
|---|---|
| Total heating energy | **19 027 kWh/year** |
| Total cooling energy | **0 kWh/year** |
| **Total HVAC energy** | **19 027 kWh/year** |
| Floor area | 24 m² |
| **Specific consumption** | **793 kWh/(m²·year)** |

 Observing this data it seems that: July and August are free-running months the room floats naturally
between the heating setpoint (20 °C) and cooling setpoint (26 °C) with no HVAC.
required. With the current building design and ambient conditions used to model the room never needs cooling under average conditions.

## 9. Summary of previous findings

### Equivalent conductances

| Element | G_eq (U - value) [W/(m²·K)] | 
|---|---|
| 3 concrete walls (uninsulated) | **3.22** | 
| Concrete ceiling (uninsulated) | **3.24** | 
| Double-glazed window | **1.27** |

We observe that the double-glazed window actually outperforms the uninsulated concrete walls. Adding insulation to the concrete walls would significantly reduce heating demand.

### Thermal worst-case loads

| Condition | Load | Specific load |
|---|---|---|
| Winter peak (T_o = 0 °C) | **4597.8 W heating** | **192 W/m²** |
| Summer peak (T_o = 35 °C) | **5975.9 W cooling** | **249 W/m²** |

Although there is no insulation, the cooling load is larger than the heating load. This is likely due to the 18 m² south-facing double-glazed window transmitting ~9090 W of solar in summer (τ_dg × 700 W/m² × 18 m²).


---

*Stefanos Theodoros Aristodemou, Irene Benito Gómez, Alberto Navarro López,Pedro Guilherme Cerqueira Sampaio (26/4/2026)*
*Report for Assignment 1 - Smart Cities - Building Modeling*
