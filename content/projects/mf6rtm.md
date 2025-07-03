---
title: "mf6rtm"
author: Pablo Ortega
date: 2025-06-01
description: "A python package for reactive transport modeling via the MODFLOW 6 and PhreeqcRM APIs"
image: "/mrbeaker.png"
draft: false


---

<!-- [View on GitHub](https://github.com/p-ortega/mf6rtm) -->
[![GitHub Repo](https://img.shields.io/badge/GitHub-mf6rtm-blue?logo=github)](https://github.com/p-ortega/mf6rtm)
[![Last Commit](https://img.shields.io/github/last-commit/p-ortega/mf6rtm)](https://github.com/p-ortega/mf6rtm/commits/main)
[![GitHub Stars](https://img.shields.io/github/stars/p-ortega/mf6rtm?style=social)](https://github.com/p-ortega/mf6rtm/stargazers)

---
# mf6rtm: Reactive Transport Model with MODFLOW 6 and PHREEQCRM
---
## Integration Overview

mf6rtm couples the modular capabilities of MODFLOW 6 (MF6) with the geochemical modeling power of PHREEQCRM (PHREEQC-3). The workflow is as follows:

- **Initialization**: PHREEQCRM and arrays are initialized using a `phinp.dat` file.
- **Model Setup**: Grid dimensions and transported components are derived from the MF6 `dis` or `disv` object and PHREEQCRM respectively. MF6 model files are written for each component.
- **Transport Loop**: At each time step, MF6 solves the transport system. Convergence iterations and failures are tracked.
- **Reaction Calculations**: If enabled, concentrations are passed to PHREEQCRM for reaction calculations, and results are fed back into MF6.
- **Output**: MF6 handles UCN outputs. Custom outputs can also be saved as CSV.

### Current Capabilities

- Structured grids (`dis` object) and unstructured grids through discretization by vertices (`disv`)
- Single simulation setup (flow + transport)
- Supported PHREEQC reactions:
  - Solution
  - Equilibrium phases
  - Cation exchange
  - Surface complexation
  - Kinetics

---

## Benchmarks

### Example 1: Engesgaard & Kipp (1992)

A 1D domain with calcite and dolomite equilibrium reacting to a chemical change in inflow. Simulated using the WEL and SSM packages. Compared against PHT3D.

![Engesgaard 1992 benchmark](/ex1.png)

---

### Example 2: Walter et al. (1994) — 1D Redox Problem

Simulates acidic mine tailings leaching into a carbonate aquifer. Includes 17 aqueous components, 6 minerals. mf6rtm uses larger time steps than PHT3D for similar accuracy.

![Walter 1994 1D redox](/ex2_10xtsteps.png)

---

### Example 3: Walter et al. (1994) — 2D Redox Problem

Extension of Example 2 to 2D. Uses CHD and CNC in MF6 for compatibility with the original PHT3D setup.

![2D benchmark](/ex3.png)  
![2D Ca cross-section](/ex3_Ca_2000.png)

---

### Example 4: Cation Exchange — Na/K/NO₃ flushed with CaCl₂

Simulates ion exchange in a flow-through column. Benchmarked against PHT3D and PHREEQC-3. mf6rtm uses double the time step due to numerical dispersion.

![Cation exchange](/ex4.png)

---

### Example 5: Oxidation of Marine Pyritic Sediment (Appelo et al. 1998)

Includes pyrite oxidation, calcite dissolution, CO₂ sorption, ion exchange, and organic matter oxidation. Simulates three experimental phases involving MgCl₂ and H₂O₂ solutions.

![Oxidation experiment](/ex5.png)

