# Data Sources

This project distinguishes **measured data**, **figure-derived data**, **supplier guidance**, **cold-flow literature**, and **counterexamples**. The distinction matters because those sources answer different questions and have different uncertainty.

Links were reviewed during the project through **2026-08-18**.

## 1. Minnesota petroleum-diesel cold-weather report

**Report:** Petroleum Diesel Fuel and Biodiesel Technical Cold Weather Issues  
**Organizations:** Minnesota Departments of Agriculture and Commerce / Cold Weather Technical Team  
**Source:** https://www.lrl.mn.gov/docs/2009/mandated/090515.pdf

Petroleum-diesel values used:

| Fuel | Cloud point | Pour point | CFPP |
|---|---:|---:|---:|
| 100% #2 ULSD | +2°F | -22°F | -6°F |
| 50% #1 / 50% #2 | -11°F | -38°F | -20°F |
| 100% #1 ULSD | -52°F | -60°F | -53°F |

Normalized 50/50 cloud-point drop:

```text
F = (2 - (-11)) / (2 - (-52))
  = 13 / 54
  ≈ 0.24074
```

This is one of the strongest direct checks because both neat fuels and the 50/50 blend appear in the same petroleum-diesel dataset.

---

## 2. Iowa Central Fuel Testing Laboratory petroleum #1/#2 curve

**Source:** https://ifl.iowacentral.edu/reports/Final_Database_Report_Updated_LTFT.pdf

Measured starting fuels:

| Fuel | Reported CP | In-house tested CP |
|---|---:|---:|
| #1 ULSD | -49.9°F | -53.5°F |
| #2 ULSD | +4.1°F | +3.2°F |
| High-cloud #2 | +17.6°F | +12.2°F |

The report built a petroleum #1/#2 blend curve using the ordinary +3.2°F #2 and #1 fuel. The graph contains blends across the range in approximately 2% or 5% increments and error bars based on repeated determinations.

Explicitly useful blend levels from the report:

| #1 | Approx. CP |
|---:|---:|
| 57% | -15°F |
| 80% | -30°F |
| 90% | -40°F |

Normalized values using +3.2/-53.5°F endpoints:

```text
57% -> F ≈ 0.32099
80% -> F ≈ 0.58554
90% -> F ≈ 0.76190
```

### Iowa graph-derived project fits

The project later digitized the published graph in two different ways:

- a fit to the **displayed fitted curve**;
- a fit to the **displayed plotted points**.

Those project-derived equations are documented in [MODEL-CATALOG.md](MODEL-CATALOG.md). They are not equations published by Iowa Central.

### Important high-cloud #2 limitation

The report's high-cloud #2 sample tested +12.2°F, but the report did **not** publish a complete #1/high-cloud-#2 blend curve. The calculator's +14°F summer-#2 use case therefore remains a normalized extrapolation rather than a direct reproduction of an Iowa +14°F experiment.

---

## 3. Cenex / CHS cold-weather guidance

### Cenex — cold-weather diesel guidance

https://www.cenex.com/expert-advice-and-insights/cold-weather-diesel-problems

Project-relevant guidance includes:

- approximately +14°F as a typical #2 cloud-point reference in the winter-blending discussion;
- roughly 3°F cloud-point reduction for each 10% #1 added as a rule of thumb;
- the importance of accounting for the existing tank heel; and
- use of winterized products / #1 as temperatures become more severe.

This is operational supplier guidance rather than a controlled published blend experiment.

### Cenex winter-fuels guides

2023-style Cenex-branded guide used during research:  
https://inetsgi.com/customer/608/7d014d1d.pdf

Older/alternate guide copy:  
https://inetsgi.com/customer/608/a6de404c.pdf

2022 cooperative-hosted copy:  
https://www.rivercountrycoop.com/wp-content/uploads/2021/11/2022-Winter-Fuels-Products-and-Best-Practices-for-Handling-Guider.pdf

Project-relevant guidance:

