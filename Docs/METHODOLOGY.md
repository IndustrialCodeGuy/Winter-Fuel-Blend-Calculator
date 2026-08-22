# Methodology

This document separates the **model-independent calculator math** from the **cloud-point model choices**. That distinction becomes important as the experimental calculator gains a model selector.

## 1. Blend-ratio bookkeeping

The UI expresses the blend as percent #2. The #1 fraction is the remainder.

```text
#1 fraction = 1 - #2 fraction
```

Let:

- `H` = current gallons in the tank
- `h₂` = current #2 fraction
- `t₂` = target #2 fraction
- `A` = gallons of straight #1 to add

Current #2 gallons:

```text
G₂ = H × h₂
```

Because adding straight #1 does not change the gallons of #2 already present:

```text
G₂ / (H + A) = t₂
```

Solving for `A`:

```text
A = (H × h₂ - H × t₂) / t₂
```

The requested target is infeasible by adding #1 when the existing tank already contains more #1 than the target permits.

## 2. Tank capacity

```text
final gallons = H + A
remaining capacity = tank size - final gallons
```

The calculator treats liquid volumes as directly additive for planning purposes. It does not correct for thermal expansion, contraction, or density differences.

## 3. Why normalize cloud-point response?

A useful cloud-point model should not require one fixed #1 or #2 temperature. Supplier batches and refinery regions vary.

For that reason the project models a dimensionless cloud-point response:

```text
x = fraction of #1 diesel, 0 to 1
F(x) = normalized fraction of the endpoint cloud-point drop realized at x
```

The actual blend cloud point is then:

```text
CPblend = CP₂ + (CP₁ - CP₂) × F(x)
```

Equivalent normalization for measured data:

```text
Fmeasured = (CP₂ - CPblend) / (CP₂ - CP₁)
```

This allows a +2/-52°F laboratory pair, a +14/-40°F supplier-guidance pair, and a +6/-60°F northern-tier pair to be compared by **shape** rather than raw temperature.

### Required endpoint behavior

Most project candidate functions are constrained so that:

```text
F(0) = 0
F(1) = 1
```

Therefore pure #2 always returns the entered `CP₂`, and pure #1 always returns the entered `CP₁`.

A production candidate should also be monotonic:

```text
F'(x) >= 0 over 0 <= x <= 1
```

so that adding more #1 cannot make the estimated cloud point warmer solely because of a mathematical wiggle.

## 4. Endpoint changes compress or expand the curve

Changing the #1 or #2 cloud-point input does **not** chop off part of a normalized model. It rescales the same `F(x)` vertically across a different endpoint span.

For example, with #2 fixed at +14°F:

```text
+14 to -55 = 69°F span
+14 to -50 = 64°F span
+14 to -40 = 54°F span
```

A normalized difference of `0.01` therefore corresponds to:

- 0.69°F on a 69°F span;
- 0.64°F on a 64°F span; or
- 0.54°F on a 54°F span.

For repository **model-comparison temperature charts**, the project now uses **+14°F #2 / -50°F #1** as a working pair. The -50°F value is a project comparison choice between the roughly -40°F southern Cenex guidance and the approximately -52 to -53.5°F Minnesota/Iowa measured #1 values. It is not presented as a supplier specification, and it does not change the calculator's separately documented/default endpoint settings.

An additional cross-source observation reinforces why normalization is useful:

| Supply / test reference | Typical or measured #2 CP | Typical or measured #1 CP | Endpoint span |
|---|---:|---:|---:|
| Cenex southern-tier guidance | +14°F | -40°F | 54°F |
| Cenex northern-tier guidance | +6°F | -60°F | 66°F |
| Minnesota petroleum test | +2°F | -52°F | 54°F |
| Iowa petroleum test | +3.2°F | -53.5°F | 56.7°F |

The **Cenex southern-tier guidance and Minnesota test have the same 54°F endpoint span**, even though the Cenex range is shifted about 12°F warmer at both endpoints. The Iowa span is also very close at 56.7°F.

This does not prove that the fuels must share an identical blend curve, but it is an interesting structural result: several otherwise different fuel references span nearly the same total cloud-point range. Comparing their normalized `F(x)` behavior therefore separates **curve shape** from a simple warmer/colder shift in the endpoint pair. The larger 66°F northern-tier span is also useful because it tests how the same normalized shape behaves when the endpoint range expands.

This is why normalized comparison is preferred for model research.

## 5. Evidence classes

Model development uses several evidence classes. They are deliberately kept distinct.

### A. Direct measurements

Examples:

- Minnesota 50/50 petroleum #1/#2 result;
- Iowa stock-fuel blend measurements / published blend percentages.

These receive the highest weight when evaluating general blend shape.

### B. Published plotted curves

The Iowa report contains a full plotted petroleum #1/#2 curve. The original project work used a raster screenshot of that figure. A later source-native review found that the original PDF stores both the fitted line and plotted markers as vector objects.

The repository now treats the **direct PDF vector extraction** as the preferred graph-derived representation. It preserves two distinct things:

- the source-native fitted regression line, which is **not endpoint constrained**; and
- project-developed endpoint-constrained fits to the extracted regression line and plotted markers for normalized model comparison.

The endpoint-constrained project fits are analysis transformations, not equations published by Iowa Central.

### C. Supplier guidance and specifications

Examples:

- Cenex 3°F-per-10% #1 rule;
- Cenex northern/southern typical endpoint guidance;
- Cenex Southern Region blending chart hosted by Agtegra;
- Cenex finished winter-product specifications.

