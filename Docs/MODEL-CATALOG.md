# Model Catalog

This file records the names used during development so that charts, code, discussion, and future model-selector labels stay consistent.

Unless otherwise noted:

```text
x = fraction of #1 diesel, 0 to 1
CP = CP₂ + (CP₁ - CP₂) × F(x)
```

The formulas below are **project-developed analysis models** unless they are explicitly identified as an external supplier rule or a curve derived from a published figure.

## Status summary

| Project name | Type | Primary purpose / status |
|---|---|---|
| Old linear / original calculator | linear | historical baseline |
| Cenex 3°F-per-10% rule | supplier rule | external operational reference |
| Cenex Southern Region guideline (Agtegra-hosted) | point/graphic reference | external supplier-guidance reference |
| Cenex three-anchor quadratic | quadratic | first endpoint-aware Cenex-literature experiment; historical reference |
| Cenex literature-constrained cubic | cubic | Cenex-literature model/reference; preserves rule, 50/50 anchor, and entered #1 endpoint |
| Rounded original / rounded general cubic | cubic | general/research baseline |
| Original exact cubic | cubic | precise predecessor to rounded baseline |
| Iowa graph curve | cubic | derived from digitized Iowa fitted line |
| Iowa graph points | cubic | derived from digitized Iowa plotted points |
| New test cubic / Cenex Southern-guideline test fit | cubic | more aggressive midrange diagnostic |
| Piecewise hybrid / production hybrid | piecewise cubic | strong practical compromise; less elegant to explain externally |
| Balanced/minimax cubic | cubic | equalizes Cenex Southern-guideline errors; diagnostic |
| Localized 20–60 correction | local correction | diagnostic, intentionally fits midrange chart points |
| Broad balanced localized | local correction | diagnostic, spreads midrange error |
| Monotonic spline / PCHIP | spline | interpolation experiment |
| Production spline | spline | restrained spline comparison |
| Source-balanced cubic (early) | cubic | first source-balanced aggregation |
| Hybrid-like global cubic | cubic | one-formula approximation to hybrid behavior |
| Systematic source-balanced cubic | cubic | leading single-formula research candidate |
| Endpoint quartic | quartic | systematic family test |
| Monotonic Bernstein quartic | quartic | systematic family test |
| Simple rational | rational | systematic family test |

---

## 1. Old linear / original calculator model

```text
F(x) = x
```

Original defaults:

```text
CP₂ = +14°F
CP₁ = -16°F
```

Those endpoints create exactly a 3°F reduction for each additional 10% #1.

The problem is not the linear equation by itself; the combination of a realistic neat-#1 endpoint with straight interpolation becomes far too cold in the middle compared with the Minnesota/Iowa data.

---

## 2. Cenex 3°F-per-10% rule

External operating guidance rather than a normalized physical law:

```text
CP ≈ CP₂ - 3°F × (#1 percent / 10)
```

When started at +14°F, extending this rule all the way to 100% produces -16°F, which is why the old calculator happened to match the rule while having an unrealistic neat-#1 endpoint.

The rule is most useful as a low-to-moderate-percentage operational check. In the project comparisons, the nonlinear model family begins to develop a steeper local slope than the fixed 3°F-per-10% line around the mid-30s to low-40s percent #1. The apparent separation near 40% is gradual rather than a hard cutoff and is partly emphasized by 10% chart sampling.

---

## 3. Cenex Southern Region guideline (Agtegra-hosted)

No project equation is claimed as an official Cenex formula. The project uses approximate points read from a Cenex Southern Region guideline graphic hosted on Agtegra’s site:

```text
#1% : CP °F
0    : +14
10   : +11
20   : +8
30   : +3
40   : +2
50   : -2
60   : -8
70   : -14
80   : -20
90   : -36
100  : -40
```

The graphic itself is labeled as a guideline. Values read from it are therefore treated as approximate.

---

## 4. Cenex three-anchor quadratic

This was the first nonlinear endpoint-aware experiment built from the Cenex literature references rather than the Minnesota/Iowa measured blend curve.

Using fixed working anchors:

```text
CP(0)   = +14°F
CP(0.5) = -1°F
CP(1)   = -40°F
```

the simplest quadratic is:

```text
CP = 14 - 6x - 48x²
```

The `-1°F` 50/50 point is a project-derived Cenex-literature anchor: starting at +14°F and applying the published 3°F-per-10% rule for five 10% increments gives a 15°F reduction.

Why it was not selected:

