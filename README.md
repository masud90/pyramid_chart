![GitHub Tag](https://img.shields.io/github/v/tag/masud90/pyramid_chart?logo=github&label=latest%20version)  ![release](https://img.shields.io/github/release-date/masud90/pyramid_chart) [![Static Badge](https://img.shields.io/badge/published_on-SSC-blue?color=blue)](https://ideas.repec.org/c/boc/bocode/s459350.html) ![GitHub License](https://img.shields.io/github/license/masud90/pyramid_chart) [![X (formerly Twitter) Follow](https://img.shields.io/twitter/follow/masudtweets)](https://twitter.com/masudtweets)



# Population Pyramid Chart in Stata

This repository contains the `pyramid_chart` ado file for creating population pyramid charts in Stata. The `pyramid_chart` program allows you to create informative population pyramids, a type of graph that shows the distribution of a population by age groups and sex.

## Features

- Generates population pyramids based on numeric and categorical variables.
- Allows customization of decimal places for percentage labels.
- Supports additional `twoway` graph and `scatter` graph options for further customization.

## Installation

To install the `pyramid_chart` program, download the `pyramid_chart.ado` and `pyramid_chart.sthlp` files and place them in your Stata ado directory or your working directory. You can find or set your Stata ado path using the `adopath` command in Stata.

## Options
The `pyramid_chart` program supports additional options that can be passed to the `twoway` graph command for further customization. All options for `twoway` in stata should work, with the exception of `legend()`, `xlabel`, `ylabel` which are produced using existing labels from the variables passed into the syntax.

## Example
Here is an example of how to use the `pyramid_chart` program:

### Pre-analysis:
While reshaping is not required before producing a pyramid chart, it is important that the data is in **long format** before it is fed into the `pyramid_chart` command. As an illustration, the following code reshapes the system-included `pop2000.dta` dataset into a long dataset containing the three required variables: `population`, `sex`, and `agegrp`.

    sysuse pop2000.dta, clear
    keep agegrp maletotal femtotal
    rename maletotal population1
    rename femtotal population2
    reshape long population, i(agegrp) j(sex)
    label define sexlbl 1 "Male" 2 "Female"
    label values sex sexlbl
    label variable population "Population"
    label variable sex "Sex"
    describe

| Variable name | Storage type | Display format | Value label | Variable label |
|---------------|--------------|----------------|-------------|----------------|
|agegrp|float|%12.0gc|agelbl|Age group|
|sex|byte|%10.0g|sexlbl|Sex|
|population|float|%12.0gc|-|-|


### Example 1:
A basic pyramid chart with minimum mandatory input, and using system generated default options.

    pyramid_chart population, over(agegrp) by(sex) dec(0)

![image](https://github.com/masud90/pyramid_chart/assets/17425770/01457933-4c31-4b28-8f71-d998fd425242)


### Example 2:
Add titles to the graph, add 1 decimal place to the bar labels.

    pyramid_chart population, over(agegrp) by(sex) dec(1) title("Population Pyramid for year 2000") subtitle("in percentages") xtitle("Percentage") ytitle("Age groups")

![image](https://github.com/masud90/pyramid_chart/assets/17425770/3e450523-751f-44e7-b38d-dffc6e0409de)


### Example 3:
Add scatterplot options (for example, for color selection of bar labels).

    pyramid_chart population, over(agegrp) by(sex) dec(1) title("Population Pyramid for year 2000") subtitle("in percentages") xtitle("Percentage") ytitle("Age groups") sctopt(mlabcolor(blue)) 

![image](https://github.com/masud90/pyramid_chart/assets/17425770/a70e4c74-3c54-4579-9b2a-a3b8a17cb06e)

### Example 4:
Add various schemes to the graph.

    pyramid_chart population, over(agegrp) by(sex) dec(1) title("Population Pyramid for year 2000") subtitle("in percentages") xtitle("Percentage") ytitle("Age groups") sctopt(mlabcolor(blue)) scheme(white_tableau)

![image](https://github.com/masud90/pyramid_chart/assets/17425770/efb56726-b124-48a1-9039-8f4565ca97ff)

    pyramid_chart population, over(agegrp) by(sex) dec(1) title("Population Pyramid for year 2000") xtitle("Percentage") ytitle("Age groups") scheme(economist)

![image](https://github.com/masud90/pyramid_chart/assets/17425770/bc965d1b-6667-4507-9e8e-64ae7d87376d)



## Documentation
Detailed documentation for the `pyramid_chart` program is available in the [pyramid_chart.sthlp](https://github.com/masud90/pyramid_chart/blob/main/pyramid_chart.sthlp) file. You can access it in Stata using the help command:

        help pyramid_chart

## Version updates
- v1.2: released on July 10th, 2024. Submitted to SSC for update.
  - Bug fixed: color for bars/ bar labels can now be controlled by the user using regular `graph` / `scatter` / `twoway` options.
  - Bug fixed: typos in [`pyramid_chart.sthlp`](https://github.com/masud90/pyramid_chart/blob/main/pyramid_chart.sthlp) file fixed.
  - Feature added: now can use scatter plot options `sctopt` inside the syntax.

- v1.1: first release on July 07, 2024. Now the package is available from Stata terminal: `ssc install pyramid_chart`.

## Contributing
Contributions are welcome! If you find a bug or have a feature request, please open an issue on GitHub. If you'd like to contribute code, please fork the repository and submit a pull request.

- July 10, 2024: [Fernando Rios-Avila](https://github.com/friosavila) added version check, added dynamic color selection to the left and right bars, and added the ability to use scatter options for coloring the bar labels.

## License
This project is licensed under the MIT License. See the [LICENSE](https://github.com/masud90/pyramid_chart/blob/main/LICENSE) file for details.
