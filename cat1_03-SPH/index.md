# ExCALIBUR Exascale Challenge Problem: Simulating the Universe

## Basic information

 - Design and development working group name: Massively Parallel Particle Hydrodynamics	
 - Challenge Problem Category: Category 1

## Brief description of exascale use case

This challenge problem focusses on simulating the evolution of the universe and the formation of stars and galaxies within it. The aim is to be able to understand how processes such as star and black hole formation, and their resulting energy output, regulates the creation of galaxies and the large-scale structure of the Universe. These calculations are essential for exploiting next-generation telescopes such as the square kilometre array, the large synoptic telescope and the Euclid space observatory. Without theoretical calculations of matching size and fidelity, it will not be possible to interpret the data from these new facilities. This requirement forces us to both increase the resolution of current calculations, so that the morphology of galaxies is comparable to the observational data, and to expand the volume of the calculation so that it is comparable to the forth coming deep-sky surveys. The combination of simulations and real-world data will allow us to understand how the properties of galaxies arise from the big bang – we already ready know that black holes must play a key element in this – and allow the large-scale surveys to understand the biases introduced by using galaxies to trace the gravitational structure of the Universe. Simulations of this scale and type are essential if we are to understand the properties of dark matter and dark energy.

## Challenge Problem definition

### Physical phenomena and associated models

The challenge problem will scale up existing simulations demonstrating the potential for a complete simulation of the volume and resolution required by the science goals.  The simulation will include the full range of physical processes need to simulate the universe:

 - Gravity. This is modelled using a Fourier mesh on the largest scales, combined with a fast multipole method at intermediate scales and a direct summation of forces between local particles.
 - Hydrodynamics. This is handled through adaptive smooth particle hydrodynamics, including adaptive timesteps. Particles are used to represent Lagrangian mass elements allowing us to trace metal enrichment as particles flow through the evolving structures. Shock capturing is handled through and adaptive dissipation scheme.
 - Astrophysical Processes. Additional processes such as metal enrichment, star and black hole formation and associated feedback cannot be resolved directly and are handled by including additional source and sink term in the hydrodynamics equations. These additional terms are based on higher resolution simulations, capturing the main features of the physics. 

### Numerical approach, algorithms

The primary algorithms are N-body simulation for the calculation of gravitational forces, and smoothed particle hydrodynamics (SPH) for the calculation of hydrodynamics. We solve these equations in a highly adaptive manner. The Lagrangian scheme also ensures that resolution traces the growth of structure. This is essential in this problem as densities vary by a factor of more than a million across the calculation volume. In the SWIFT code, spatial adaptivity is matched by timestep adaptivity. This makes the calculation possible as the timestep required (eg., by the Courant condition) varies by more than a thousand between regions. 

SWIFT uses fine-grained task parallelism to handle the multi-physics nature of the problem and the extreme adaptivity.  Both aspects makes the per particle work-load difficult to predict, but these variations can be accommodated by the task-based paradigm. In essence the numerical approach is to write down the timestep operator as a series of tasks, many of which can be carried out independently, but all of which have dependencies. The task-engine within SWIFT selects tasks for execution depending on their dependencies and accounting for tasks which conflict because the modify the same data. Between compute nodes, communication is handled by asynchronous MPI, taking advantage of the task system to interleave communication and computation. The division of the computational domain across compute nodes is handled by the Metis optimal graph decomposition method.

### Simulation/model details

Our aim is to run a full physics version of the cosmological problem. The calculations will be carried out in a periodic cube. In order facilitate the comparison of the exascale simulation with smaller calculations, we will replicate a smaller cube in order to match the volume/particle number requirement of the exascale challenge calculation. 

We will base the initial conditions on the low-redshift (present-day) output of an existing calculation in order to maximise the complexity and Multiphysics nature of the challenge problem.

The largest of our current simulations have a volume of (100 Mpc)^3 and 1500^3 particles. On the recently installed Cosma-8 system is able to carry out a calculation with 27 times this number of particles. We propose that a suitable level for the Exascale challenge is a further factor of at least 27 increase in particle number, achieved through further tiling of low-redshift outputs.

### Demonstration calculation requirements

As described above, the starting conditions will be based on low-redshift (present-day) outputs of existing simulations, tiling these to the required number of particles.

It is important that the calculation is run for a representative number of timesteps, so that the full depth of the timestep hierarchy is exercised. This requires a minimum of 2000 steps, with a wall-clock time of around 1.5 hours.

### Resource requirements to run calculation

Based on existing weak-scaling tests, used for the assessment of Cosma-8 performance, the challenge problem will need 100GB of memory per tile. On Cosma-8 we are able to run 8 tiles per node because of the large memory of the system. On other systems, we need to run fewer tiles per node, so the minimal memory requirement is 100GB plus memory for operating system etc.

Our code is not yet able to run on GPU systems and performance is best with substantial memory per core so that network communication can be minimised.

The adaptive timestep nature of the code makes strong-scaling to low particle-per-core loads intrinsically challenging since there will be fewer particles to update than cores in many time steps. On-going work is exploring parallel-in-time adaptivity in order to overcome this limitation, but this is not the focus of the proposed benchmark challenge.

The system fabric needs to fully support MPI standards and implement asynchronous communication. We currently see better code performance using Intel MPI over InfiniBand EDR fabric, but this situation changes rapidly as vendors fix bugs and performance issues in their implementations. The code is written in C and uses a small set of standard libraries, such a parMetis.

Further details and full software requirements can be found at  http://swift.dur.ac.uk.

### Figure of merit definition

Our aim is to demonstrate reasonable weak scaling of the problem into the Exascale regime.
We will use the wall-clock time to complete the required number of steps as the basic figure of merit.

FLOP rates are not themselves a directly useful measure of performance of our code, since the algorithms have been developed to minimise unnecessary FLOPS. As a result, the performance limitations tend to be set by a combination of FLOP, memory bandwidth and network speed. Higher flop rates could be obtained by reducing adaptivity but this would not result in any increase in accuracy.

As noted above, the code is not yet GPU ready and requires significant memory per core to perform well.
