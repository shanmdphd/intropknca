# Compute the parameters {#compute-the-parameters}

Parameter calculation will automatically split the data by the grouping factor(s), subset by the interval, calculate all required parameters, record all options used for the calculations, and include data provenance to show that the calculation was performed as described. For all this, just call the `pk.nca` function with your PKNCAdata object.

```
my.results.automatic <- pk.nca(my.data.automatic)
knitr::kable(head(my.results.automatic$result))
```

| start | end | Subject | PPTESTCD | PPORRES |
| --: | --: | :-- | :-- | --: |
| 0 | 24 | 1 | auclast | 92.3654416 |
| 0 | Inf | 1 | cmax | 10.5000000 |
| 0 | Inf | 1 | tmax | 1.1200000 |
| 0 | Inf | 1 | tlast | 24.3700000 |
| 0 | Inf | 1 | lambda.z | 0.0484570 |
| 0 | Inf | 1 | r.squared | 0.9999997 |

```
summary(my.results.automatic)
```

| start | end | auclast | cmax | tmax | half.life | aucinf |
| --: | --: | :-- | :-- | :-- | :-- | :-- |
| 0 | 24 | 74.6 [24.3] | . | . | . | . |
| 0 | Inf | . | 8.65 [17.0] | 1.14 [0.630, 3.55] | 8.18 [2.12] | 115 [28.4] |

```
my.results.manual <- pk.nca(my.data.manual)
knitr::kable(head(my.results.manual$result))
```

| start | end | Subject | PPTESTCD | PPORRES |
| --: | --: | :-- | :-- | --: |
| 0 | Inf | 1 | auclast | 147.2347485 |
| 0 | Inf | 1 | cmax | 10.5000000 |
| 0 | Inf | 1 | tmax | 1.1200000 |
| 0 | Inf | 1 | tlast | 24.3700000 |
| 0 | Inf | 1 | lambda.z | 0.0484570 |
| 0 | Inf | 1 | r.squared | 0.9999997 |

```
summary(my.results.manual)
```

| start | end | auclast | cmax | tmax | aucinf |
| --: | --: | :-- | :-- | :-- | :-- |
| 0 | Inf | 98.7 [22.5] | 8.65 [17.0] | 1.14 [0.630, 3.55] | 115 [28.4] |

**Illegal HTML tag removed :** // add bootstrap table styles to pandoc tables $(document).ready(function () { $('tr.header').parent('thead').parent('table').addClass('table table-condensed'); });

**Illegal HTML tag removed :** (function () { var script = document.createElement("script"); script.type = "text/javascript"; script.src = "https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"; document.getElementsByTagName("head")[0].appendChild(script); })();