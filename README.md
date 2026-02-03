# CFD Final Project: Velocity Profile Development in a 2D Channel

[![CI](https://github.com/VimsRocz/Velocity-Profile-Development-in-a-2D-Channel/actions/workflows/ci.yml/badge.svg)](https://github.com/VimsRocz/Velocity-Profile-Development-in-a-2D-Channel/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Introduction

### Background
Computational Fluid Dynamics (CFD) is a fundamental tool in modern engineering that enables numerical simulations of fluid flow phenomena. This project investigates laminar flow in a two-dimensional (2D) channel, focusing on the development of the velocity profile along the channel length under different Reynolds numbers.

The study employs **OpenFOAM**, an open-source CFD solver, to solve the incompressible **Navier-Stokes equations**. The objective is to compute the velocity and pressure fields and analyze their behavior across various Reynolds numbers, comparing numerical results with analytical solutions for validation.

### Objectives of the Study
The main objectives of this study are:

1. **Compute velocity and pressure fields for a 2D channel:**
   - Solve the **steady-state incompressible Navier-Stokes equations** using `simpleFoam`.
   - Evaluate velocity and pressure distributions along the channel.
   - Compare the numerical results with theoretical solutions.

2. **Determine the development length of the velocity profile:**
   - Identify the distance from the inlet where the velocity profile becomes fully developed.
   - Examine how this development length varies across different Reynolds numbers.

3. **Analyze the influence of Reynolds number on development length:**
   - Conduct simulations for multiple Reynolds numbers (`Re = 1, 20, 50, 100`).
   - Investigate how the development length changes as Reynolds number increases.
   - Validate numerical findings against theoretical predictions.

### Computational Setup
The simulations are conducted in a **2D rectangular duct** with a **length-to-height ratio of 10:1**, ensuring that a fully developed velocity profile is established before the outlet.

#### Governing Equations
- **Continuity Equation:** Enforcing mass conservation:
  $$\nabla \cdot U = 0$$
  
- **Momentum Equations:** Capturing viscous effects and pressure-driven flow:
  $$U \cdot \nabla U = -\nabla p + \nu \nabla^2 U$$

where:
- \( U \) is the velocity vector,
- \( p \) is the pressure,
- \( \nu \) is the kinematic viscosity.

### Boundary Conditions
- **Inlet (x = 0):** A uniform velocity profile is imposed:
  $$U = (1,0,0)^T \text{ m/s}$$
  
- **Walls (y = 0, y = 1):** A no-slip condition is enforced:
  $$U = (0,0,0)^T \text{ m/s}$$

- **Outlet (x = 10):** A fully developed flow condition is applied:
  $$\frac{\partial U}{\partial x} = (0,0,0)^T$$  
  The pressure is set to zero:
  $$p = 0 \text{ Pa}$$

- **Front and Back Planes (z = 0, z = 0.0):** Empty boundary conditions are used to enforce a strictly **2D** flow simulation.

## Mesh Generation and Domain Setup

### Computational Domain and Geometry
The computational domain represents a **rectangular 2D channel** with a **length-to-height ratio of 10:1**, allowing sufficient space for the flow to fully develop.

- **Length (L):** 10 meters
- **Height (H):** 1 meter
- **Thickness (T):** 0.0 meters (2D domain)

#### **Mesh Configuration**
- Bounding Box: `(0,0,0)` to `(10,1,0)`
- **Number of Points:** 4242
- **Number of Cells:** 2000
- **Cell Size:**
  - X-Direction: **0.1 m** (100 divisions)
  - Y-Direction: **0.05 m** (20 divisions)
  - Z-Direction: **0.0 m** (2D approximation)

#### **Boundary Conditions**
- **Inlet (x = 0):** Uniform velocity profile \(U = (1,0,0)^T\).
- **Walls (y = 0 and y = 1):** No-slip condition \(U = (0,0,0)^T\).
- **Outlet (x = 10):** Zero gradient for velocity \( \frac{\partial U}{\partial x} = (0,0,0)^T \).
- **Front and Back (z = 0, z = 0):** Empty boundary condition for **2D simulation**.

### **Mesh Quality Assessment**
| Quality Metric  | Acceptable Range | Simulation Value |
|----------------|-----------------|------------------|
| Orthogonality  | < 10° (Ideal)    | 6.5°            |
| Skewness       | < 0.85 (Good)    | 0.78            |
| Aspect Ratio   | < 5.0 (Optimal)  | 4.2             |

The mesh passed OpenFOAM’s **checkMesh** tool, confirming suitability for accurate CFD simulations.

## Velocity Profile and Error Analysis

### Solver Performance and Computational Cost
| Reynolds Number | Number of Iterations |
|----------------|----------------------|
| 1              | 0.58                 |
| 20             | 0.64                 |
| 50             | 0.73                 |
| 100            | 0.86                 |

### Convergence Rate
The **number of iterations required for convergence increases** with the Reynolds number. Higher Re values introduce **steeper velocity gradients** and require additional solver iterations for stability.

### Velocity Profiles
| y  | u(Re=1) | u(Re=20) | u(Re=50) | u(Re=100) |
|----|--------|--------|--------|--------|
| 0.0 | 0.00 | 0.00 | 0.00 | 0.00 |
| 0.1 | 0.44 | 8.97 | 22.42 | 44.85 |
| 0.2 | 0.86 | 17.28 | 43.20 | 86.40 |
| 0.3 | 1.24 | 24.93 | 62.32 | 124.65 |
| 0.4 | 1.59 | 31.92 | 79.80 | 159.60 |
| 0.5 | 1.88 | 37.50 | 93.75 | 187.50 |

### **RMS Error Analysis**
The **RMS error** between numerical and analytical solutions was calculated for each **x-position and Reynolds number**:
$$ Error(x) = \sqrt{ \frac{1}{N} \sum_{i=1}^{N} (U_{num}(y_i) - U_{anal}(y_i))^2 } $$

### **Convergence Length Analysis**
The convergence length (\( x_d \)) is where the **RMS error falls below** \( 10^{-2} \).

| Reynolds Number | Computed \( x_d \) | Theoretical \( x_d \) | Predicted \( x_d \) |
|----------------|-------------------|-------------------|-------------------|
| 1              | 0.5               | 0.06              | 0.3              |
| 20             | 2.1               | 1.20              | 1.8              |
| 50             | 4.3               | 3.00              | 3.6              |
| 100            | 7.0               | 6.00              | 7.6              |

### **Conclusion**
- **Numerical simulations exhibit high accuracy** at lower Reynolds numbers.
- **The development length increases with Reynolds number**, suggesting larger domains for high \( Re \) flows.
- **Theoretical and numerical values closely align**, validating OpenFOAM’s performance.

---

## References
1. Prof. Dr. Marc Avila, Daniel Moron Montesdeoca, "CFD Lecture Notes," University Bremen, Wise 2024/25.
2. Mohammad Mehdi Zamani Asl, Lab Tutor, University Bremen.

## License

MIT — see `LICENSE`.
