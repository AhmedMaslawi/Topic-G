# How Strong Is a Single Lennard-Jones Bond?

**Author:** Ahmed Maslawi  
**Affiliation:** Göteborgs Universitet (Physics Master)  
**Course:** Simulation of Complex Systems (FFR120 / FYM119) — Project Topic G

---

## Abstract
At the nano scale, the strength of a single non covalent bond cannot and is not described by a single number, there is always thermal fluctuations and the rate at which an external force is applied strongly influence when and how the bond dissociates (Evans & Ritchie, 1997; Abkenar et al., 2017). In this project, we use Langevin molecular dynamics simulations to investigate how to measure and characterize the strength of a single Lennard-Jones (LJ) bond between two particles. By simulating single molecule pulling experiments with a harmonic trap moving at constant velocity, we compare two complementary measures of bond strength: the dynamic **rupture force** and the equilibrium **binding free energy** ($\Delta F$) reconstructed from nonequilibrium work distributions via **Jarzynski's equality** (Jarzynski, 1997; Hummer & Szabo, 2001; Liphardt et al., 2002). We evaluate why a single simulation trajectory is insufficient due to thermal noise and systematically test how pulling velocity, temperature, spring stiffness, and ensemble size govern the accuracy and convergence of both approaches.

## Research Questions
1. **Reliability of a single realization:** Why can a single pulling trajectory not provide a reliable estimate of bond strength, and how large an ensemble ($N$) is required for both measurement methods to converge?
2. **Dependence of dynamic rupture force on parameters:** How do the rupture force distribution and mean rupture force depend on pulling velocity ($v$), spring stiffness ($k$), and temperature ($T$), and how do they compare to the deterministic zero-temperature maximum restoring force ($F_{\max} \approx 2.40\,\epsilon/\sigma$)?
3. **Accuracy and breakdown of Jarzynski's equality:** Under which pulling velocities and temperatures can Jarzynski's equality accurately reconstruct the equilibrium binding free energy ($\Delta F$) from nonequilibrium pulling trajectories, and where does finite-sampling bias cause the estimator to break down?

## Methods Overview

### 1. Physical Model
We model two particles in reduced Lennard-Jones units ($\sigma = 1$, $\epsilon = 1$, $k_B = 1$) interacting via the Lennard-Jones potential:
$$U_{\text{LJ}}(r) = 4\epsilon \left[ \left(\frac{\sigma}{r}\right)^{12} - \left(\frac{\sigma}{r}\right)^{6} \right]$$
which has an equilibrium separation at $r_0 = 2^{1/6}\sigma$ and a potential well depth of $\epsilon$. One particle is held fixed at the origin while the second particle is coupled to a harmonic trap (modeling an AFM cantilever) of stiffness $k$ moving at constant pulling velocity $v$:
$$U_{\text{trap}}(r, t) = \frac{1}{2}k\left(r - r_0 - vt\right)^2$$

### 2. Simulation Protocol
Trajectories are evolved using overdamped Langevin dynamics (Euler integration) with friction coefficient $\gamma$ and Gaussian thermal white noise $\xi(t)$ at temperature $T$:
$$\gamma \dot{r} = -\frac{\partial}{\partial r}\left[U_{\text{LJ}}(r) + U_{\text{trap}}(r, t)\right] + \sqrt{2\gamma k_B T}\,\xi(t)$$
Before pulling begins ($t = 0$), each realization is thermally equilibrated in the combined bound potential so that initial positions follow the Boltzmann distribution.

### 3. Measurement Approaches and Parameter Study
For ensembles of $N$ independent trajectories, we compute and compare two bond-strength metrics:
- **Dynamic Rupture Force ($F_{\text{rupture}}$):** Recorded as the maximum trap force $F(t) = k(r_0 + vt - r(t))$ reached prior to bond dissociation. We analyze its distribution and mean scaling with loading rate $kv$ and temperature $T$ (Evans & Ritchie, 1997; Abkenar et al., 2017), validating the low-temperature limit against the analytical maximum LJ restoring force $F_{\max} = \max_r |dU_{\text{LJ}}/dr|$.
- **Nonequilibrium Work and Free Energy ($\Delta F$):** For each trajectory, the external mechanical work performed by the moving trap is computed as
  $$W = \int_0^{\tau} \frac{\partial U_{\text{trap}}}{\partial t}\,dt = kv \int_0^{\tau} \left(r_0 + vt - r(t)\right) dt$$
  From the ensemble of work values, the equilibrium free energy difference is estimated via Jarzynski's equality (Jarzynski, 1997; Liphardt et al., 2002):
  $$\Delta F = -k_B T \ln \left\langle e^{-W / k_B T} \right\rangle$$
  and compared against the exact analytical free energy difference of the 1D system.

We systematically sweep pulling velocity $v$ (from quasi-static to fast pulling), temperature $T$, trap stiffness $k$, and ensemble size $N$ to quantify statistical uncertainties and map the regimes where each method succeeds or breaks down.

## References
- Abkenar, M., Gray, T. H., & Zaccone, A. (2017). Dissociation Rates from Single-Molecule Pulling Experiments under Large Thermal Fluctuations or Large Applied Force. *Physical Review E*, 95(4), 042413. https://doi.org/10.1103/PhysRevE.95.042413
- Evans, E., & Ritchie, K. (1997). Dynamic Strength of Molecular Adhesion Bonds. *Biophysical Journal*, 72(4), 1541–1555. https://doi.org/10.1016/S0006-3495(97)78802-7
- Hummer, G., & Szabo, A. (2001). Free Energy Reconstruction from Nonequilibrium Single-Molecule Pulling Experiments. *Proceedings of the National Academy of Sciences*, 98(7), 3658–3661. https://doi.org/10.1073/pnas.071034098
- Jarzynski, C. (1997). Nonequilibrium Equality for Free Energy Differences. *Physical Review Letters*, 78(14), 2690–2693. https://doi.org/10.1103/PhysRevLett.78.2690
- Liphardt, J., Dumont, S., Smith, S. B., Tinoco, I., & Bustamante, C. (2002). Equilibrium Information from Nonequilibrium Measurements in an Experimental Test of Jarzynski's Equality. *Science*, 296(5574), 1832–1835. https://doi.org/10.1126/science.1071152