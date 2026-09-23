# Density Functional Theory (DFT) — A Summary

A quick-reference overview of Density Functional Theory, written to accompany
this repository's materials-science examples (which use VASP with PBEsol and
PAW_PBE pseudopotentials — see `CLAUDE.md` in `02_agents_with_tools_advanced.ipynb`).

## 1. What DFT Is

DFT is a quantum-mechanical method for computing the electronic structure of
atoms, molecules, and solids. Instead of solving the many-body Schrödinger
equation for the full electron wavefunction (which scales exponentially with
the number of electrons), DFT reformulates the problem in terms of the
**electron density** `n(r)`, a function of only three spatial coordinates.
This makes it tractable for systems with hundreds of atoms, and it is the
workhorse method of modern computational chemistry and materials science.

## 2. Theoretical Foundation

### Hohenberg-Kohn Theorems (1964)
1. The ground-state electron density `n(r)` uniquely determines the external
   potential (and hence all ground-state properties) of a many-electron
   system.
2. There exists a universal energy functional `E[n]` that is minimized by
   the true ground-state density.

These theorems prove that the density — not the wavefunction — carries all
the information needed, but they don't say how to construct `E[n]` in
practice.

### Kohn-Sham Equations (1965)
Kohn and Sham made DFT computationally practical by mapping the real
interacting system onto a fictitious system of **non-interacting** electrons
that reproduces the same density. This yields a set of single-particle
Schrödinger-like equations:

```
[ -ħ²/2m ∇² + V_eff(r) ] ψ_i(r) = ε_i ψ_i(r)
n(r) = Σ_i |ψ_i(r)|²
```

`V_eff` includes the external (nuclear) potential, the Hartree (classical
electrostatic) term, and the **exchange-correlation potential** — the one
piece that must be approximated.

## 3. Exchange-Correlation Functionals

All the many-body quantum complexity is packed into the exchange-correlation
(XC) functional `E_xc[n]`. Common approximations, roughly in order of
increasing sophistication and cost:

| Functional class | Examples | Notes |
|---|---|---|
| LDA (Local Density Approx.) | PZ, PW92 | Depends only on local density; cheap, systematically underestimates bond lengths less well than GGA for many solids |
| GGA (Generalized Gradient Approx.) | PBE, PBEsol, PW91 | Adds density gradient; PBEsol is tuned for solids/surfaces (used in this repo's conventions) |
| Meta-GGA | SCAN, r2SCAN | Adds kinetic energy density; better accuracy at moderate extra cost |
| Hybrid | HSE06, PBE0, B3LYP | Mixes in exact (Hartree-Fock) exchange; much better band gaps, significantly more expensive |
| DFT+U | PBE+U, PBEsol+U | Adds a Hubbard correction for localized d/f electrons (transition metals, lanthanides) |

There is no universally "best" functional — the choice trades accuracy
against computational cost and depends on the property and material class
being studied.

## 4. Practical Ingredients of a DFT Calculation

- **Basis set**: plane waves (common for periodic solids, e.g. VASP,
  Quantum ESPRESSO) or localized atomic orbitals (e.g. Gaussian, CRYSTAL).
- **Pseudopotentials / PAW potentials**: replace the core electrons and
  strong nuclear potential with a smoother effective potential, so only
  valence electrons need to be treated explicitly (e.g. PAW_PBE in VASP).
- **Plane-wave cutoff (ENCUT)**: controls the size of the plane-wave basis;
  must be converged for reliable energies and forces.
- **k-point sampling**: the Brillouin zone is sampled on a grid (e.g.
  Monkhorst-Pack); density is often set via a k-spacing parameter.
- **Convergence criteria**: electronic (SCF) convergence and ionic/force
  convergence must both be checked — "converged" results should always
  state what was tested.

## 5. What DFT Is Good At

- Ground-state total energies, equilibrium structures, and elastic
  properties.
- Formation energies, phase stability, and thermodynamic convex hulls.
- Vibrational (phonon) properties via forces/force constants.
- Electronic structure (bands, DOS) — qualitatively reliable, though band
  gaps are systematically underestimated by LDA/GGA (hybrids or GW
  corrections do better).
- Screening large numbers of candidate materials computationally
  (high-throughput DFT, e.g. the Materials Project database).

## 6. Known Limitations

- Standard LDA/GGA functionals underestimate band gaps and struggle with
  strongly correlated electrons (transition-metal oxides, f-electron
  systems) without corrections like DFT+U or hybrids.
- Van der Waals (dispersion) interactions are not captured by local/
  semi-local functionals and need explicit corrections (e.g. DFT-D3,
  vdW-DF).
- Excited-state properties (optical spectra) generally require methods
  beyond ground-state DFT, such as TDDFT, GW, or Bethe-Salpeter.
- Results depend on functional choice, basis set/cutoff, and k-point
  density — reproducibility requires reporting all of these explicitly.

## 7. Common Software

VASP, Quantum ESPRESSO, ABINIT, CASTEP, CP2K, GPAW, and (for molecules)
Gaussian, ORCA, and PySCF are among the most widely used DFT codes.

## 8. Further Reading

- P. Hohenberg and W. Kohn, *Phys. Rev.* **136**, B864 (1964).
- W. Kohn and L. J. Sham, *Phys. Rev.* **140**, A1133 (1965).
- R. M. Martin, *Electronic Structure: Basic Theory and Practical Methods*,
  Cambridge University Press.
- Materials Project documentation: https://docs.materialsproject.org