- southern-tier typical #2 around +14°F;
- northern-tier typical #2 around +6°F;
- southern-tier typical #1 around -40°F;
- northern-tier typical #1 around -60°F;
- #1/Y-grade often described as -40°F or lower;
- approximately 3°F CP reduction per 10% #1;
- example of +10°F #2 -> 50/50 blend -> about -5°F;
- Wintermaster described around 70% #1 / 30% #2, with published specifications of -20°F cloud point, -37°F CFPP, and -30°F operability;
- CFI changes wax behavior rather than cloud point;
- explicit guidance not to rely on CFI to extend operability more than 15°F below cloud point; and
- blending/treating should occur while fuel is sufficiently above cloud point.

The project uses the north/south values primarily for **normalization and endpoint sensitivity**, not as proof that all Cenex fuel follows one exact curve.

---

## 4. Cenex Southern Region guidance

Agtegra-hosted fuel page:  
https://www.agtegra.com/products-services/energy/fuel

Published Southern Region chart image used during the project:  
https://storageatlasengagepdcus.blob.core.windows.net/atlas/all-media/agtegra/images/energy/winterfuels_blending_fy24_cenex_final-for-agtegra.jpg

Agtegra states that its premium fuels are supplied through Cenex/CHS channels. In this repository, **Cenex/CHS is treated as the origin/provenance of the Southern Region fuel guidance; Agtegra is cited as the host/location where this Cenex material was obtained.** The chart is therefore not counted as an independent Agtegra data source.

Approximate cloud-point points read from the chart:

| #1 | Approx. CP |
|---:|---:|
| 0% | +14°F |
| 10% | +11°F |
| 20% | +8°F |
| 30% | +3°F |
| 40% | +2°F |
| 50% | -2°F |
| 60% | -8°F |
| 70% | -14°F |
| 80% | -20°F |
| 90% | -36°F |
| 100% | -40°F |

The graphic explicitly presents itself as a **guideline**. These values are therefore treated as approximate visual readings.

The chart is especially useful because it provides a supplier-oriented shape across the full 0–100% range, but the unusually large 80→90% drop is intentionally not treated as an exact laboratory constraint.

The Agtegra-hosted Cenex material also describes roughly **10–12°F additional operability** from CFI in a 50/50 example and warns that extra additive does not necessarily produce extra benefit.

---

## 5. Cenex current product pages / product-data context

Cenex winterized premium diesel:  
https://www.cenex.com/our-products/fuels/premium-diesel-fuels/winterized-premium-diesel

Roadmaster XL:  
https://www.cenex.com/our-products/fuels/premium-diesel-fuels/cenex-roadmaster-xl

Ruby Fieldmaster:  
https://www.cenex.com/our-products/fuels/premium-diesel-fuels/cenex-ruby-fieldmaster

These pages are useful for current product context, but product specifications can change and should be rechecked before relying on a specific value in a production deployment.

---

## 6. Shrestha, Van Gerpen & Thompson — additive effectiveness

**Paper:** Effectiveness of Cold Flow Additives on Various Biodiesels, Diesel, and Their Blends  
**Transactions of the ASABE 51(4), 1365–1370 (2008)**  
**DOI:** https://doi.org/10.13031/2013.25219  
**ASABE:** https://elibrary.asabe.org/abstract.asp?aid=25219

A copy of this paper was reviewed during project research.

Key project-relevant findings:

- additives had little/statistically insignificant effect on cloud point of the tested petroleum diesel;
- all tested additives substantially reduced petroleum-diesel pour point at recommended loading;
- additive response varied by fuel/feedstock;
- benefits did not continue proportionally at higher loading; and
- no added benefit was observed beyond 200% loading in the summarized study results.

This strongly argues against a calculator assumption of linear “more ounces = proportionally more °F” response.

---

## 7. Chiu, Schumacher & Suppes — petroleum #1 and mixed-fuel LTFT response

**Paper:** Impact of cold flow improvers on soybean biodiesel blend  
**Biomass and Bioenergy 27 (2004), 485–491**  
**DOI:** https://doi.org/10.1016/j.biombioe.2004.04.006  
**Public author copy / page used during research:** https://www.researchgate.net/publication/248353607_Impact_of_cold_flow_improvers_on_soybean_biodiesel_blend

Table 3 is especially useful because it includes petroleum Diesel No. 1 and a 60% #1 / 40% #2 petroleum mixture in low-biodiesel blends.

