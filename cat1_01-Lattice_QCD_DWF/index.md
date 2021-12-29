# Lattice QCD with Domain-Wall Fermions

## Basic information

 - Design and development working group name: EXALAT
 - Challenge Problem Category: Category 1 
 - Contacts for this Challenge Problem: Luigi Del Debbio (luigi.del.debbio@ed.ac.uk),
   Jonathan Flynn (j.m.flynn@soton.ac.uk), Andreas Juettner (juettner@soton.ac.uk)

## Brief description of exascale use case 

Lattice Gauge Theory provides the most robust framework for computations involving strongly interacting systems. Among the latter is Quantum Chromodynamics (QCD), which describes the strong force in the standard model. Several problems are still open in this field, whose solution is paramount for interpreting and supplementing current high-energy experimental results. These problems require exascale performance. Among the major open questions, the proposed challenge focusses on flavour physics. In our approach, we make use of a first-principle lattice formulation of the theory in which the elementary matter fields, the quarks, are described in the Domain Wall formalism. This formalism requires the use of an extra dimension to obtain four-dimensional massless quarks. On the one hand, this approach solves one of the major fundamental issues of Lattice QCD, i.e., how to provide a rigorous discretisation of real-world massless QCD. On the other hand, the proposed approach is computationally very intensive, the Domain Wall formulation being the most numerically expensive one among those used in the current literature. Overall, the benefit-cost comparison makes the proposed problem an ideal exascale challenge for the ExCALIBUR programme.   

## Challenge Problem definition

### Physical phenomena and associated models

We shall study the decays of K, D and B mesons, examining both simple single-hadron final states and more complex processes involving multi-hadron final states, decay-induced mixing, long- distance effects and electromagnetic processes. 

### Numerical approach, algorithms

We shall use the methods of lattice QCD and a chiral fermion formulation. Electromagnetic effects are treated with infinite-volume methods. Linear and bi-linear combinations of composite operators are renormalized non-perturbatively. The problem requires Lanzcos eigenvectors, deflation, all-to-all propagators, all-mode-averaging, open boundary conditions and Fourier acceleration. All these methods are already implemented in our codes. 

### Simulation/model details

The four-dimensional target lattice volume is V = 963 × 384 with a lattice spacing of a = 0.055 fm. The Wilson gauge action and Mobius Domain-Wall Fermions will be used. 

### Demonstration calculation requirements

The demonstration calculation shall perform Monte Carlo evolutions for 5 time units on a 963 x 384 lattice, with the lattice spacing a = 0.55 fm and at the physical quark mass (final system). As an input, it should take a configuration constructed from 162 thermalised configurations for a 323 x 64 lattice at the same values of the quark mass and of the lattice spacing (baseline system). A standard suite of measurements should be executed on a single configuration. 

### Resource requirements to run calculation

25% of the full exascale machine for 6 hours for evolution and for 10 hours for measurements. 

### Figure of merit definition 

The baseline figure of merit (FOM) will be calculated as the geometric mean of the gauge-generation FOM and the analysis FOM for that fermion type. Each of those FOMs is defined to be 

FOM(B_a, F_b) = [ t_a(B) * f_a(B) * n_b ] / [ t_b(F) * f_b(F) * n_b ]

where *B*, *F* represent the baseline and final system; *a*, *b* represent baseline and target problems; and *t*, *f*, and *n* represent the wall time, fraction of the system used (assuming benchmark run is part of a large ensemble), and complexity of the problem (FLOPs). 
