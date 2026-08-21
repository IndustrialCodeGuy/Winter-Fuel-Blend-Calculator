# Limitations and Warnings

This calculator and the research models are planning/learning tools. They are not a laboratory method, fuel specification, engineering guarantee, or additive-performance warranty.

## 1\. No single normalized blend curve is universal

The project has strong evidence for nonlinear ordinary #1/#2 ULSD blending, but refinery stream and wax composition matter.

Specialty winter/arctic datasets such as CRC Report 650 do not follow the same normalized shape as the Minnesota/Iowa summer-#2-to-#1 use case.

The selected formula therefore represents a **useful empirical family**, not a physical law.

## 2\. Endpoint cloud points do not uniquely determine blend behavior

Two fuels can share similar neat-fuel CP values and still have different wax distributions and blend response.

The advanced CP inputs improve localization, but they cannot encode full fuel composition.

## 3\. +14°F is an intended-use default, not every #2 fuel

The calculator's winterization use case often begins with a summer #2 heel. +14°F is a useful Cenex/southern-tier style reference for that condition.

Winterized #2 or northern-tier fuel can be much colder.

## 4\. The #1 endpoint can vary materially

Project sources include #1 values from roughly -40°F guidance to measured values near -52/-53.5°F and northern-tier guidance around -60°F.

Actual supplier or certificate-of-analysis values should override a generic default.

## 5\. The 90% Cenex Southern guideline point is not treated as an exact laboratory constraint

The published Southern Region guideline approximately shows a very large 80→90% temperature drop followed by a much smaller 90→100% change.

Many smooth independently derived formulas do not reproduce that local shape. The project therefore retains the point as supplier guidance but prevents it from dominating general model fitting.

## 6\. Graph digitization is approximate

The Iowa graph-derived models and Cenex Southern guideline point readings from the Agtegra-hosted chart were produced from published figures rather than original numeric datasets.

Digitized values should be used for shape comparison and hypothesis testing, not represented as exact source tables.

## 7\. The model selector will show alternatives, not competing guarantees

If the experimental calculator allows switching models, a colder result does not mean that model is “better.”

Each model should be labeled by provenance and purpose.

## 8\. The gallons solver assumes straight #1 is added

The primary blend-ratio equation assumes the added product is 100% #1.

A preblended product such as Wintermaster or another 70/30 blend changes both #1 and #2 gallons and requires separate mass-balance logic.

Do not interpret “required #1 gallons” as “required gallons of any winter fuel.”

## 9\. Existing #1 and added #1 may not average perfectly

The current implementation can volume-weight two different #1 endpoint CP values before evaluating the final normalized #1/#2 curve.

That is convenient but does not capture all composition interactions between two different #1 fuels.

## 10\. Biodiesel is not separately modeled

Biodiesel can change CP, PP, CFPP/LTFT, and additive response.

Unless the calculator explicitly asks for biodiesel percentage and uses supporting data, its results should not be treated as a biodiesel-specific prediction.

## 11\. Cloud point is not equipment failure temperature

Cloud point indicates visible wax under a standardized method.

Actual equipment can fail warmer or operate colder depending on:

* filter pore size and loading;
* return-fuel heating;
* fuel-system design;
* cooling rate/history;
* tank geometry;
* water/ice;
* sediment/contamination; and
* wax settling.

## 12\. Estimated operability is not CFPP or LTFT

The calculator's estimated-operability value is a planning estimate.

It does not reproduce ASTM/CGSB test procedures and should not be labeled as a measured CFPP or LTFT.

## 13\. CFI response is fuel- and product-specific

Cold-flow improver response depends on additive chemistry, concentration, fuel n-paraffin concentration/distribution, and other composition factors.

Straight #1 can show residual benefit in some tests, while other supplier guidance suggests little incremental benefit near neat #1. Both can be true for different fuel/additive combinations.

## 14\. Do not scale additive benefit linearly with dose

Multiple sources show that increasing additive treatment does not produce a universal proportional temperature improvement.

A future production implementation should use dose to calculate **quantity required at the selected treat rate**, not multiply a temperature benefit by dose without product-specific evidence.

## 15\. Blend/treatment temperature matters

Fuel that is already at or below its cloud point may not mix or respond to treatment as intended. Supplier guidance commonly emphasizes blending/treating while fuel is sufficiently above CP.

The calculator does not know actual tank temperature unless that feature is explicitly added.

## 16\. Storage and contamination are outside the model

The calculation does not model:

* water/ice;
* microbes;
* rust/sediment;
* oxidation/old fuel;
* wax settling;
* temperature stratification;
* incomplete mixing; or
* contaminated/plugged filters.

These can dominate real-world cold-weather problems.

## 17\. Use actual data when available

Preferred order for real operational decisions:

1. actual batch/terminal/supplier specifications;
2. actual laboratory fuel testing;
3. product-specific additive instructions/test data;
4. local historical operating experience; and
5. calculator estimates as a planning aid.

## 18\. No guarantee of safety, suitability, or performance

The software and research results are provided for general informational, educational, and planning purposes. Actual results can vary materially for reasons the calculator cannot observe.

Do not rely on these estimates as the sole basis for purchasing, blending, treating, storing, or operating with fuel.

To the fullest extent permitted by applicable law, the software and its results are provided **as is**, without warranties of accuracy, completeness, merchantability, fitness for a particular purpose, or particular performance. The repository license controls the software license terms.

