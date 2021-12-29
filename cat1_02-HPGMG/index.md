# ExCALIBUR Exascale Challenge Problem: HPGMG-FEM

## Basic information

 - Design and development working group name: GenX: Code Generation for Extreme Scale PDE Discretisation and Composable Outer Loops
 - Challenge Problem Category: Category 1

## Brief description of exascale use case (0.5-1 page)

Geometric Multigrid (GMG) is a widely used solver for continuum problems. When performing continuum mechanics simulations the expectation is that, for a suitably sized problem, the majority of execution time is spent numerically solving the system of equations that arise from the discretisation of the underlying Partial Differential Equation (PDE). This challenge problem would test the performance of GMG when used to solve a problem discretised using the finite element method (FEM).

GMG can be used to solve the linear systems that arise from the PDEs discretised using the FEM, finite volume (FV) or finite difference, on arbitrary domains and with any choice of boundary conditions. In a number of problems multigrid is a known optimal solver and a full multigrid sweep obtains a solution with computational cost O(n) (for an n×n matrix).  This makes the method applicable to a wide range of continuum mechanics problems (e.g: problems in elasticity and fluid dynamics). GenX have produced a demonstration application for solving the Navier Stokes equations for high speed fluid flow (high Reynolds number) using geometric multigrid as key component of the solver algorithm. Another advantage of GMG, which makes it well suited for large scale problems, is that the solver never has to assemble the n×n system matrix, which for large n would overwhelm the system memory (in other words, multigrid can be applied as a matrix free method).

The HPGMG benchmark is an attempt to assess the performance of systems when solving sparse linear algebra systems using GMG. This is in contrast to LINPACK or HPCG benchmarks, which primarily data access patterns and general performance of dense linear algebra direct solves or iterative solvers respectively. The HPGMG-FEM benchmark produces performance data specifically for the FEM discretisation, there is also a FV variant with slightly different performance characteristics.

The appetite of continuum mechanics for computing resources is unbounded, since simulations require complex domains, high resolution meshes, and high order discretisation to provide accurate solutions of PDEs. Furthermore, these problems form just the inner loop of the interesting research questions. To be able to perform Uncertainty Quantification (UQ) these simulations are placed in an outer loop, for instance using Multilevel Monte Carlo (MLMC). Currently it is possible to fully occupy a supercomputer just performing a single instance of a continuum mechanics simulation, the real research problems involve running a whole ensemble of such simulations. It is therefore essential that the solver routine runs quickly and efficiently on any future HPC hardware.

## Challenge Problem definition

### Physical phenomena and associated models

HPGMG solves variable-coefficient elliptic problems (-b div beta grad u = f) on isotropic Cartesian grids using FV or FEM and Full Multigrid (FMG). The details of which are summarised on the [HPGMG webpage](https://crd.lbl.gov/divisions/amcr/computer-science-amcr/par/research/hpgmg/).

### Numerical approach, algorithms

HPGMG offers two discretisations of the Poisson equation: FV and FEM. It is the latter that is of primary interest, although FV would give a good proxy for the performance of a FEM solve, if for instance the PETSc dependency could not be satisfied. The FEM full approximation scheme solver has higher arithmetic intensity due to quadrature, metric terms, and higher approximation order than FV.

### Simulation/model details

Typical runs sample across a range of problem sizes in order to understand the range of problem sizes that can be solved efficiently.

 - Operator type, we suggest biquadratic elements. (Specify with -op_type poisson2)
 - Minimum and maximum number of elements per MPI rank, which must be chosen on a system by system basis to effectively occupy the machine.
 - The number of different problem sizes to try between the minimum and maximum problem sizes.
 - Number of repeats to eliminate performance variability.
 - The minimum interval (in seconds) for repeatedly solving each problem size to ensure the problem runs long enough to obtain reliable timings.

See the [HPGMG bitbucket repository](https://bitbucket.org/hpgmg/hpgmg) for details of the precise command line options available.

### Demonstration calculation requirements

The [submission guidelines on the HPGMG website](https://crd.lbl.gov/divisions/amcr/computer-science-amcr/par/research/hpgmg/) dictate the calculation requirements, but for the FV solver, adapting these for the FEM solver:

> Run the hpgmg-fe executable using the default configuration and providing suitable command line arguments (see simulation/model details above). The final results must benchmark 3 problem sizes of grid spacings h, 2h, and 4h (e.g. 256^3 per socket, 128^3 per socket, 64^3 per socket) performing at least 10 solves and running for at least 60s for each. A single invocation of either the CPU or GPU reference implementations will benchmark these three problem sizes, summarize performance, and validate the error properties.

HPGMG will currently run in both an MPI+OpenMP and MPI+CUDA GPU configuration.

### Resource requirements to run calculation

The problem size can be scaled to fill any size machine available. There is a list of performance results available on the [HPGMG webpage](https://crd.lbl.gov/divisions/amcr/computer-science-amcr/par/research/hpgmg/), which gives examples of resources required and performance expectations for existing HPC architectures.

The run solves the smallest problem size first to provide instant feedback about incompatible configuration. Then it solves the largest problem size to ensure that the whole run will not exceed machine memory. Timing data is then collected across the range of problem sizes in between.

### Figure of merit definition

Timing data is collected across the range of problem sizes specified. Timing is reported in seconds per solve, manually counted gigaflops (GF), and millions of equations solved per second (Meq/s). The figure of merit used by the HPGMG benchmarking group is the number of gigaflops obtained on the finest, second finest and third finest meshes.
