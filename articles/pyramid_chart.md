# Get started with pyramid_chart

`pyramid_chart` turns three variables into a population pyramid: a
numeric value, the categories stacked on the vertical axis, and the two
groups drawn left and right. This page walks through preparing the data
and then builds up the chart one option at a time. All code is Stata.

## Preparing the data

The command needs **long** data: one row per category × group. Stata’s
bundled `pop2000` dataset is wide (separate `maletotal` and `femtotal`
columns), so reshape it first.

``` stata
sysuse pop2000, clear
keep agegrp maletotal femtotal
rename maletotal population1
rename femtotal  population2
reshape long population, i(agegrp) j(sex)
label define sexlbl 1 "Male" 2 "Female"
label values sex sexlbl
label variable population "Population"
label variable sex "Sex"
describe
```

| Variable     | Storage | Display format | Value label | Variable label |
|--------------|---------|----------------|-------------|----------------|
| `agegrp`     | float   | `%12.0gc`      | `agelbl`    | Age group      |
| `sex`        | byte    | `%10.0g`       | `sexlbl`    | Sex            |
| `population` | float   | `%12.0gc`      |             | Population     |

Two requirements matter:

- The [`by()`](https://rdrr.io/r/base/by.html) variable must be coded
  **1** and **2**, each with a value label. Level 1 is drawn on the
  left, level 2 on the right, and the labels become the legend.
- The `over()` variable’s value labels become the vertical-axis labels,
  so label it before plotting.

Percentages are computed within each
[`by()`](https://rdrr.io/r/base/by.html) group: each bar is that
category’s share of its own group’s total.

## 1. The minimal call

Only the three variables and the number of decimals are required.

``` stata
pyramid_chart population, over(agegrp) by(sex) dec(0)
```

![](https://github.com/masud90/pyramid_chart/assets/17425770/01457933-4c31-4b28-8f71-d998fd425242)

## 2. Titles and decimals

Any `twoway` title option passes straight through. `dec(1)` adds one
decimal to the bar labels; the x-axis stays in whole numbers.

``` stata
pyramid_chart population, over(agegrp) by(sex) dec(1) ///
    title("Population Pyramid for year 2000") subtitle("in percentages") ///
    xtitle("Percentage") ytitle("Age groups")
```

![](https://github.com/masud90/pyramid_chart/assets/17425770/3e450523-751f-44e7-b38d-dffc6e0409de)

## 3. Styling the bar labels

The percentage labels are a `scatter` layer. Pass `scatter` options to
it with `sctopt()`.

``` stata
pyramid_chart population, over(agegrp) by(sex) dec(1) ///
    title("Population Pyramid for year 2000") subtitle("in percentages") ///
    xtitle("Percentage") ytitle("Age groups") ///
    sctopt(mlabcolor(blue))
```

![](https://github.com/masud90/pyramid_chart/assets/17425770/a70e4c74-3c54-4579-9b2a-a3b8a17cb06e)

## 4. Styling each side

`bar1()` and `bar2()` take bar options for the left and right bars
respectively, so the two groups can be coloured independently of the
scheme.

``` stata
pyramid_chart population, over(agegrp) by(sex) dec(1) ///
    bar1(color(navy%80)) bar2(color(orange%80)) ///
    sctopt(mlabcolor(gs4) mlabsize(small))
```

## 5. Schemes

Because everything is a regular `twoway` graph, any installed scheme
applies.

``` stata
pyramid_chart population, over(agegrp) by(sex) dec(1) ///
    title("Population Pyramid for year 2000") subtitle("in percentages") ///
    xtitle("Percentage") ytitle("Age groups") ///
    sctopt(mlabcolor(blue)) scheme(white_tableau)
```

![](https://github.com/masud90/pyramid_chart/assets/17425770/efb56726-b124-48a1-9039-8f4565ca97ff)

``` stata
pyramid_chart population, over(agegrp) by(sex) dec(1) ///
    title("Population Pyramid for year 2000") ///
    xtitle("Percentage") ytitle("Age groups") scheme(economist)
```

![](https://github.com/masud90/pyramid_chart/assets/17425770/bc965d1b-6667-4507-9e8e-64ae7d87376d)

## What you can’t override

The legend, x-axis labels and y-axis labels are generated from the data
(the x-axis is mirrored so both sides read as positive percentages), so
[`legend()`](https://rdrr.io/r/graphics/legend.html), `xlabel()` and
`ylabel()` are not intended to be changed. Everything else, including
`name()`, `saving()`, `note()`, `graphregion()` and `xsize()`/`ysize()`,
works as usual.

## Getting help

In Stata, `help pyramid_chart`. Bugs and feature requests: [GitHub
issues](https://github.com/masud90/pyramid_chart/issues).
