# Contributing

Bug reports, usability improvements, source corrections, and additional fuel-test data are welcome.

Because this repository is now both a calculator and a model-comparison project, contributions should preserve the distinction between **data**, **supplier guidance**, and **project-derived formulas**.

## Reporting a calculator bug

Please include:

- browser/version;
- inputs used;
- selected cloud-point model (when the selector exists);
- expected result;
- actual result; and
- whether Advanced endpoint values were changed.

## Submitting new fuel data

The most useful dataset includes:

1. measured neat #1 CP;
2. measured neat #2 CP;
3. one or more measured blend ratios;
4. test method;
5. repeatability/error information if available;
6. fuel grade/type;
7. date/season;
8. refinery/terminal/region if known; and
9. a stable source or lawful copy.

Clearly label each value as one of:

- direct controlled measurement;
- product specification;
- supplier/industry guidance;
- digitized chart value;
- project-derived model output; or
- anecdotal field experience.

Do not silently mix these categories.

## Proposing a new cloud-point model

A new model should document:

- formula and coefficients;
- why the mathematical family was chosen;
- endpoint behavior;
- monotonicity over 0–100% #1;
- source provenance families and fitting evidence groups used;
- source/evidence-group weighting;
- whether robust loss/outlier handling was used;
- measured-data error;
- supplier-guidance error;
- worst-case error; and
- leave-one-evidence-group-out behavior when enough independent sources exist.

A better fit to one chart or one point is not sufficient.

### Minimum comparison set

At minimum compare against:

- Minnesota 50/50 petroleum result;
- Iowa 57%, 80%, and 90% petroleum points;
- Iowa full-curve / graph-derived checks when appropriate;
- Cenex lower-range 3°F-per-10% guidance;
- Cenex Southern Region guideline (Agtegra-hosted); and
- at least one known counterexample/out-of-domain dataset.

## Do not overfit approximate charts

Supplier graphics are valuable operational evidence, but visually read points have digitization uncertainty and may themselves be rounded or schematic.

A model that exactly traces every approximate chart point is not automatically more credible than a slightly higher-error model that generalizes across independent sources.

## Cold-flow / operability changes

Do not treat CP, PP, CFPP, LTFT, and equipment operability as interchangeable.

A CFI-model contribution should identify:

- additive/product;
- fuel composition;
- treatment rate;
- untreated CP/PP/CFPP/LTFT;
- treated values;
- test type; and
- whether the claimed benefit changes with dose or blend composition.

Do not implement a linear dose-to-temperature multiplier without product-specific evidence.

## Documentation changes

Research corrections are welcome, including cases where a previous project interpretation was too favorable. The repository intentionally records corrected analyses rather than deleting the history.

## Safety and disclaimer language

Do not weaken warnings or imply guaranteed fuel/equipment performance without a clear technical and legal basis.

## Contribution license

By submitting a contribution, you represent that you have the right to submit it and agree that the contribution may be distributed as part of this project under the repository's **PolyForm Noncommercial License 1.0.0**.

Do not submit third-party copyrighted figures/tables unless redistribution rights are clear. Prefer citations and independently generated plots based on factual values.