These are highly relevant to a Cenex-oriented production calculator but should not be mislabeled as controlled laboratory blend experiments.

### D. Project-developed formulas

These are analysis tools, not published industry standards. Examples include the rounded cubic, the piecewise hybrid, the source-balanced cubic, and the model-family fits.

### E. Counterexamples

Specialty already-winterized/arctic fuels and renewable-diesel datasets demonstrate that endpoint values alone do not define one universal blend curve.

## 6. Source provenance and fitting-group weighting

A recurring model-development problem is that one source can contain many more points than another. Counting every plotted point equally can accidentally let one publication dominate the fit.

The later systematic pass therefore grouped related observations before fitting rather than simply counting every row equally.

For **source provenance**, the principal independent families are:

- Minnesota measured petroleum diesel;
- Iowa measured petroleum diesel; and
- Cenex/CHS supplier guidance and specifications.

The Cenex/CHS material includes both the general 3°F-per-10% / endpoint guidance and the Southern Region blending chart hosted by Agtegra. The Agtegra-hosted chart is **not treated as an independent Agtegra data family**; Agtegra identifies where the project obtained that Cenex material.

Historically, the later systematic numerical pass split the Cenex material into two **fitting evidence groups** because they constrain different features of the curve: the general rule/endpoints constrain low-range rate and normalization, while the Southern Region chart supplies a full-range supplier-guidance shape. That produced four fitting groups (Minnesota, Iowa, Cenex general guidance, and Cenex Southern guidance), but only three independent provenance families.

For that reason, this repository now describes the systematic test as using comparable **evidence-group influence** and **leave-one-evidence-group-out** validation rather than implying four independent source families. The Iowa PDF graph-derived formulas remain cross-checks rather than additional independent publications. Importantly, the Iowa fitting group used in the later systematic/leave-one-evidence-group-out pass was based on the explicit published stock-blend evidence, **not** the screenshot-derived Iowa fitted-line equation. Replacing the screenshot trace with the direct-PDF vector extraction therefore does not change the systematic cubic coefficients or its historical LOO ranking.

### Evidence hierarchy is not the same as fitting-group balance

The systematic pass deliberately asked a narrow research question: *if the historical evidence groups are given comparable influence, which simple mathematical family best compromises among them?* That is useful for model-family selection, but it does **not** override the evidence hierarchy above.

For selection of a **general petroleum #1/#2 cloud-point model**, the project now interprets the evidence in this order:

1. direct Minnesota/Iowa measured blend points;
2. the Iowa plotted curve as secondary shape evidence, with digitization uncertainty retained;
3. Cenex/CHS supplier guidance as an operational/supplier cross-check; and
4. project-developed supplier-oriented fits as diagnostics.

Under that interpretation, the original exact cubic is essentially the direct measured-point fit, and the rounded general cubic is its deliberately simplified production/research form. The systematic robust cubic remains valuable as a balanced historical-evidence experiment rather than superseding the measured-data family.

## 7. Robust fitting

The Cenex Southern Region guideline has an unusually steep 80→90% change followed by a much smaller 90→100% change. A conventional squared-error fit gives that one 90% point unusually high leverage.

The later systematic model-selection pass therefore used a **robust pseudo-Huber-style loss** so that all points remained present but one isolated discrepancy could not determine the entire equation.

This is a project analysis choice, not a supplier-prescribed fitting method.

## 8. Model-family comparison

The systematic single-formula pass compared:

- endpoint-constrained cubic;
- endpoint-constrained quartic;
- monotonic Bernstein quartic; and
- a simple rational curve.

Every candidate preserved endpoints and was checked for monotonicity.

The endpoint-constrained cubic had the best average **leave-one-evidence-group-out** error among those tested for the comparable-evidence-group objective. The more flexible quartic could fit the combined development data more closely but generalized worse when an entire fitting evidence group was withheld.

This result selects a mathematical family for that systematic compromise exercise; it is not, by itself, a ranking of which evidence source should control the general petroleum model.

See [VALIDATION.md](VALIDATION.md) and [MODEL-CATALOG.md](MODEL-CATALOG.md).

## 9. Current, projected, and final cloud point

Whichever model is selected, the same normalized function should be applied consistently to:

- the current tank estimate;
- the selected target ratio; and
- the calculated final tank composition.

If the existing #1 and newly added #1 use different entered cloud points, the current implementation can volume-weight the #1 endpoint before evaluating the final #1/#2 curve:

```text
effective CP₁ =
    (existing #1 gallons × existing CP₁
     + added #1 gallons × added CP₁)
    / total #1 gallons
```

This remains an approximation; two compositionally different #1 fuels are not guaranteed to behave exactly like one fuel with an averaged endpoint.

## 10. Cloud point vs. estimated operability

Cloud point is modeled first. Cold-flow treatment is a separate layer.

The project intentionally does not redefine cloud point when CFI is enabled because the research consistently shows that common operability additives primarily modify wax-crystal growth/filterability rather than the onset of visible wax.

See [COLD-FLOW-RESEARCH.md](COLD-FLOW-RESEARCH.md).

## 11. Planned experimental model selector

The experimental calculator is expected to expose selected models by name. The intended architecture is:

```text
selected model -> F(x) -> endpoint scaling -> cloud-point output
```

The gallons math should remain unchanged when the user switches cloud-point models.

Recommended UI behavior for future implementation:

- show the selected model name;
- provide a short provenance note;
- keep endpoint inputs visible/adjustable;
- avoid implying that a lower predicted temperature is automatically “better”; and
- clearly identify models that are diagnostic/research-only.
