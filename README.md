# Winter Fuel Blend Ratio Calculator

A browser-based calculator and research project for estimating **#1/#2 diesel blend ratios**, **cloud point**, **cold-flow treatment quantity**, **estimated operability**, and **tank capacity**.

This repository is intentionally more transparent than a typical calculator. The project now includes several cloud-point models so readers can see how different reasonable interpretations of the available data behave rather than being shown one equation with no context.

> **Pre-publication note:** the documentation has been updated ahead of the planned experimental model-selector UI. The current HTML may still use one fixed cloud-point model. The public experimental build is expected to allow switching among selected models later.

![Full model comparison](Docs/Images/full-model-comparison.png)

## What the project is trying to answer

The practical question is simple:

> If a tank already contains some mixture of #1 and #2 ULSD, how much straight #1 should be added to reach a target ratio, and what cloud point should that blend reasonably be expected to have?

The gallons calculation is straightforward mass balance. The cloud-point calculation is not. Published and supplier data show that #1/#2 cloud point is **nonlinear** and that the shape can vary with the actual refinery streams, wax distribution, and seasonal fuel formulation.

This repository therefore separates:

- **blend-ratio bookkeeping** — stable, model-independent math;
- **cloud-point models** — empirical alternatives that can be compared;
- **supplier guidance** — useful operational references, not laboratory truth;
- **cold-flow improver research** — a separate problem from cloud point; and
- **limitations/counterexamples** — evidence that no one normalized curve is universal.

## Current model status

The project started with a simple linear model and then moved to a normalized nonlinear cubic based primarily on Minnesota and Iowa petroleum-diesel data. Later research added Cenex/CHS guidance, two separate fits digitized from the Iowa graph, cold-flow literature, several alternative model families, and a leave-one-evidence-group-out model-selection pass.

The major models are documented in [MODEL-CATALOG.md](Docs/MODEL-CATALOG.md).

Three particularly important reference models are currently:

### Rounded general cubic

```text
F(x) = 0.6x - 0.8x² + 1.2x³
CP = CP₂ + (CP₁ - CP₂) × F(x)
```

This remains the cleanest **general/research baseline**. It is easy to audit and closely follows the original Minnesota/Iowa-derived exact fit.

### Cenex Literature Cubic

```text
CP = 14 - 30x - (CP₁ + 16)x² + 2(CP₁ + 16)x³
```

This preserves the early Cenex-literature construction that helped move the project away from straight interpolation. It starts from +14°F #2 at the published 3°F-per-10% rate, passes through -1°F at 50/50, and reaches the entered neat-#1 cloud point. It is useful when the goal is to model the **shape implied by Cenex literature guidance** rather than the Minnesota/Iowa measured blend curve. It is a project-developed empirical formula, not a published Cenex equation.

### Systematic robust cubic

```text
F(x) = 0.583960x - 0.675308x² + 1.091348x³
CP = CP₂ + (CP₁ - CP₂) × F(x)
```

This was produced in the later systematic model-family pass using comparable fitting-evidence-group influence, robust loss so that one unusual chart point could not dominate the solution, exact endpoint constraints, monotonicity, and leave-one-evidence-group-out testing. Among the tested single-formula families, the endpoint-constrained cubic generalized best.

This should be viewed as a **leading single-formula candidate**, not as a published industry equation.

## Why multiple models?

The experimental calculator is intended to be useful for learning as well as planning. A model switcher makes it possible to see, for example:

- how the old 3°F-per-10% linear assumption behaves;
- how the endpoint-aware **Cenex Literature Cubic** preserves the published Cenex low-range rule and 50/50 anchor while reaching a realistic entered #1 endpoint;
- how the Minnesota/Iowa-derived curve differs from supplier guidance;
- why the Iowa graph-derived fits are somewhat warmer through the midrange;
- why the Cenex Southern-guideline-oriented test fit is more aggressive around 30–50% #1;
- what a piecewise hybrid gains and what it costs in methodological simplicity; and
- why more flexible quartic/spline models can fit a chart better without necessarily generalizing better.

The project does **not** treat model switching as proof that any one curve is physically exact.

## Evidence hierarchy

The documentation distinguishes between:

1. **direct measured petroleum-diesel data**;
2. **published fitted curves or values derived from published figures**;
3. **supplier product specifications and operating guidance**;
4. **project-derived model fits and diagnostic formulas**; and
5. **counterexamples / out-of-domain data**.

These categories are intentionally not blended together without labeling.

## Cold-flow improver research

Cloud point and additive-assisted operability are separate.

The later research pass strongly supports these points:

- CFI usually changes **wax crystal growth, size, shape, and filterability**, not the temperature at which the first wax appears.
- Additive response depends on the fuel's **n-paraffin concentration and distribution**, the additive chemistry, and treatment rate.
- Straight #1 can still show some LTFT/pour-point response to an additive, but the benefit is not guaranteed to equal the response of #2-heavy blends.
- More additive does **not** imply a proportional temperature benefit; multiple sources show diminishing, inconsistent, or sometimes adverse response at higher treatment rates.

See [COLD-FLOW-RESEARCH.md](Docs/COLD-FLOW-RESEARCH.md).

## Repository guide

- [Methodology](Docs/METHODOLOGY.md) — model-independent calculator math, normalization, and evaluation rules.
- [Model catalog](Docs/MODEL-CATALOG.md) — formulas tried, names used in the project, and why each exists.
- [Data sources](Docs/DATA-SOURCES.md) — measured data, supplier guidance, literature, and counterexamples.
- [Validation](Docs/VALIDATION.md) — measured comparisons, normalized comparisons, model-family testing, and error behavior.
- [Research history](Docs/RESEARCH-HISTORY.md) — how the project evolved and which apparent dead ends were useful.
- [Cold-flow research](Docs/COLD-FLOW-RESEARCH.md) — CFI, CFPP/LTFT/PP, dose response, and neat-#1 behavior.
- [Learning guide](Docs/LEARNING-GUIDE.md) — how to read the curves and understand why the formulas differ.
- [Limitations and warnings](Docs/LIMITATIONS-AND-WARNINGS.md) — intended range, exclusions, safety, and uncertainty.
- [Contributing](CONTRIBUTING.md) — how to propose data or model changes without overfitting.
- [Changelog](CHANGELOG.md) — project/documentation history.

Research CSVs and source links are under [`Docs/Data/`](Docs/Data/).

## Quick start

No build system is required for the current browser calculator.

1. Download or clone the repository.
2. Open `winter-fuel-blend-calculator.html` in a modern browser.
3. Enter current gallons and tank capacity.
4. Set the current and target #1/#2 ratios.
5. Use actual supplier/test cloud-point values in Advanced settings when available.

The blend solver assumes **straight #1 diesel is being added**. A preblended winter product such as a 70/30 winter fuel requires separate blend bookkeeping unless the calculator is explicitly extended to model that product.

## Important warning

This is a **planning and research tool**, not a laboratory test, fuel specification, engineering specification, additive recommendation, or guarantee of equipment operability.

Actual performance can vary materially with refinery stream, batch composition, seasonal formulation, biodiesel content, wax distribution, storage temperature, water, contamination, additive chemistry, treatment and mixing, filters, fuel-system design, cooling history, and other factors.

Use supplier specifications, product instructions, actual fuel testing, and appropriate operational/professional judgment for real fuel decisions.

## License

Copyright 2026 Dan Michel.

Licensed under the **PolyForm Noncommercial License 1.0.0** (`PolyForm-Noncommercial-1.0.0`). See [LICENSE](LICENSE) and [NOTICE](NOTICE).
