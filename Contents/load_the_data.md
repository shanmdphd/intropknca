# Theophylline

Examples simplify understanding. Below is an example of how to use the theophylline dataset to generate NCA parameters.

# Load the data {#load-the-data}

먼저 데이타를 살펴보겠습니다.
```
## It is always a good idea to look at the data
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