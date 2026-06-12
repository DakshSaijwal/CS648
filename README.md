# CS648 Mini Project — Phase Transition in Erdős–Rényi Random Graphs

> **Course:** CS648 — Randomized Algorithms  
> **Group:** 230305  
> **Forked from:** [chaitanyavb-502/CS648-mini-project](https://github.com/chaitanyavb-502/CS648-mini-project)

---

## Overview

This project empirically studies the **phase transition phenomenon** in Erdős–Rényi random graphs G(n, m). The central question: as edges are added one-by-one to a graph of n vertices, at what point does a **giant connected component** suddenly emerge?

The theoretical answer is sharp — the transition happens at the critical threshold **c = 2m/n = 1**, i.e., when the average degree crosses 1. This project validates that theory through simulation and visualization.

---

## Background

In an Erdős–Rényi random graph G(n, p) or G(n, m):

- **Subcritical phase** (c < 1): all connected components have size O(log n). No giant component exists.
- **Critical threshold** (c = 1): a giant component of size Θ(n^(2/3)) begins to form.
- **Supercritical phase** (c > 1): a unique giant component of size ρ(c) · n dominates, where ρ(c) is the survival probability of a Poisson branching process satisfying ρ = 1 − e^(−c·ρ).

---

## Repository Structure

```
CS648/
├── a.cpp                          # Main simulation: DSU-based edge addition, outputs (edge, max_component_size)
├── b.cpp                          # Variant simulation
├── c.cpp                          # Variant simulation
├── d.cpp                          # Variant simulation
├── data.txt                       # Output data from C++ simulation (edge_count, largest_component)
│
├── plot.py                        # Giant component: theory vs simulation plot
├── plotb.py                       # Additional plotting script
├── plotc.py                       # Additional plotting script
├── animation.py                   # Animated visualization of graph phase transition
├── critical_window.py             # Plots behavior near the critical window (c ≈ 1)
├── subcritical.py                 # Subcritical phase analysis (p = (1−ε)/n)
├── supercritical.py               # Supercritical phase analysis (p = (1+ε)/n)
│
├── plot2.png                      # Plot output
├── plot_1000.png                  # Simulation with n=1000
├── plot_1000_with_theoretical.png # Simulation vs theoretical curve
├── plot_10_iterations.png         # 10-iteration average plot
├── plot_num_components.png        # Number of components over time
├── critical_window.png            # Critical window visualization
├── subcritical1/2/3.png           # Subcritical phase plots
├── supercritical1/2/3.png         # Supercritical phase plots
├── graph_phase_transition.mp4     # Animation of the phase transition
│
├── Group_230305_report.pdf        # Full project report
├── presentation_ppt_230305.pptx   # Presentation slides
├── CS648_PS_session.pdf           # Problem statement / session notes
└── simple_proof_paper.pdf         # Reference proof paper
```

---

## How It Works

### Step 1 — Simulate with C++ (`a.cpp`)

The C++ program uses a **Disjoint Set Union (DSU)** data structure to efficiently track connected components as random edges are inserted one at a time into a graph of N = 1000 vertices. After each edge addition, it outputs the current edge count and the size of the largest component.

```bash
g++ -O2 -o sim a.cpp
./sim > data.txt
```

### Step 2 — Plot with Python

**Giant component vs theory (`plot.py`)**
```bash
python plot.py
```
Reads `data.txt`, computes `c = 2m/n`, and plots the normalized largest component size `L1/n` against the theoretical prediction ρ(c).

**Subcritical phase (`subcritical.py`)**
```bash
python subcritical.py
```
Runs 200 independent G(n, p) simulations with `p = (1−ε)/n` using NetworkX, and verifies that the largest component stays within the O(log n) bound.

**Supercritical phase (`supercritical.py`)**
```bash
python supercritical.py
```
Analogous analysis for `p = (1+ε)/n`, showing the emergence of a linear-sized giant component.

**Critical window (`critical_window.py`)**
```bash
python critical_window.py
```
Zooms into the behavior around c = 1.

**Animation (`animation.py`)**
```bash
python animation.py
```
Produces an animated visualization of how components evolve as edges are added.

---

## Dependencies

**C++**
- C++17 or later
- Standard library only (`<bits/stdc++.h>`)

**Python**
```bash
pip install matplotlib numpy networkx
```

---

## Key Results

- The simulation closely matches the theoretical curve ρ(c) = 1 − e^(−c·ρ(c)) for the supercritical regime.
- In the subcritical phase, the largest component consistently stays below the 7 ln(n) / ε² bound.
- The phase transition at c = 1 is visually sharp even for n = 1000.

See `Group_230305_report.pdf` for the full analysis and proofs.

---

## References

- Erdős, P. & Rényi, A. (1960). *On the evolution of random graphs.*
- Bollobás, B. (2001). *Random Graphs.* Cambridge University Press.
- `simple_proof_paper.pdf` (included in repo)
