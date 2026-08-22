# Research History

This file records how the project evolved. It is intentionally candid about corrections, dead ends, and formulas that were useful even when they were not selected.

## Stage 1 — original linear calculator

The calculator initially used straight cloud-point interpolation with defaults:

```text
#2 = +14°F
#1 = -16°F
```

That produced exactly 3°F of cloud-point improvement per 10% #1 and therefore looked consistent with Cenex's common rule of thumb.

The problem was the endpoint: -16°F was a mathematical convenience, not a realistic neat-#1 cloud point.

## Stage 2 — realistic #1 endpoints expose nonlinearity

Minnesota and Iowa petroleum-diesel data showed neat #1 roughly in the -52 to -53.5°F range while 50/50 blends remained far warmer than a straight midpoint.

This established the key modeling problem:

> realistic endpoints + linear interpolation produce implausibly cold midrange estimates.

The project moved to a normalized nonlinear curve.

## Stage 3 — first Cenex-literature constrained nonlinear formulas

Before the later normalized Minnesota/Iowa fit, the project explored whether the published Cenex guidance could be made internally consistent with a realistic neat-#1 endpoint.

Using `x` as the fraction of #1 diesel, the working anchors were:

```text
x = 0.0  -> 100% #2 = +14°F
x = 0.5  -> 50/50      = -1°F
x = 1.0  -> 100% #1 = -40°F
```

The `-1°F` 50/50 anchor came from carrying the Cenex 3°F-per-10% guidance through five 10% steps from the +14°F southern-style #2 reference. It is therefore a **Cenex-literature-derived anchor**, not a Minnesota/Iowa measured point.

The simplest curve through those three anchors was the quadratic:

```text
CP = 14 - 6x - 48x²
```

It hits all three points exactly, but it departs too quickly from the Cenex 3°F-per-10% behavior in the ordinary low-to-middle blend range. It was useful as the first demonstration that realistic endpoints required curvature, but it was not selected as the preferred model.

A more useful constrained cubic also preserved the initial Cenex rate of 3°F per 10% #1:

```text
CP = 14 - 30x + 24x² - 48x³
```

For the -40°F neat-#1 working endpoint, this cubic satisfies all four constraints exactly:

```text
CP(0)   = +14°F
CP'(0)  = -30°F per unit x = -3°F per 10% #1
CP(0.5) = -1°F
CP(1)   = -40°F
```

Its predicted values are:

| #1 / #2 | Cenex 3°F/10% line | Constrained cubic |
|---:|---:|---:|
| 0 / 100 | +14.0°F | +14.0°F |
| 10 / 90 | +11.0°F | +11.2°F |
| 20 / 80 | +8.0°F | +8.6°F |
| 30 / 70 | +5.0°F | +5.9°F |
| 40 / 60 | +2.0°F | +2.8°F |
| 50 / 50 | -1.0°F | -1.0°F |
| 60 / 40 | -4.0°F | -5.7°F |
| 70 / 30 | -7.0°F | -11.7°F |
| 80 / 20 | -10.0°F | -19.2°F |
| 90 / 10 | -13.0°F | -28.6°F |
| 100 / 0 | -16.0°F | -40.0°F |

The endpoint-aware generalized form was then written with the actual neat-#1 cloud point `CP₁` as an input:

```text
CP = 14 - 30x - (CP₁ + 16)x² + 2(CP₁ + 16)x³
```

This generalized form always:

1. starts at +14°F for neat #2;
2. starts with the Cenex 3°F-per-10% slope;
3. passes through -1°F at 50/50; and
4. ends at the entered neat-#1 cloud point.

For example, changing the neat-#1 endpoint from -40°F to -55°F only modestly changes the 10–40% region, while its influence grows as #1 becomes the dominant fuel:

| #1 | -40°F #1 model | -55°F #1 model |
|---:|---:|---:|
| 0% | +14.0°F | +14.0°F |
| 10% | +11.2°F | +11.3°F |
| 20% | +8.6°F | +8.9°F |
| 30% | +5.9°F | +6.4°F |
| 40% | +2.8°F | +3.2°F |
| 50% | -1.0°F | -1.0°F |
| 60% | -5.7°F | -6.8°F |
| 70% | -11.7°F | -14.6°F |
| 80% | -19.2°F | -25.0°F |
| 90% | -28.6°F | -38.3°F |
| 100% | -40.0°F | -55.0°F |

This model remains valuable as a possible **Cenex Literature Model**: it is intentionally constrained to match the published Cenex rule-of-thumb structure rather than being fitted to the Minnesota/Iowa real-world test curve. It is an empirical curve construction, not a published Cenex equation or a chemical/thermodynamic law.

## Stage 4 — original exact cubic and rounded general cubic

Using normalized Minnesota/Iowa points produced:

```text
Original exact:
F(x) = 0.59136x - 0.75721x² + 1.16585x³
```

This was rounded to:

```text
Rounded general:
F(x) = 0.6x - 0.8x² + 1.2x³
```

