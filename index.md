# pyramid_chart

`pyramid_chart` is a Stata command that draws a **population pyramid**:
a back-to-back horizontal bar chart showing how a population is
distributed across age groups (or any ordered category) for two groups,
typically male and female. Bars show each category’s share of its
group’s total, labelled with percentages, and the axes, legend and
labels are built for you from the variables’ value labels.

![Population pyramid for the United States, 2000, drawn with
pyramid_chart](https://github.com/masud90/pyramid_chart/assets/17425770/3e450523-751f-44e7-b38d-dffc6e0409de)

Population pyramid for the United States, 2000, drawn with pyramid_chart

## Installation

From [SSC](https://ideas.repec.org/c/boc/bocode/s459350.html):

``` stata
ssc install pyramid_chart, replace
```

Or manually: download
[`pyramid_chart.ado`](https://github.com/masud90/pyramid_chart/blob/main/pyramid_chart.ado)
and
[`pyramid_chart.sthlp`](https://github.com/masud90/pyramid_chart/blob/main/pyramid_chart.sthlp)
into your working directory or a folder on your `adopath`.

## Quick start

The data must be in **long** format: one row per category × group, with
a numeric value, a categorical `over()` variable, and a two-level
[`by()`](https://rdrr.io/r/base/by.html) variable coded 1 and 2 with
value labels.

``` stata
sysuse pop2000, clear
keep agegrp maletotal femtotal
rename (maletotal femtotal) (population1 population2)
reshape long population, i(agegrp) j(sex)
label define sexlbl 1 "Male" 2 "Female"
label values sex sexlbl

pyramid_chart population, over(agegrp) by(sex) dec(1) ///
    title("Population Pyramid for year 2000") subtitle("in percentages") ///
    xtitle("Percentage") ytitle("Age groups")
```

## Syntax

``` stata
pyramid_chart varname [if] [in], over(varname) by(varname) dec(#) ///
    [bar1(barlook_options) bar2(barlook_options) sctopt(scatter_options) twoway_options]
```

| Option | Description |
|----|----|
| `over(varname)` | Categorical variable on the vertical axis (e.g. age group). Its value labels become the axis labels. |
| `by(varname)` | Two-level variable splitting the pyramid: level **1** is drawn left, level **2** right. Value labels feed the legend. |
| `dec(#)` | Decimal places for the percentage labels on the bars. |
| `bar1()`, `bar2()` | Appearance of the left and right bars, e.g. `bar1(color(navy)) bar2(color(orange))`. |
| `sctopt()` | Options for the bar labels, e.g. `sctopt(mlabcolor(blue) mlabsize(small))`. |
| *twoway_options* | Any other `twoway` option: [`title()`](https://rdrr.io/r/graphics/title.html), `subtitle()`, `xtitle()`, `ytitle()`, `scheme()`, `note()`, `name()`, … |

The legend and both axis labels are generated from the data, so
[`legend()`](https://rdrr.io/r/graphics/legend.html), `xlabel()` and
`ylabel()` are not meant to be overridden.

## Examples

Full worked examples, with output, are on the [**package
website**](https://masud90.github.io/pyramid_chart/articles/pyramid_chart.html),
and in Stata via `help pyramid_chart`.

| Default | Custom bar-label colour | With a scheme |
|:--:|:--:|:--:|
| ![Default output](https://github.com/masud90/pyramid_chart/assets/17425770/01457933-4c31-4b28-8f71-d998fd425242) | ![Blue bar labels](https://github.com/masud90/pyramid_chart/assets/17425770/a70e4c74-3c54-4579-9b2a-a3b8a17cb06e) | ![economist scheme](https://github.com/masud90/pyramid_chart/assets/17425770/bc965d1b-6667-4507-9e8e-64ae7d87376d) |

## Citation

> Rahman, M. (2024). *PYRAMID_CHART: Stata module to generate a
> population pyramid chart.* Statistical Software Components S459350,
> Boston College Department of Economics.
> <https://ideas.repec.org/c/boc/bocode/s459350.html>

## Contributing

Bug reports and feature requests are welcome as
[issues](https://github.com/masud90/pyramid_chart/issues); code
contributions as pull requests. Thanks to [Fernando
Rios-Avila](https://github.com/friosavila) for the version check,
per-side bar colours and scatter-label options in v1.2.

## License

[MIT](https://github.com/masud90/pyramid_chart/blob/main/LICENSE) ©
Masud Rahman
