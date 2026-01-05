# memristor-ga-cosim
A physics-informed COMSOL–MATLAB co-simulation framework for optimizing oxide-based memristor (RRAM) devices using a genetic algorithm. The workflow couples a COMSOL electrothermal device model with MATLAB GA to systematically tune 13 design and operating parameters, targeting reduced forming voltage and improved memory contrast (HRS/LRS). 

# Memristor COMSOL–MATLAB GA Co-Optimization

## 👥 Authors
- Armin Gooran-Shoorakchaly  
- Sarah S. Sharif  
- Yaser M. Banad* (Corresponding author: bana@ou.edu)  
School of Electrical and Computer Engineering, University of Oklahoma, Norman, OK, USA

## 🧾 Abstract
This repository provides a physics-informed, multi-objective simulation–optimization workflow for oxide-based memristive (RRAM) devices. The framework couples a validated COMSOL Multiphysics electrothermal device model with a MATLAB genetic algorithm (GA) to jointly minimize forming voltage and improve memory contrast (HRS/LRS) by exploring a high-dimensional design space of geometrical, electrical, thermal, and operational parameters.

## 🧠 Project Overview
- **Device model:** Physics-based COMSOL model of a TaOx-based memristor (RRAM), developed and tested in **COMSOL Multiphysics 6.3**.  
- **Optimization engine:** MATLAB-based multi-objective GA that iteratively evaluates candidate device parameter sets through the COMSOL–MATLAB pipeline.  
- **Primary objectives:**  
  1. Minimize forming voltage (**Vf**)  
  2. Maximize memory contrast (**HRS/LRS**)  

## 🧰 Requirements
- **COMSOL Multiphysics 6.3** (with the appropriate modules used by the memristor model)
- **MATLAB** with:
  - Global Optimization Toolbox (for `ga`)
  - COMSOL LiveLink for MATLAB (or an equivalent MATLAB–COMSOL interface)

> Note: COMSOL Multiphysics is proprietary software and requires a valid license.

## 📁 Repository Contents
- `Memristor_COMLAB.mph`  
  COMSOL device model (loaded and parameterized during optimization).
- `Dataset.initial.m`  
  Reference simulation dataset (time, voltage, current) and post-processing routines (e.g., read voltage selection for HRS/LRS).
- `Dataset.m`  
  Defines and stores reference metrics and initializes the optimization design space (13 parameters).
- `Cmodel.m`  
  MATLAB–COMSOL interface: loads the COMSOL model, substitutes candidate parameters, and evaluates objective functions.
- `Try.m`  
  Robust solver wrapper implementing fallback solution strategies when a candidate causes convergence issues.
- `Initial_run.m`  
  Generates a valid initial population (e.g., **200** individuals) and establishes objective behavior before full optimization.
- `Fitfun.m` and `fitnesscheck.m`  
  Fitness evaluation and consistency checks to enforce simultaneous minimization of objectives.
- `Final_Run.m`  
  Launches the full GA loop from the saved Generation 0 dataset (e.g., **20** generations).

## 🧬 Optimization Workflow
1. **Run `Dataset.initial.m`** to load reference I–V–t data and compute reference metrics (e.g., forming voltage and HRS/LRS at the specified read voltage).
2. **Run `Dataset.m`** to define the 13 optimization parameters, discretization, and initial dataset.
3. **Execute `Initial_run.m`** to generate the initial random population and validate end-to-end evaluation.
4. **Execute `Final_Run.m`** to run the GA for the specified number of generations.
5. Inspect saved outputs for the final Pareto-optimal candidates and their normalized objective values.

## 🚀 Quick Start (Typical Order)
```matlab
run('Dataset.initial.m');
run('Dataset.m');
run('Initial_run.m');
run('Final_Run.m');