The rounded formula became the practical baseline because the difference from the exact fit was only a few tenths of a degree over realistic endpoint spans.

## Stage 5 — Iowa full-curve validation, screenshot correction, and source-native PDF extraction

The published Iowa graph was first digitized from a raster screenshot to compare the complete curve shape, not only the explicit 57/80/90% stock-blend points.

The first overlay normalized the traced fitted curve by its own traced endpoints. That made agreement look artificially tight. A second pass corrected that problem by calibrating the screenshot's temperature axis and comparing in **absolute temperature** against the formula using Iowa's known +3.2/-53.5°F measured endpoints.

A later source review improved the process again: the original Iowa PDF stores the plotted regression and data markers as **vector objects**. The project therefore replaced screenshot pixel selection with direct source-PDF vector extraction.

That final/source-native pass established that the displayed Iowa regression itself is **not endpoint constrained**:

```text
displayed regression near 0% #1   ≈ +4.58°F
measured #2 endpoint              = +3.2°F

displayed regression near 100% #1 ≈ -56.47°F
measured #1 endpoint              = -53.5°F
```

Using the source-native line, the Rounded General model evaluated with Iowa's measured endpoints is approximately **0.5–1.3°F colder through most of the 20–80% range**, reaches about **1.5–1.6°F colder around 5–15%**, and becomes essentially coincident near 90%.

On a 5% grid from 10–80%, the Rounded General vs. source-native Iowa fitted-line comparison is approximately:

```text
RMSE ≈ 1.05°F
MAE  ≈ 0.99°F
maximum absolute difference ≈ 1.60°F
```

The raw regression endpoint overshoot is now preserved rather than silently forced away. For normalized apples-to-apples model comparisons, separate endpoint-constrained project fits are derived from the direct-PDF line and markers.

This three-step history is retained because it shows why **source-native extraction, absolute calibration, and endpoint constraints are different methodological choices**.

## Stage 6 — Cenex/CHS research

Cenex material added several operational references:

- +14°F southern #2 / +6°F northern #2 typical guidance;
- roughly -40°F southern #1 / -60°F northern #1 guidance;
- 3°F CP reduction per 10% #1 rule;
- +10°F #2 -> roughly -5°F at 50/50 example; and
- Wintermaster / cold-flow-operability guidance.

The Cenex Southern Region chart hosted by Agtegra supplied a full supplier-oriented curve and showed stronger midrange CP reduction than the original general cubic in several regions.

This motivated a series of Cenex Southern-guideline-oriented experimental formulas.

## Stage 7 — new test cubic and the hybrid

A more aggressive single cubic was tried:

```text
F(x) = 0.8300x - 1.3109x² + 1.4809x³
```

It fit the 30–50% Cenex Southern Region range much better but gave up the very good 10–20% behavior of the original curve.

That led to the piecewise hybrid:

- original exact through 20%;
- smooth transition 20–30%;
- new test cubic from 30% upward.

The hybrid performed extremely well against the supplier-guidance curve and naturally reconverged at high #1 percentages.

Its weakness was not numerical; it was **third-party explainability**. A reader can reasonably ask why 20% and 30% are special transition boundaries.

## Stage 8 — localized corrections, minimax, and splines

Several alternatives tested whether the 30% mismatch could be redistributed without changing the entire curve:

- minimax/balanced cubic;
- localized 20–60 correction;
- broader balanced correction;
- monotonic PCHIP spline through all supplier points; and
- restrained production-oriented spline through selected anchors.

These experiments were useful for understanding the shape, but the more directly they traced the supplier graphic, the harder they became to defend as general physical models.

## Stage 9 — one global cubic approximating the hybrid

A global cubic was fitted to reproduce most of the hybrid's behavior without a piecewise carve-out:

```text
F(x) = 0.67868x - 0.86630x² + 1.18762x³
```

This “hybrid-like global cubic” became a strong public-facing single-formula candidate, especially before the later evidence-group pass.

## Stage 10 — Iowa graph-derived formulas, later superseded by direct-PDF fits

The Iowa graph was originally revisited from the screenshot and two endpoint-constrained project cubics were produced:

```text
historical screenshot fitted-line model:
F(x) = 0.4510x - 0.5719x² + 1.1210x³

historical screenshot plotted-points model:
F(x) = 0.5719x - 0.9072x² + 1.3353x³
```

After the source-native PDF vector extraction, those screenshot-derived diagnostics were superseded by:

```text
direct-PDF fitted-line constrained cubic:
F(x) = 0.484285x - 0.606821x² + 1.122537x³

direct-PDF plotted-points constrained cubic:
F(x) = 0.536995x - 0.731212x² + 1.194217x³
```

The source-native regression itself is retained separately in absolute temperature and is not forced to pass through the measured neat-fuel endpoints.

The revised PDF-derived shapes reinforce the earlier qualitative result:

- the Iowa regression/endpoint-constrained interpretations are generally warmer / shallower than the Rounded General curve through the middle;
- the difference is modest; and
- the original/rounded family remains less aggressive than several Cenex Southern-guideline-oriented experimental formulas.

