# Learning Guide: Reading the Blend Models

This page is for readers who want to understand what the equations are doing rather than simply use the calculator.

## 1. The most important idea: normalized response

A cloud-point model in this project normally has two layers:

```text
1. F(x) says how far through the #2-to-#1 cloud-point span the blend has moved.
2. The entered CP₂ and CP₁ values turn that normalized fraction into °F.
```

Example:

```text
CP₂ = +14°F
CP₁ = -45°F
span = 59°F
```

If a model gives:

```text
F(0.50) = 0.25
```

then the 50/50 blend is one-quarter of the way through the 59°F endpoint drop:

```text
CP = 14 - 59 × 0.25
   = -0.75°F
```

A 50/50 **volume blend does not have to be the temperature midpoint**.

## 2. Why the old linear model was deceptive

The old model used:

```text
CP₂ = +14°F
CP₁ = -16°F
```

and straight interpolation.

That made each 10% #1 step exactly 3°F, which looked good next to Cenex's rule of thumb.

But replacing -16°F with a realistic -50-ish°F #1 endpoint while keeping the same straight line makes the midrange much too cold compared with Minnesota/Iowa measurements.

The correct lesson was therefore:

> the 3°F rule can be useful over part of the range without implying that the entire blend curve is linear to neat #1.

### What the error-gap chart says about the 3°F rule

The useful signal in an error-gap chart is not only the distance from zero; it is also the **slope of the error line**.

If a model's error stays roughly flat, its rate of cloud-point change is tracking the reference at about the same rate. If its error begins to tilt steadily upward, the model is becoming progressively warmer than the reference because its cloud-point reduction is no longer keeping up.

That is what happens to the fixed 3°F-per-10% rule. The nonlinear model family has a shallow early response, reaches roughly the same local rate as the 3°F rule around the mid-30s to low-40s percent #1, and then continues to steepen. The linear rule cannot steepen; its slope is fixed forever.

So the apparent separation near 40% on a 10%-interval chart should not be read as a physical cutoff at exactly 40%. It is better understood as the point where the general nonlinear family begins to **systematically outrun the fixed 3°F-per-10% rate**. The exact visual crossing is partly an artifact of sampling at 10% intervals and of the irregular 30%/40% Cenex Southern guideline points.

This behavior is not unique to the Cenex Southern Region comparison. The Iowa-derived curve and point fits also steepen beyond the low-percentage range. That cross-source pattern is one reason the 3°F rule is treated as a useful low-to-moderate blend check rather than a full-range blending law.

## 3. Changing endpoints compresses or expands the curve

Suppose the normalized formula is unchanged and #2 remains +14°F.

Changing #1 from -55°F to -40°F does not “cut off” the curve. The same normalized shape is compressed vertically from a 69°F span to a 54°F span.

The effect is small in #2-heavy blends and larger near neat #1 because `F(x)` is still small near the left side and approaches 1 near the right side.

## 4. Why several cubics look so similar

Most project cubics satisfy:

```text
F(0)=0
F(1)=1
```

That forces them to start and end at exactly the same places.

The difference between two endpoint-constrained cubics can only form a fairly broad middle-shape deviation; it cannot independently move every 10% point wherever desired.

This is why improving a mismatch near 30% often shifts 20%, 40%, or 50% too.

## 5. Why 30% kept attracting attention

The Cenex Southern Region guideline approximately does this:

```text
20% -> +8°F
30% -> +3°F
40% -> +2°F
```

That is a large drop into 30% followed by a very small drop into 40%.

At the same time, many project cubics have their inflection region around 20–30% #1.

The combination makes 30% a natural place for model disagreement.

## 6. Why 60–70% often remains warm, then 80% catches up

The Cenex Southern Region guideline is almost a straight -6°F-per-10% sequence from about 50–80%.

The cubic family is not straight there; it is still accelerating. Many models therefore lag the supplier curve around 60–70% and then catch back up by about 80%.

## 7. Why the 90% Cenex Southern guideline point is unusual

Approximate Cenex Southern guideline changes:

```text
80 -> 90: -20 to -36°F  = 16°F drop
90 -> 100: -36 to -40°F = 4°F drop
```

A smooth global cubic has difficulty reproducing a cliff followed immediately by flattening without distorting the rest of the curve.

This is why many independent formulas cluster around a similar error at 90%.

That agreement among the formulas is information: it suggests the 90% supplier-graphic point should not automatically receive enough leverage to reshape the whole model.

## 8. What the Iowa graph-derived formulas teach us

The two Iowa-derived graph formulas are generally warmer through the middle than the original project model.

The Cenex Southern-guideline-oriented formulas are generally colder through parts of the middle.

The original cubic family sits between those sources surprisingly often.

That is a stronger argument for a cross-evidence model than simply asking which equation has the smallest error against one chart.

## 9. Why a piecewise hybrid can be numerically excellent but harder to trust

The hybrid says, in effect:

```text
use behavior A here
transition here
use behavior B there
```

There is nothing mathematically invalid about that, and it fit the chosen evidence well.

But a third party may reasonably ask:

```text
Why exactly 20%?
Why exactly 30%?
```

A single global cubic can be slightly less accurate against a particular chart but easier to audit and defend.

## 10. Why a spline is not automatically more scientific

A monotonic spline can pass through every plotted supplier point exactly.

That only proves the interpolation algorithm can connect the points. It does not prove the supplier graphic is an exact physical dataset.

More flexibility is useful when the data justify it. It becomes overfitting when the extra flexibility mostly traces approximate pixels.

## 11. Why leave-one-evidence-group-out testing matters

If a model is fit to Minnesota, Iowa, Cenex general guidance, and the Cenex Southern guideline simultaneously, a good fit can simply mean it memorized all four fitting evidence groups. The two Cenex groups are useful for different constraints, but they are not independent supplier provenance.

Leave-one-evidence-group-out asks a harder question:

> If the model never saw one entire fitting evidence group, how well would the other sources predict it?

In the project's systematic test, the endpoint-constrained cubic beat the more flexible quartic and rational families on this criterion.

That is one of the strongest reasons the project currently favors cubic simplicity.

## 12. What model switching should teach the user

A future experimental selector should help the user see that:

- endpoint assumptions matter;
- model shape matters;
- different credible sources do not produce identical curves;
- lower temperature is not automatically “more correct”;
- supplier guidance may be more relevant for a particular supply chain than a source-neutral research curve; and
- actual fuel testing should override generic model assumptions when available.