- it satisfies the three anchors exactly;
- it proves that a realistic -40°F neat-#1 endpoint can coexist with a much warmer 50/50 blend only by introducing curvature; but
- its low/midrange slope departs too far from the Cenex 3°F-per-10% guidance.

It is retained as a historical reference rather than a recommended calculator model.

---

## 5. Cenex literature-constrained cubic

A more useful early model added a fourth constraint: the curve should **start** at the Cenex rate of 3°F cloud-point reduction per 10% #1.

For the +14°F / -40°F working endpoints:

```text
CP = 14 - 30x + 24x² - 48x³
```

It exactly satisfies:

```text
CP(0)   = +14°F
CP'(0)  = -30°F per unit x = -3°F per 10% #1
CP(0.5) = -1°F
CP(1)   = -40°F
```

A generalized endpoint-aware form keeps the neat-#1 cloud point `CP₁` adjustable:

```text
CP = 14 - 30x - (CP₁ + 16)x² + 2(CP₁ + 16)x³
```

This generalized construction always starts at +14°F, starts at the Cenex 3°F-per-10% slope, passes through -1°F at 50/50, and ends at the entered `CP₁`.

Why it remains useful:

- it closely follows the Cenex rule in the common low-to-middle #1 range;
- it allows a realistic neat-#1 endpoint instead of forcing -16°F;
- endpoint changes have relatively little effect at low #1 percentages and progressively more effect as #1 dominates; and
- it provides a clean candidate for a future **Cenex Literature Model** selector option.

What it is **not**:

- it is not a published Cenex equation;
- it is not fitted to the Minnesota/Iowa measured blend curve; and
- it is not a universal chemical or thermodynamic cloud-point model.

Its purpose is specifically to represent the internal shape implied by the Cenex literature assumptions.

---

## 6. Rounded original / rounded general cubic

```text
F(x) = 0.6x - 0.8x² + 1.2x³
```

Why it matters:

- clean coefficients;
- exact endpoints;
- monotonic;
- `F(0.5) = 0.25` exactly;
- tracks the original exact fit to within only a few tenths of a degree on practical endpoint spans;
- remains the simplest general/research baseline.

---

## 7. Original exact cubic / original exact fit

```text
F(x) = 0.59136x - 0.75721x² + 1.16585x³
```

This came from the original normalized Minnesota/Iowa point fit before coefficient rounding.

It is almost indistinguishable from the rounded general cubic in practical use.

---

## 8. Iowa graph curve

Derived from the digitized **fitted line** in the Iowa #1/#2 graph:

```text
F(x) = 0.4510x - 0.5719x² + 1.1210x³
```

The displayed coefficients sum to `1.0001` because of rounding. This model should be treated as a graph-derived approximation, not the report authors' published equation.

It is generally **warmer / shallower** through the middle than the original project cubic.

---

## 9. Iowa graph points

Derived from digitized **plotted points** from the Iowa graph:

```text
F(x) = 0.5719x - 0.9072x² + 1.3353x³
```

This also tends to be warmer than the original exact model through much of the middle range, though it is closer to the original than the Iowa fitted-line model.

The two Iowa graph fits should not be counted as two independent publications; they are two interpretations of the same figure.

---

## 10. New test cubic / Cenex Southern-guideline test fit

```text
F(x) = 0.8300x - 1.3109x² + 1.4809x³
```

Purpose:

- improve agreement with the stronger 30–50% cloud-point drop suggested by the Cenex Southern Region guidance;
- remain one global cubic;
- naturally reconverge with the original family around 70–80%.

Tradeoff:

- it is noticeably more aggressive at 10–20% than the original/Cenex rule behavior.

---

## 11. Piecewise hybrid / production hybrid

Define:

```text
FA(x) = 0.59136x - 0.75721x² + 1.16585x³
FB(x) = 0.8300x - 1.3109x² + 1.4809x³
```

For `x <= 0.20`:

```text
F(x) = FA(x)
```

For `0.20 < x < 0.30`:

```text
t = (x - 0.20) / 0.10
S(t) = 3t² - 2t³
F(x) = FA(x) + S(t) × [FB(x) - FA(x)]
```

For `x >= 0.30`:

```text
F(x) = FB(x)
```

Why it was attractive:

- preserves the very good 10–20% behavior of the original fit;
- adopts the better Cenex Southern-guideline midrange behavior;
- no second high-range transition is required because the two cubics naturally reconverge.

Why it may not be preferred for a public “one equation” model:

- a third party can reasonably ask why 20% and 30% were chosen as explicit transition boundaries.

---

## 12. Balanced / minimax cubic