This reduced confidence that the most aggressive Cenex-only cubic should become the universal model.

## Stage 11 — source-balanced aggregation

An early equal-source aggregation produced approximately:

```text
F(x) = 0.5983x - 0.7094x² + 1.1111x³
```

The surprising result was how close it remained to the original cubic family.

That suggested the project had not strayed away from the evidence; different source-balancing methods were converging back toward the original coefficient region.

## Stage 12 — systematic model-family selection

The final major research step so far froze the major fitting evidence groups and compared several single-formula families under common rules:

- endpoint cubic;
- endpoint quartic;
- monotonic Bernstein quartic; and
- simple rational model.

The fit used comparable evidence-group influence, monotonicity, endpoint constraints, robust handling of isolated supplier-chart anomalies, and leave-one-evidence-group-out evaluation. The four fitting groups were Minnesota, Iowa, Cenex general guidance, and Cenex Southern guidance; the two Cenex groups are not independent provenance families.

The **Iowa fitting group in this systematic pass used the explicit published stock-blend evidence, not either screenshot-derived Iowa graph equation**. The later replacement of the screenshot digitization with direct-PDF vector extraction therefore does not alter the reported systematic coefficients or LOO family ranking.

The leading cubic **for that comparable-evidence-group exercise** was:

```text
F(x) = 0.583960x - 0.675308x² + 1.091348x³
```

Leave-one-evidence-group-out ranking:

```text
Cubic              0.0347 normalized RMSE
Bernstein quartic  0.0392
Rational           0.0415
Quartic            0.0459
```

The more flexible families fit the development data more closely in places but did not predict withheld fitting evidence groups as well.

A later review clarified the scope of this result. The systematic pass used four fitting groups—Minnesota, Iowa, Cenex general guidance, and Cenex Southern guidance—even though the two Cenex groups belong to one supplier provenance family. That was appropriate for testing a balanced historical-evidence objective, but it should not be read as overriding the project's evidence hierarchy, which gives direct Minnesota/Iowa measurements the highest weight for general blend shape.

## Stage 13 — current research interpretation

The project now has enough evidence to separate three ideas:

### Preferred general petroleum model

The **Rounded General Cubic** is now the preferred general petroleum model. Its unrounded predecessor is essentially the direct normalized fit to the Minnesota/Iowa measured-point set, and rounding changes the four-point RMSE only from about 0.54°F to 0.56°F while producing simple coefficients and the exact `F(0.5)=0.25` property.

The **Original Exact Cubic** is retained as the measured-data reference.

### Cenex literature-constrained reference

The early constrained/generalized cubic remains a useful alternate model when the goal is to follow the structure of Cenex literature guidance: +14°F southern-style #2, an initial 3°F-per-10% #1 rate, the derived -1°F 50/50 anchor, and a realistic entered neat-#1 endpoint. It should be labeled as a **Cenex Literature Model**, not confused with a fit to the later Southern Region chart.

### Supplier-oriented production behavior

The piecewise hybrid remains a strong practical Cenex Southern-guideline compromise, especially around 30–50%, but is less elegant as a public single equation.

### Systematic evidence-group compromise

The systematic robust/source-balanced cubic remains the strongest result **within the later comparable-evidence-group family-selection exercise**. It is retained as a valuable research compromise, not as a replacement for the measured-data family when the goal is general petroleum blend behavior.

No model is presented as a universal physical law.


## Literature-model background considered during research

The project also looked outside the immediate Minnesota/Iowa/Cenex data at published cloud-point modeling approaches.

A 1997 Saiban/Brown paper describes a **semi-empirical kinetic cloud-point blending model** with adjustable parameters based on cooling-rate/cloud-point behavior. It is useful evidence that cloud-point blending has long been treated as nonlinear, but it requires fitted parameters rather than only two endpoint cloud points.

Source: https://www.sciencedirect.com/science/article/pii/S0016236197001324

A 2002 fuel-blend paper by Coutinho and coauthors applied a more detailed **thermodynamic solid/liquid-equilibrium approach** to cloud/pour behavior. Such models can be more physically descriptive, but they require composition/activity-model information that is not available from the calculator's simple #1/#2 endpoint inputs.

Source: https://sweet.ua.pt/jcoutinho/file/Fuel_2002_81_963.pdf

These literature approaches helped confirm that a nonlinear empirical browser model is reasonable, while also showing why the project should not claim its cubic is a universal thermodynamic law.

## Cold-flow research evolved separately

The additive side of the calculator also changed substantially during research:

- CFI was confirmed to primarily alter wax growth/filterability rather than CP;
- direct literature showed measurable residual LTFT response even in neat #1;
- other sources showed much stronger response in mixed fuels;
- composition and n-paraffin distribution matter; and
- linear dose-to-temperature scaling is not supported.

See [COLD-FLOW-RESEARCH.md](COLD-FLOW-RESEARCH.md).
