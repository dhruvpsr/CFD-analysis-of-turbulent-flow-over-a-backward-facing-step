# Turbulent Flow over a Backward-Facing Step: Validation and Parametric Study

**Authors:** Dhruv Pratap Singh (B22ME017) & Ripunj Gupta (B22ME054)  
**Institution:** Department of Mechanical Engineering, IIT Jodhpur  
**Course:** MEL7400: Applying CFD Procedures

---

## 📌 Project Overview

This repository contains the OpenFOAM case files and analysis reports for a Computational Fluid Dynamics (CFD) study of turbulent flow over a Backward-Facing Step (BFS). 

The BFS is a canonical benchmark for separated flows, featuring shear layer detachment, recirculation, and reattachment. This project validates a steady RANS workflow against experimental data (Driver & Seegmiller, 1985) and performs extensive parametric studies to understand the flow physics and numerical sensitivity.

### Key Highlights
- **Solver:** `simpleFoam` (Steady-state, Incompressible)
- **Turbulence Model:** $k-\omega$ SST (Baseline) vs. Standard $k-\epsilon$
- **Reynolds Number:** $Re_H = 36,000$ (Baseline) with variations from 20k to 50k.
- **Validation Error:** < 0.2% deviation in reattachment length ($X_r/H$) compared to experiments.

---

## 📊 Visualizations


![Velocity Field](https://github.com/dhruvpsr/CFD-analysis-of-turbulent-flow-over-a-backward-facing-step/blob/main/Vortex_U_streamlines.png)


![Validation Plot](https://github.com/dhruvpsr/CFD-analysis-of-turbulent-flow-over-a-backward-facing-step/blob/main/ab.jpeg)

---

## ⚙️ Methodology

### 1. Computational Domain
- **Geometry:** Step height $H = 0.0127$ m. Expansion ratio 1:1.125.
- **Mesh:** Structured hexahedral grid generated using `blockMesh` with exponential grading to resolve the boundary layer ($30 < y^+ < 300$).

### 2. Physics & Solver
- **Governing Equations:** RANS (Reynolds-Averaged Navier-Stokes).
- **Schemes:** Second-order consistent (`linearUpwind` for convection).
- **Boundary Conditions:** - Inlet: Fixed velocity ($U_{ref} = 44.2$ m/s), $I=0.06\%$.
  - Walls: No-slip with wall functions.

---

## 🧪 Parametric Studies & Results

We conducted four specific parametric studies. The raw data and comparisons are available in the `results/` directory.

### 1. Reynolds Number Sensitivity ($Re_H = 20k - 50k$)
- **Observation:** Reattachment length ($X_r/H$) increases weakly from 5.75 to 6.22 as $Re$ increases.
- **Physics:** Higher inertia in the separated shear layer delays the momentum transfer required to curve the flow back to the wall.

### 2. Turbulence Model Comparison
- **Finding:** Standard $k-\epsilon$ is **unsuitable** for this flow.
- **Reasoning:** It over-produces Turbulent Kinetic Energy (TKE) in the stagnation region, leading to excessive mixing and premature reattachment (9% under-prediction error).
- **Conclusion:** $k-\omega$ SST is required for accurate separation prediction.

### 3. Geometric Scale Invariance
- **Test:** Simulated Half Scale, Baseline, and Large Scale geometries while maintaining constant $Re = 36,000$.
- **Result:** Pressure coefficient ($C_p$) profiles collapsed onto a single curve, confirming that flow physics depend on non-dimensional parameters ($Re$), not physical size.

### 4. Mesh Strategy & Engineering Recommendation
We tested Coarse (20k), Medium (80k), and Fine (180k) meshes.
- **Observation:** The Coarse mesh showed visible deviation in the pressure recovery region ($x/H > 6$) due to gradient resolution.
- **Critical Insight:** Despite this deviation, the Coarse mesh predicted the **Reattachment Length ($X_r/H$)** with **< 2% error** compared to the Fine mesh.
- **Recommendation:** For rapid engineering design iterations, the **Coarse Mesh** is highly recommended. It offers a **10-20x speedup** while accurately capturing the primary separation metric.

---

## 📂 Repository Structure

```bash
├── 0/                   # Initial Boundary Conditions (U, p, k, omega, nut)
├── constant/            # Physical properties and Mesh (polyMesh)
├── system/              # Solver settings (controlDict, fvSchemes, fvSolution)
├── results/             # Post-processing data, graphs, and logs
├── reports/             # Final PDF Report and Presentation
└── README.md            # Project documentation

## 🚀 How to Run

### **Clone the repository**
```bash
git clone https://github.com/yourusername/bfs-cfd-openfoam.git
cd bfs-cfd-openfoam
```

### **Generate Mesh**
```bash
blockMesh
```

### **Run Solver**
```bash
simpleFoam
```

### **Post-Processing**
```bash
paraFoam
```

---

## 📚 References

- Driver, D.M., & Seegmiller, H.L. (1985). *Features of a reattaching turbulent shear layer in divergent channel flow.* **AIAA Journal**.  
- NASA Langley Research Center. *Turbulence Modeling Resource – 2D Backward Facing Step.*  
- Menter, F.R. (1994). *Two-equation eddy-viscosity turbulence models for engineering applications.* **AIAA Journal**.
