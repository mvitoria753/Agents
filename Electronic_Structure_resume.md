# Electronic Structure — A Summary

A quick-reference overview of electronic structure theory, the broader field
that Density Functional Theory (see `DFT_resume.md`) belongs to. Written to
accompany this repository's materials-science examples (VASP, PBEsol,
PAW_PBE — see `CLAUDE.md` in `02_agents_with_tools_advanced.ipynb`).

## 1. What "Electronic Structure" Means

Electronic structure theory is the study of how electrons are distributed
and behave around nuclei in atoms, molecules, and solids — their energies,
spatial distribution, and how they respond to external fields. Knowing the
electronic structure of a system underlies nearly all of its chemical and
physical properties: bonding, reactivity, conductivity, optical response,
magnetism, and mechanical strength.

## 2. The Core Problem

The full description requires solving the many-body Schrödinger equation
for all electrons and nuclei simultaneously — a problem with no closed-form
solution beyond the simplest systems (e.g. the hydrogen atom), because
electron-electron interactions couple every particle to every other.

### Born-Oppenheimer Approximation
Since nuclei are thousands of times heavier (and slower) than electrons,
nuclear and electronic motion are decoupled: nuclei are treated as fixed
point charges while electrons instantaneously relax around them. This is
the starting point of essentially all electronic structure methods and
reduces the problem to solving the electronic Schrödinger equation for a
fixed set of nuclear positions.

Even with this simplification, the remaining electronic many-body problem
is still intractable to solve exactly for more than a few electrons — which
is why the methods below exist as different strategies for approximating it.

## 3. Families of Methods

| Approach | Idea | Strengths | Costs |
|---|---|---|---|
| Hartree-Fock (HF) | Each electron moves in the mean field of the others; antisymmetrized wavefunction | Exact exchange, good starting point | Neglects electron correlation |
| Post-HF (MP2, CI, CCSD(T)) | Systematically add correlation on top of HF | Very high accuracy ("gold standard" is CCSD(T)) | Scales steeply (N^5–N^7+); limited to small systems |
| Density Functional Theory (DFT) | Reformulates the problem in terms of electron density instead of the wavefunction | Good accuracy/cost balance; scales to hundreds of atoms | Accuracy limited by the exchange-correlation functional (see `DFT_resume.md`) |
| Many-body perturbation theory (GW, BSE) | Corrects DFT's one-electron energies and adds electron-hole interactions | Much better band gaps and optical spectra than plain DFT | Significantly more expensive than DFT |
| Tight-binding / semi-empirical | Parametrized, simplified Hamiltonians fit to reference data or symmetry | Very fast; good for large systems, qualitative trends, model building | Accuracy depends entirely on parametrization/transferability |
| Quantum Monte Carlo (QMC) | Stochastic sampling of the many-body wavefunction | Among the most accurate methods available | Very computationally expensive |

DFT (covered in detail in `DFT_resume.md`) sits in the middle of this
landscape: it is the most widely used method today because of its favorable
accuracy-to-cost ratio.

## 4. Periodic vs. Molecular Electronic Structure

- **Molecules / finite systems**: electrons occupy discrete molecular
  orbitals (built from atomic-orbital basis sets, e.g. Gaussians). Typical
  codes: Gaussian, ORCA, PySCF.
- **Periodic solids**: translational symmetry lets electron states be
  described by **Bloch's theorem** — wavefunctions labeled by a crystal
  momentum `k` within the first **Brillouin zone**. Properties are computed
  by sampling a grid of `k`-points (as referenced in this repo's
  `CLAUDE.md` k-spacing convention). Typical codes: VASP, Quantum ESPRESSO,
  ABINIT, CASTEP.

## 5. Key Observables

- **Band structure**: electron energy `E` as a function of `k`, showing
  occupied/unoccupied bands and the gap between them (metal vs.
  semiconductor vs. insulator).
- **Density of states (DOS)**: number of electronic states per unit energy;
  projected DOS (PDOS) attributes contributions to specific atoms/orbitals.
- **Fermi surface**: the constant-energy surface in `k`-space at the Fermi
  level, central to understanding metals' transport properties.
- **Optical / dielectric properties**: absorption spectra, refractive
  index — from transitions between occupied and unoccupied states.
- **Effective mass**: curvature of bands near band extrema, governing
  carrier mobility in semiconductors.
- **Charge/spin density**: real-space distribution of electrons, used to
  analyze bonding character and magnetism.

## 6. Localization and Post-Processing Tools

- **Wannier functions** (e.g. via Wannier90): transform delocalized Bloch
  states into localized, chemically intuitive orbitals; used for
  interpolating band structures, computing polarization, and building
  tight-binding models from first principles.
- **Population/bonding analysis** (Bader charges, COHP/COOP, NBO): extract
  chemically meaningful bonding information from a converged electronic
  structure calculation.

## 7. Common Software

VASP, Quantum ESPRESSO, ABINIT, CASTEP, CP2K, GPAW, Wannier90 (periodic
solids); Gaussian, ORCA, PySCF, Psi4 (molecules); QMCPACK, CASINO (quantum
Monte Carlo).

## 8. Further Reading

- N. W. Ashcroft and N. D. Mermin, *Solid State Physics*, Holt-Saunders.
- R. M. Martin, *Electronic Structure: Basic Theory and Practical Methods*,
  Cambridge University Press.
- A. Szabo and N. S. Ostlund, *Modern Quantum Chemistry*, Dover.
- See `DFT_resume.md` in this repository for a detailed treatment of DFT
  specifically.