```text
F(x) ≈ 0.76285x - 1.06934x² + 1.30650x³
```

Designed to distribute Cenex Southern-guideline error more evenly rather than minimize average squared error.

It is mathematically interesting but too directly optimized against approximate supplier-chart points to be the preferred general model.

---

## 13. Localized 20–60 correction

Starts with the original exact cubic and adds a correction only between 20% and 60% #1:

```text
F(x) = Fexact(x) + C(x)
```

For `0.20 < x < 0.60`:

```text
C(x) = [(x-0.20)(0.60-x)]² ×
       (509.51803202 - 2357.84006591x + 2762.39774011x²)
```

Elsewhere:

```text
C(x) = 0
```

This experiment intentionally passed very close to the approximate Cenex Southern guideline 30%, 40%, and 50% points. Its low average error is therefore not evidence that the underlying physical law is known that precisely.

---

## 14. Broad balanced localized correction

Another localized correction to the original exact model, active roughly from 20% to 75%:

```text
C(x) = [(x-0.20)(0.75-x)]² × (10.64346798 - 5.69322581x)
```

inside that interval, otherwise zero.

Purpose: spread disagreement across neighboring percentages rather than make selected chart points exact.

It achieved a small worst-case error but made some already-good points deliberately worse, so it remained diagnostic.

---

## 15. Monotonic spline / PCHIP experiments

PCHIP was tested because it:

- passes through selected anchors;
- remains monotonic when anchors are monotonic; and
- avoids high-order polynomial overshoot.

Two broad styles were tested:

- **all-knot spline** — intentionally traces the approximate supplier points; useful only to show what perfect interpolation looks like;
- **selected-anchor / production spline** — uses fewer anchors and lets the monotonic cubic interpolation determine intermediate behavior.

The restrained spline did not clearly outperform the hybrid enough to justify the additional anchor-table methodology.

---

## 16. Source-balanced cubic (early aggregation)

```text
F(x) ≈ 0.5983x - 0.7094x² + 1.1111x³
```

Produced during an early equal-source aggregation pass.

It independently landed very close to both the original cubic family and the later systematic robust cubic, which was an important stability check.

---

## 17. Hybrid-like global cubic

```text
F(x) = 0.67868x - 0.86630x² + 1.18762x³
```

Designed as one global cubic that approximates the desirable behavior of the piecewise hybrid without an explicit carve-out.

It is a strong single-formula diagnostic but was not the final winner of the broader evidence-group validation pass.

---

## 18. Systematic source-balanced cubic / systematic robust cubic

```text
F(x) = 0.583960x - 0.675308x² + 1.091348x³
```

This is the leading result of the later model-family selection exercise.

Development constraints/choices:

- exact endpoints;
- monotonic over 0–100%;
- major fitting evidence groups treated with comparable influence;
- robust loss to reduce leverage of isolated supplier-chart anomalies;
- comparison against other model families; and
- leave-one-evidence-group-out evaluation.

The result is surprisingly close to the original cubic family, but slightly more aggressive through the middle.

This is currently the strongest **single-formula research candidate** if third-party methodological simplicity is prioritized.

---

## 19. Endpoint quartic

Systematic family-test result:

```text
F(x) = 0.691190x - 1.273143x² + 2.086208x³ - 0.504255x⁴
```

It can fit the development set more flexibly, but leave-one-evidence-group-out performance was worse than the cubic.

---

## 20. Monotonic Bernstein quartic

A fourth-degree Bernstein representation was tested with control values approximately:

```text
[0, 0.157290, 0.157290, 0.389741, 1]
```

This parameterization makes monotonic constraints convenient. It generalized better than the unrestricted endpoint quartic in the project test, but still did not beat the cubic.

---

## 21. Simple rational model

```text
F(x) = x(0.348823 + 0.033029x) / (1 - 0.618148x)
```

The rational family provided a different curvature shape with only a few parameters, but its leave-one-evidence-group-out result was behind the cubic and Bernstein quartic.

---

## Naming recommendation for the future model selector

Suggested user-facing names:

- **Old Linear (3°F/10%)**
- **Cenex Literature Cubic**
- **Rounded General Cubic**
- **Original Exact Cubic**
- **Iowa Graph Curve**
- **Iowa Graph Points**
- **Cenex Southern-Guideline Test Cubic**
- **Hybrid (Research)**
- **Hybrid-Like Global Cubic**
- **Systematic Robust Cubic**

Keep localized corrections, minimax, and model-family diagnostics available in documentation rather than cluttering the primary calculator selector unless there is a specific research reason to expose them.
