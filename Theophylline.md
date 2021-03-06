# Theophylline
Theophylline 데이타셋에서 NCA 파라메터를 계산하는 작은 예를 실행해 봅시다.
# Load the data {#load-the-data}
먼저 데이타를 살펴보겠습니다.
```
knitr::kable(head(datasets::Theoph))
```
knitr를 통해 Theoph를 좀 더 쉽게 exploartion할 수 있습니다.

| Subject | Wt | Dose | Time | conc |
| :-- | --: | --: | --: | --: |
| 1 | 79.6 | 4.02 | 0.00 | 0.74 |
| 1 | 79.6 | 4.02 | 0.25 | 2.84 |
| 1 | 79.6 | 4.02 | 0.57 | 6.57 |
| 1 | 79.6 | 4.02 | 1.12 | 10.50 |
| 1 | 79.6 | 4.02 | 2.02 | 9.66 |
| 1 | 79.6 | 4.02 | 3.82 | 8.58 |

The columns that we will be interested in for our analysis are conc, Time, and Subject in the concentration data set and Dose, Time, and Subject for the dosing data set.

```
## By default it is groupedData; convert it to a data frame for use
my.conc <- PKNCAconc(as.data.frame(datasets::Theoph), conc~Time|Subject)

## Dosing data needs to only have one row per dose, so subset for
## that first.
d.dose <- unique(datasets::Theoph[datasets::Theoph$Time == 0,
                                  c("Dose", "Time", "Subject")])
knitr::kable(d.dose,
             caption="Example dosing data extracted from theophylline data set")
```

Example dosing data extracted from theophylline data set

|  | Dose | Time | Subject |
| :-- | --: | --: | :-- |
| 1 | 4.02 | 0 | 1 |
| 12 | 4.40 | 0 | 2 |
| 23 | 4.53 | 0 | 3 |
| 34 | 4.40 | 0 | 4 |
| 45 | 5.86 | 0 | 5 |
| 56 | 4.00 | 0 | 6 |
| 67 | 4.95 | 0 | 7 |
| 78 | 4.53 | 0 | 8 |
| 89 | 3.10 | 0 | 9 |
| 100 | 5.50 | 0 | 10 |
| 111 | 4.92 | 0 | 11 |
| 122 | 5.30 | 0 | 12 |

```
my.dose <- PKNCAdose(d.dose, Dose~Time|Subject)
```

# Merge the Concentration and Dose {#merge-the-concentration-and-dose}

After loading the data, they must be combined to prepare for parameter calculation. Intervals for calculation will automatically be selected based on the `single.dose.aucs setting` in `PKNCA.options`

```
my.data.automatic <- PKNCAdata(my.conc, my.dose)
knitr::kable(PKNCA.options("single.dose.aucs"))
```

| start | end | auclast | aucall | aumclast | aumcall | cmax | cmin | tmax | tlast | tfirst | clast.obs | f | cav | ctrough | ptr | tlag | half.life | r.squared | adj.r.squared | lambda.z | lambda.z.time.first | lambda.z.n.points | clast.pred | span.ratio | aucinf | aumcinf | aucpext | cl | mrt | vss | vd | thalf.eff | kel | vz |
| --: | --: | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE |

```
knitr::kable(my.data.automatic$intervals)
```

| start | end | auclast | aucall | aumclast | aumcall | cmax | cmin | tmax | tlast | tfirst | clast.obs | f | cav | ctrough | ptr | tlag | half.life | r.squared | adj.r.squared | lambda.z | lambda.z.time.first | lambda.z.n.points | clast.pred | span.ratio | aucinf | aumcinf | aucpext | cl | mrt | vss | vd | thalf.eff | kel | vz | Subject |
| --: | --: | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 1 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 1 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 2 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 2 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 3 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 3 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 4 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 4 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 5 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 5 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 6 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 6 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 7 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 7 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 8 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 8 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 9 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 9 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 10 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 10 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 11 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 11 |
| 0 | 24 | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 12 |
| 0 | Inf | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | 12 |

Intervals for calculation can also be specified manually. Manual specification requires at least columns for `start` time, `end` time, and the parameters requested. The manual specification can also include any grouping factors from the concentration data set. Column order of the intervals is not important. When intervals are manually specified, they are expanded to the full interval set when added to a PKNCAdata object (in other words, a column is created for each parameter. Also, PKNCA automatically calculates parameters required for the NCA, so while lambda.z is required for calculating AUC<sub>0-\(\infinity\)</sub>, you do not have to specify it in the parameters requested.

```
my.intervals <- data.frame(start=0,
                           end=Inf,
                           cmax=TRUE,
                           tmax=TRUE,
                           aucinf=TRUE,
                           auclast=TRUE)
my.data.manual <- PKNCAdata(my.conc, my.dose,
                            intervals=my.intervals)
knitr::kable(my.data.manual$intervals)
```

| start | end | auclast | aucall | aumclast | aumcall | cmax | cmin | tmax | tlast | tfirst | clast.obs | f | cav | ctrough | ptr | tlag | half.life | r.squared | adj.r.squared | lambda.z | lambda.z.time.first | lambda.z.n.points | clast.pred | span.ratio | aucinf | aumcinf | aucpext | cl | mrt | vss | vd | thalf.eff | kel | vz |
| --: | --: | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | Inf | TRUE | FALSE | FALSE | FALSE | TRUE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | TRUE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE | FALSE |

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
