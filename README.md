# Waveform_optimization_and_design
* About this project:
Radars emit pulses, to which phase modulation is applied. The pulse is divided into N chips with quantized phase each. The instant at which a target is detected is represented by a peak (called main lobe) at the instant that distinguishes its distance. In practice, around that main lobe appear small undesired lobes called side lobes. The Side Lobes appear due to the mathematical process used to design de recognition system through the correlation between the emitted and the received pulses. This correlation is condensed into a quantity called Integrated Side-Lobes Ratio (ISLR), which tells us about how intense the noise caused by these correlations, which in turn originates from the choice of the phases of each chip. Therefore, the goal is to find the sequence with the lowest ISLR, that is, the phase sequence for each chip that minimizes that unwanted noise that directly affects image generation by contaminating neighboring pixels that are made up of their respective main lobes.

* About this repository:
This repository shows the quantum optimization algorithm that we implemented to find the best combinations of radar pulse chip phases, aiming to minimize sidelobes and enhance image resolution in SAR systems. Comparing performance with classical genetic algorithms and Barker sequences.
This repository is divided into two notebooks: QUBO_HUBO_tests.ipynb to make the setups, establish the objective function, and run the baselines (genetic algorithm and Barker) and Classiq.ipynb for posing the problem in Pyomo, setting up variables, the objective function, constraints, and making the connection with Classiq, which takes the "classic" setup and creates the algorithm capable of solving the problem, providing results consistent with the baseline.

*How to run the repository:
The QUBO_HUBO_tests.ipynb notebook doesn't require anything more than importing certain Python libraries and modules like sympy or itertools. But for the Classiq.ipynb the first requirement is to install "pip install pyomo classiq" and then complete the authentication process for Classiq "import classiq ->
classiq.authenticate()".

*Main results:

