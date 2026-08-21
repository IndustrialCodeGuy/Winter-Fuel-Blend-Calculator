# Cold-Flow Improver Research

Cloud-point research and cold-flow-improver (CFI) research are related but must not be treated as the same problem.

## 1. The four temperatures/ideas that are easy to confuse

### Cloud point (CP)

The temperature where visible wax first appears under the specified test method.

### Pour point (PP)

A standardized measure related to when the bulk fuel stops showing movement under the test conditions.

### Cold Filter Plugging Point (CFPP)

A standardized filterability test used widely outside North America and in some diesel/biodiesel work.

### LTFT / actual operability

The Low-Temperature Flow Test and real equipment operation address whether fuel can still move through a fuel system/filter below cloud point. Vehicle/equipment design matters.

The calculator's output is intentionally called **Estimated operability**, not CFPP.

## 2. CFI usually does not lower cloud point

Several independent sources converge here.

Shrestha, Van Gerpen & Thompson (2008) found no statistically significant cloud-point depression in the tested petroleum diesel from the additives, while pour point was strongly affected.

Chiu, Schumacher & Suppes (2004) Table 3 shows neat Diesel No. 1 remaining at -26°C CP with and without Bio Flow-875, while PP and LTFT improved.

Cenex guidance likewise describes CFI as changing wax behavior rather than reducing CP.

For the calculator, the implication is straightforward:

```text
cloud-point model first
cold-flow / operability allowance second
```

Do not subtract additive effect from cloud point and then label that result “new cloud point.”

## 3. Neat #1 can still respond to an additive

One question raised during the project was whether CFI has essentially zero effect on straight #1.

The Cenex Southern chart visually suggests that cloud point and typical operability converge at very high #1 percentages, which could be read as little/no practical extra CFI benefit near neat #1.

However, Chiu et al. provide an important counterexample in Table 3:

```text
Diesel No. 1, untreated:
CP   -26°C
PP   -42°C
LTFT -35°C

Diesel No. 1, Bio Flow-875 treated:
CP   -26°C
PP   -54°C
LTFT -39°C
```

So in that fuel/additive combination:

- CP effect = 0°C;
- PP improvement = 12°C;
- LTFT improvement = 4°C (~7.2°F).

The correct conclusion is therefore **not** “CFI has zero effect on neat #1.”

A more defensible conclusion is:

> CFI benefit can persist in neat #1, but the size of the benefit is fuel/additive-specific and can differ substantially from the response of mixed #1/#2 fuel.

## 4. Mixed fuel can respond more strongly than neat #1

The same Chiu table includes 60% #1 / 40% #2 petroleum portions in low-biodiesel samples. The treated rows show much larger LTFT improvements than the neat-#1 row in that particular experiment.

This supports the qualitative shape seen in supplier guidance: CFI benefit may remain substantial through ordinary winter blends and then become less predictable as the fuel approaches neat #1.

It does **not** provide enough evidence to define a universal percentage-based taper for Cenex CFI.

## 5. Why percent #1 is only a proxy

Reddy & McMillan, SAE 811181, found that flow-improver effectiveness depends on:

- additive concentration;
- fuel n-paraffin concentration; and
- n-paraffin distribution.

CRC Report 673 adds that total wax, wax-size distribution, and surrounding fuel chemistry matter, and that narrow/high wax distributions can make traditional CFI less effective.

Therefore:

```text
CFI response != simple function of percent #1 alone
```

Percent #1 correlates with fuel composition in many ordinary blends, but it is not the underlying physical variable.

## 6. Why dose should not scale linearly with temperature benefit

The original calculator logic allowed a proportional temperature scaling such as:

```text
applied effect = entered effect × entered dose / reference dose
```

The research does not support that as a general physical relationship.

### Shrestha et al.

The study explicitly examined 100%, 200%, and 300% of recommended loading. Response plateaued, varied by fuel/additive, and the summarized results found no additional benefit above 200% loading.

### Chiu et al.

Some additive/blend combinations showed non-monotonic or inconsistent response as loading changed.

### Cenex

Supplier guidance warns that increasing treatment above the recommended rate may provide no additional operability and that excessive treatment can hurt performance/filterability.

### Production implication

A better calculator architecture is:

```text
dose -> ounces required
expected effect -> independent planning assumption at the intended supplier treat rate
```

rather than using dose as a temperature multiplier.

## 7. Is 12°F still a reasonable planning allowance?

For common treated blends, roughly 10–12°F appears consistent with Cenex guidance. The Cenex winter-fuels guide also gives an explicit upper caution: do not rely on cold-flow improvers to extend operability more than **15°F below cloud point**.

That makes **12°F** a reasonable *planning default* for a Cenex-oriented production calculator when the additive is used correctly at its intended rate.

It should not be represented as:

- a universal additive response;
- a guaranteed LTFT/CFPP improvement;
- a reason to increase treatment; or
- a fixed benefit all the way to every neat-#1 fuel.

### Wintermaster consistency check

Wintermaster gives the 12°F planning allowance a useful product-level reasonableness check. The Cenex source material used by this project describes Wintermaster as roughly 70% #1 / 30% #2 and publishes approximately -30°F operability.

Using the rounded general cloud-point model with generic +14°F #2 and -55°F #1 endpoints at 70% #1 gives approximately **-16.3°F cloud point**. Applying the calculator's **12°F** planning allowance gives approximately **-28.3°F estimated operability**, about **1.7°F warmer** than the published -30°F Wintermaster operability figure. Applying Cenex's **15°F maximum-delta guidance** as an upper comparison gives approximately -31.3°F.

This close agreement is encouraging, but it is an **external consistency check, not a calibration target**. The actual Wintermaster component cloud points and additive formulation are not known, and Cenex also publishes a Wintermaster cloud point of about -20°F rather than the -16.3°F produced by the generic endpoint assumptions. The comparison therefore supports the plausibility of a roughly 12°F planning allowance without proving that every 70/30 blend will receive that benefit.

## 8. High-#1 taper research status

Several provisional tapers were considered during the project, including:

- full effect through ~75–80% #1 followed by a smooth reduction;
- tapering to zero at neat #1 based on a literal reading of the Cenex Southern Region chart hosted by Agtegra; and
- tapering to a nonzero residual based on the Chiu neat-#1 LTFT result.

None of these is currently considered sufficiently validated as a universal formula.

The documentation therefore recommends keeping the high-#1 CFI response **experimental** until a stronger product-specific dataset is available.

## 9. Useful sources

Shrestha et al. (2008):  
https://doi.org/10.13031/2013.25219

Chiu et al. (2004):  
https://doi.org/10.1016/j.biombioe.2004.04.006

Reddy & McMillan (1981):  
https://doi.org/10.4271/811181

Chandler, Horneck & Brown (1992):  
https://doi.org/10.4271/922186

Botros (1997):  
https://saemobilus.sae.org/papers/enhancing-cold-flow-behavior-diesel-fuels-972899

CRC 673:  
https://crcao.org/wp-content/uploads/2019/05/CRC-673.pdf

Cenex guidance:  
https://www.agtegra.com/products-services/energy/fuel

## 10. Documentation rule for future CFI changes

Any future CFI model should clearly state:

- the additive/product tested;
- the treatment rate;
- fuel composition;
- untreated CP/PP/CFPP/LTFT;
- treated values;
- whether the result is a lab test, vehicle test, supplier estimate, or calculator assumption; and
- whether the model changes with #1 percentage or actual fuel chemistry.

Do not silently convert a result from one additive/fuel pair into a universal diesel rule.
