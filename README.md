# Spherical designs on S²: t = 15, 19, 21 with 124, 198, 234 points, proved to exist

**Computer-assisted existence proofs for three antipodal spherical designs found numerically by AlphaEvolve (Problem 32).**

A spherical t-design is a finite set of points on the sphere on which every polynomial of degree at most t averages exactly to its average over the whole sphere. Small designs are hard to certify: published small designs are usually floating-point solutions of a large polynomial system. AlphaEvolve's Problem 32 gave numerical antipodal designs on S²; this repository proves that exact designs exist next to three of them.

## What it shows

1. **Mathematical objective:** prove existence of antipodal spherical t-designs on S² with N points, for (t, N) = (15, 124), (19, 198), (21, 234).
2. **Baseline:** AlphaEvolve's numerical designs (`designs.json`, from the AlphaEvolve repository of problems, Problem 32), with published residuals around 1e-7. No rigorous existence proof for these sizes was found.
3. **Candidate result:** for each (t, N), an exact antipodal t-design with N points exists inside a box of radius 2⁻⁴⁰ (checker 1) and 2⁻⁶⁰ (checker 2) around the refined point.
4. **Verification:** two independent Krawczyk interval proofs per design (python-flint arb at 320 bits with stereographic coordinates; mpmath intervals with spherical angles and a different monomial basis). All six pass. No Lean proof.
5. **Record eligibility:** existence results, not numeric records; they upper-bound the minimal size of an antipodal t-design on S² at these t by a proved configuration.
6. **Novelty:** new rigorous existence results for these (t, N), as far as the search on 23 Nov 2026 found. The numerical configurations are AlphaEvolve's.
7. **Search economics:** each proof runs in seconds (checker 1) to under a minute (checker 2).
8. **Decision:** shipped.

## Numbers

| Design | t | N | Equations | Unknowns (spare, fixed at dyadic values) | Checker 1 contraction (box 2⁻⁴⁰) | Checker 2 contraction (box 2⁻⁶⁰) | Min pairwise distance |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `spherical_design_215124` | 15 | 124 | 119 | 124 (5) | 2.1 × 10⁻⁴ | 4.5 × 10⁻⁹ | ≈ 0.269 |
| `spherical_design_219198` | 19 | 198 | 189 | 198 (9) | 2.6 × 10⁻³ | 9.1 × 10⁻⁷ | ≈ 0.199 |
| `spherical_design_221234` | 21 | 234 | 230 | 234 (4) | 3.8 × 10⁻² | 5.8 × 10⁻⁶ | ≈ 0.185 |

## Verification rules

- Antipodal set: the points come in pairs x, −x, so odd moments vanish automatically. The set is a t-design (t = 2k+1) exactly when, for every even polynomial of degree at most 2k, the sum over the N/2 representatives equals (N/2) × its average on S².
- On S², reduced monomials (one coordinate to power 0 or 1) form a basis of the even polynomials, giving E equations in the representatives' coordinates.
- Spare parameters (N − E) are fixed at exact dyadic values chosen by column-pivoted QR, leaving a square system.
- Krawczyk: if K(X) = m − Y·F(m) + (I − Y·F′(X))(X − m) lies inside the box X, the square system has a unique exact solution in X. Both checkers also report the points stay well separated (no collapse).

## Honest limits

- Computer-assisted, not peer reviewed, no Lean proof (Mathlib cannot be fetched in this sandbox).
- Smaller numerical antipodal designs are listed by Womersley (for example t = 19 with 192 points); those are numerical, not proved here.
- The checkers share the problem definition and the input configuration; they differ in library, coordinates, basis, precision and preconditioner.

## Run it

```sh
pip install python-flint mpmath numpy
python3 krawczyk_design.py designs.json spherical_design_215124 15   # checker 1; also 219198 19, 221234 21
python3 check2_iv.py      designs.json spherical_design_215124 15   # checker 2
```

## How it works

| Piece | Where |
| --- | --- |
| AlphaEvolve's numerical designs | `designs.json` |
| Checker 1: refine, square system, Krawczyk in arb | `krawczyk_design.py` |
| Checker 2: independent Krawczyk in mpmath.iv | `check2_iv.py` |
| Proof records (fixed parameters, midpoints, contraction) | `proof_spherical_design_*.json` |
| Logs (original runs and a re-run from this repo) | `results/` |
| Computation manifest | `computation-manifest.json` |

## Method

Normalize the published points, pair them antipodally, refine by Newton's method in high precision, fix the spare coordinates at dyadic values, and run the Krawczyk test on the resulting square system. The two checkers were written separately: checker 1 uses stereographic coordinates and arb ball arithmetic; checker 2 uses spherical angles, an x²-reduced basis and mpmath intervals.

## Stack

Python 3.13, python-flint 0.9 (arb), mpmath 1.4, NumPy.

## Sources and credit

- Numerical designs: B. Georgiev, J. Gómez-Serrano, T. Tao, A. Zs. Wagner, *Mathematical exploration and discovery at scale*, arXiv:2511.02864; [google-deepmind/alphaevolve_repository_of_problems](https://github.com/google-deepmind/alphaevolve_repository_of_problems), Problem 32.
- R. S. Womersley, efficient and symmetric spherical designs (numerical tables).
- X. Chen, A. Frommer, B. Lang, *Computational existence proofs for spherical t-designs*, Numer. Math. 117 (2011), for the interval method on designs.

Prepared with AI assistance (Claude, Anthropic).