For neat Diesel No. 1 in the table:

```text
untreated: CP -26°C, PP -42°C, LTFT -35°C
Bio Flow-875 treated: CP -26°C, PP -54°C, LTFT -39°C
```

That example shows:

- no CP change;
- substantial PP change; and
- a measurable 4°C LTFT improvement even in straight #1.

The mixed 60/40 #1/#2 rows show larger LTFT response in that study, reinforcing that CFI benefit depends on the fuel composition rather than only the nominal cloud point.

This is not a Cenex-product test and should not be converted directly into a Cenex-specific temperature allowance.

---

## 8. Reddy & McMillan — why CFI response varies

**Paper:** Understanding the Effectiveness of Diesel Fuel Flow Improvers  
**SAE 811181 (1981)**  
**DOI:** https://doi.org/10.4271/811181  
**SAE:** https://saemobilus.sae.org/papers/understanding-effectiveness-diesel-fuel-flow-improvers-811181

Key finding used by the project:

> flow improver effectiveness depends on additive concentration as well as fuel n-paraffin concentration and distribution.

This is one of the strongest mechanistic reasons not to model CFI solely as a function of percent #1.

---

## 9. Chandler, Horneck & Brown — additive classes and operability

**Paper:** The Effect of Cold Flow Additives on Low Temperature Operability of Diesel Fuels  
**SAE 922186 (1992)**  
**DOI:** https://doi.org/10.4271/922186  
**SAE:** https://saemobilus.sae.org/papers/effect-cold-flow-additives-low-temperature-operability-diesel-fuels-922186

The paper distinguishes pour-point depressants, cloud-point depressants, and operability additives and discusses why low-temperature operability cannot be reduced to one generic additive claim.

---

## 10. Botros — wax-crystal behavior

**Paper:** Enhancing the Cold Flow Behavior of Diesel Fuels  
**SAE 972899 (1997)**  
**SAE:** https://saemobilus.sae.org/papers/enhancing-cold-flow-behavior-diesel-fuels-972899

Low-temperature microscopy work provides useful physical context: untreated wax crystals can grow into a network, while cold-flow improvers can alter crystal size/shape and therefore flow/filterability behavior below cloud point.

---

## 11. CRC Report 673 — wax composition and CFI limitations

**Report:** Renewable Hydrocarbon Diesel Fuel Properties and Performance Review  
**CRC Report No. 673**  
https://crcao.org/wp-content/uploads/2019/05/CRC-673.pdf

Relevant conclusions:

- diesel low-temperature operability depends on viscosity and precipitated n-paraffin wax;
- wax solubility depends on total wax, wax-size distribution, and the surrounding fuel matrix;
- CFI additives co-crystallize with wax and alter size/shape; and
- increasing quantities of wax, especially concentrated in similar chain lengths, can reduce the effectiveness of traditional CFI chemistry.

This supports treating CFI response as **fuel-composition-specific**.

---

## 12. CRC Report 650 — specialty winter/arctic counterexample

https://crcao.org/wp-content/uploads/2019/05/CRC-650.pdf

This study used already-severe winter/arctic petroleum fuels with much colder #2 starting points than the calculator's summer-heel use case.

The normalized 50/50 behavior differs from the Minnesota/Iowa pattern. It is intentionally retained as evidence that there is **no universal endpoint-normalized #1/#2 curve for every petroleum stream**.

---

## 13. Older NBB / training material counterexample

https://p2infohouse.org/ref/37/36045.pdf

An older low-sulfur diesel example discussed during research used roughly:

```text
#2 ≈ +4°F
#1 ≈ -54°F
50/50 ≈ -6°F
```

That midpoint is warmer than the Minnesota/Iowa-derived general curve would predict. It is useful as a reminder that blend shape varies and that the project should publish an uncertainty/range discussion rather than imply one universal law.

---

## 14. Additional research links

A plain one-link-per-line list is stored at:

[`Docs/Data/source-links.txt`](Data/source-links.txt)

That list also contains patents and additional cold-flow/model-background sources investigated during the project. Inclusion in the list does **not** mean a source was used to fit the current cloud-point model.
