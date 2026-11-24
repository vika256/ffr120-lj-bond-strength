# How Strong Is a Single Lennard-Jones Bond?

**Author:** Vishesh Kamani

**Course:** FFR120 Simulation of Complex Systems

**Project Topic:** G

---

## Abstract

The Lennard-Jones (LJ) potential [1] is a simple model of how two atoms attract each other. The two particles sit most comfortably at a distance of about $1.12\sigma$, and pulling them apart costs an energy $\varepsilon$, the depth of the potential well.

On paper, the strength of this bond is easy to read off the potential. It takes energy $\varepsilon$ to separate the particles, and the largest force the bond can hold is about $2.4\,\varepsilon/\sigma$. In a simulation at finite temperature, things are less clear. The particles constantly jiggle due to thermal noise, so the bond breaks at random moments. How strong it looks depends on how you test it and how long you wait [2, 3].

In this project we simulate a single LJ bond and measure its strength in two ways:

1. **Waiting for it to break (thermal dissociation).** We hold the bond at a given temperature and record how long it lasts before it breaks by itself. From how the lifetime changes with temperature, we estimate the energy needed to break the bond [2].
2. **Pulling it apart (force spectroscopy).** We attach a spring to one particle, pull it away at a constant speed, and record the force at which the bond breaks. We repeat this for different pulling speeds and temperatures [3, 4].

We check both methods against cases where the exact answer is known, compare their results, and study how they depend on the simulation settings. Because the bond breaks at random, one simulation run gives a different answer each time. We therefore also ask **how many runs are needed to get a reliable estimate**, and whether a single run is ever enough.

## Methods Overview

**Model.** Two particles interact through the LJ potential
$V(r) = 4\varepsilon\left[(\sigma/r)^{12} - (\sigma/r)^{6}\right]$ [1].
We use reduced units, setting the mass, $\sigma$, $\varepsilon$ and Boltzmann's constant to 1. The simulation is in 3D. We call the bond broken when the particles move further apart than a cutoff distance (default $2.5\sigma$), and we test how much this choice matters.

**Simulation.** The particles move according to Langevin dynamics: Newton's equations plus friction and random kicks that model a surrounding heat bath at temperature $T$. We integrate the equations with the BAOAB method [5]. Many independent copies of the bond run at the same time using NumPy arrays.

**Method 1: waiting for the bond to break.**
- Start many copies of the bond in the bound state and record when each one breaks.
- Look at the spread of these lifetimes to see how much a single run can be trusted.
- Repeat at several temperatures and fit the breaking rate to Kramers' theory [2] to estimate the bond energy. Compare the result with $\varepsilon$.

**Method 2: pulling the bond apart.**
- Pull one particle away with a spring moving at constant speed, and record the force at which the bond breaks.
- Check that slow pulling at zero temperature gives the exact maximum force of the potential, about $2.4\,\varepsilon/\sigma$.
- Repeat for different pulling speeds and temperatures, and compare with the theories of Evans and Ritchie [3] and of Dudko, Hummer and Szabo [4].

**Analysis.** We estimate the uncertainty of every result from many independent runs. We then compare the two methods with each other and with the exact values from the potential, and we study how the uncertainty shrinks as the number of runs grows.

## References

Peer-reviewed references:

1. Lennard-Jones, J. E. (1924). On the determination of molecular fields. II. From the equation of state of a gas. *Proceedings of the Royal Society of London. Series A*, 106(738), 463–477. https://doi.org/10.1098/rspa.1924.0082
2. Kramers, H. A. (1940). Brownian motion in a field of force and the diffusion model of chemical reactions. *Physica*, 7(4), 284–304. https://doi.org/10.1016/S0031-8914(40)90098-2
3. Evans, E., & Ritchie, K. (1997). Dynamic strength of molecular adhesion bonds. *Biophysical Journal*, 72(4), 1541–1555. https://doi.org/10.1016/S0006-3495(97)78802-7
4. Dudko, O. K., Hummer, G., & Szabo, A. (2006). Intrinsic rates and activation free energies from single-molecule pulling experiments. *Physical Review Letters*, 96(10), 108101. https://doi.org/10.1103/PhysRevLett.96.108101
5. Leimkuhler, B., & Matthews, C. (2013). Rational construction of stochastic numerical methods for molecular sampling. *Applied Mathematics Research eXpress*, 2013(1), 34–56. https://doi.org/10.1093/amrx/abs010
