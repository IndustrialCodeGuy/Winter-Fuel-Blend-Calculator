# Changelog

All notable project changes are documented here.

## Unreleased — research/documentation expansion (2026-08-19)

### Repository direction

- Reframed the GitHub edition as an **experimental / learning-oriented model-comparison calculator**.
- Documentation now anticipates a future model selector; the selector is not claimed to be implemented yet.
- Separated stable blend-ratio math from selectable cloud-point models.

### Cloud-point research

- Restored the earliest endpoint-aware Cenex-literature formulas that led the project toward nonlinear modeling: the three-anchor quadratic and the constrained/generalized cubic.
- Documented the constrained cubic as a possible future **Cenex Literature Model** that follows Cenex literature assumptions rather than the Minnesota/Iowa real-world test curve.
- Normalized Cenex/Agtegra provenance wording: the Southern Region chart is treated as Cenex/CHS guidance hosted by Agtegra, not as an independent Agtegra data family.
- Clarified the distinction between independent **source provenance families** and the four **fitting evidence groups** used in the later systematic numerical pass.
- Added the generalized **Cenex Literature Cubic** to the full model-comparison CSV/chart and Cenex Southern-guideline error summary.
- Regenerated affected charts with Cenex Southern Region attribution and leave-one-evidence-group-out terminology.
- Corrected repository chart paths to match the actual `docs/images/` layout.
- Retained the rounded general cubic and original exact cubic as core baseline models.
- Added two Iowa graph-derived cubic models: one from the displayed fitted line and one from digitized plotted points.
- Added Cenex Southern-guideline-oriented test cubic.
- Documented piecewise hybrid, hybrid-like global cubic, minimax, localized corrections, and monotonic spline experiments.
- Added early source-balanced cubic.
- Added the later systematic source-balanced cubic:

  ```text
  F(x) = 0.583960x - 0.675308x² + 1.091348x³
  ```

- Added endpoint quartic, monotonic Bernstein quartic, and rational family-test results.
- Added leave-one-evidence-group-out comparison showing the constrained cubic generalized best among the tested single-formula families.
- Documented why the approximate Cenex Southern guideline 30%, 60–70%, and 90% regions repeatedly drive model-error patterns.
- Documented robust treatment of the unusual Cenex Southern guideline 90% point rather than silently deleting it.
- Clarified the Cenex 3°F-per-10% rule as a low-to-moderate blend **rate-of-change reference**: the nonlinear family begins to steepen beyond it around the mid-30s to low-40s percent #1, with no implied hard 40% cutoff.
- Normalized numeric precision in repository CSV data files to remove floating-point display artifacts and improve readability.
- Documented the rounded cubic's exact `F(0.5) = 0.25` quarter-span midpoint interpretation with a +14/-55°F example.
- Added a Wintermaster whole-calculator consistency check comparing the rounded 70/30 cloud-point estimate plus the 12°F planning allowance with Cenex's published -30°F operability figure.
- Added the cross-source endpoint-span observation that Cenex southern-tier guidance and the Minnesota test both span 54°F, while the Iowa test is close at 56.7°F despite different absolute endpoint temperatures.

### Iowa graph correction

- Recorded that an early normalized Iowa overlay made agreement appear too tight because traced endpoints were normalized to themselves.
- Retained the corrected absolute-temperature digitization as the preferred Iowa full-curve comparison.

### Cold-flow research

- Added Shrestha et al. (2008), Chiu et al. (2004), Reddy & McMillan (1981), Chandler et al. (1992), Botros (1997), and CRC 673 findings.
- Documented that CFI generally modifies wax growth/filterability rather than cloud point.
- Documented direct evidence that neat #1 can retain some LTFT/PP response to CFI in at least one published fuel/additive combination.
- Documented composition dependence through n-paraffin concentration/distribution.
- Strengthened the conclusion that additive dose should **not** scale linearly with temperature benefit.
- Left the exact high-#1 CFI taper as experimental because the available evidence is product/fuel-specific.

### New documentation

- Added `docs/MODEL-CATALOG.md`.
- Added `docs/RESEARCH-HISTORY.md`.
- Added `docs/COLD-FLOW-RESEARCH.md`.
- Added `docs/LEARNING-GUIDE.md`.
- Expanded `METHODOLOGY`, `DATA-SOURCES`, `VALIDATION`, and `LIMITATIONS-AND-WARNINGS`.
- Added research charts and CSV data under `docs/images/` and `docs/data/`.

## Pre-publication baseline

### Original calculator model

```text
CP = CP₂ + (CP₁ - CP₂) × #1 fraction
```

with defaults:

```text
CP₂ = +14°F
CP₁ = -16°F
```

This exactly reproduced a 3°F-per-10% decline but used an unrealistic neat-#1 endpoint.

### First nonlinear general model

The project moved to:

```text
Fexact(x) = 0.59136x - 0.75721x² + 1.16585x³
```

and its rounded form:

```text
Frounded(x) = 0.6x - 0.8x² + 1.2x³
```

based primarily on normalized Minnesota/Iowa petroleum-diesel evidence.
