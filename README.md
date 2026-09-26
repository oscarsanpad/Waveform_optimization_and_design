# Waveform_optimization_and_design

Quantum optimization of radar waveform phase codes (ISLR minimization).
Quantum Innovation Summit 2026 — Radar Signal Optimization track.
Thales use-case: *Waveform Optimization for SAR Waveform Design*.

## About this project

Radars emit pulses to which phase modulation is applied: the pulse is divided into N chips, each with a quantized phase. A detected target shows up as a peak (the **main lobe**) at the instant that identifies its distance. In practice, small undesired **side lobes** appear around that main lobe. They arise from the correlation between the emitted and received pulses, the very mechanism used to recognize the echo. Their combined energy is condensed into the **Integrated Side-Lobes Ratio (ISLR)**, which depends entirely on the choice of phases for each chip. The goal is therefore to find the phase sequence with the lowest ISLR, since that unwanted energy degrades image generation by contaminating neighbouring pixels.

## About this repository

This repository contains the quantum optimization algorithm we implemented to find the best combinations of radar pulse chip phases, minimizing side lobes to improve image resolution, and compares its performance against classical baselines (a genetic algorithm and the Barker sequences).

- **`QUBO_HUBO_tests.ipynb`** — problem setup, objective function, QUBO/HUBO construction, and the classical baselines (genetic algorithm and Barker validation).
- **`Classiq.ipynb`** — the problem posed in Pyomo (variables, objective, constraints), its connection to Classiq, which turns that classical setup into a QAOA circuit, and the metrics used to compare against the baselines.

## How to run

`QUBO_HUBO_tests.ipynb` only needs standard Python libraries (`sympy`, `itertools`, `numpy`).

`Classiq.ipynb` additionally requires:

```bash
pip install pyomo classiq
```

followed by authentication:

```python
import classiq
classiq.authenticate()
```

## Main results

**Validation.** The formulation reproduces the known optimum for N = 5, 7, 11 (ISLR = 2, 3, 5), matching the Barker codes given in the use-case up to negation and reversal symmetries.

**QAOA** (Classiq simulator, CVaR α = 0.7, 10,000 shots, median of 3 seeds):

| N  | Layers | P(optimal) | Amplification vs random | Success | Time |
|---:|-------:|-----------:|------------------------:|:-------:|-----:|
| 5  | 3      | 76.4%      | 6.1×                    | 3/3     | 13 s |
| 5  | 7      | 73.5%      | 5.9×                    | 3/3     | 22 s |
| 7  | 3      | 11.7%      | 3.8×                    | 3/3     | 21 s |
| 7  | 7      | 46.6%      | **14.9×**               | 3/3     | 31 s |
| 11 | 3      | 0.17%      | 0.9×                    | 3/3     | 51 s |
| 11 | 7      | 0.26%      | 1.3×                    | 3/3     | 92 s |
| 15 | 7      | 0.030%     | 1.2×                    | 2/2     | 855 s |

**Classical baseline** (genetic algorithm, 5 seeds): finds the optimum 5/5 for N ≤ 11 in under 0.5 s; 3/5 for N = 15 in 0.64 s.

**Circuit resources** (7 QAOA layers):

| N  | Qubits | Depth  | CX gates |
|---:|-------:|-------:|---------:|
| 5  | 5      | 106    | 84       |
| 7  | 7      | 309    | 224      |
| 11 | 11     | 1,576  | 1,211    |
| 15 | 15     | 5,300  | 4,032    |
| 20 | 20     | 11,411 | 11,564   |

**Takeaways.** The required circuit depth grows with N (at N = 7, going from 3 to 7 layers takes amplification from 3.8× to 14.9×); extra layers hurt small instances (N = 5); the signal fades from N = 11 onward. No quantum advantage is claimed — on a simulator the genetic baseline is three orders of magnitude faster. The contribution is a validated formulation together with its resource-scaling analysis.

