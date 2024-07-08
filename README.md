![GitHub Tag](https://img.shields.io/github/v/tag/masud90/pyramid_chart?logo=github&label=latest%20version)  ![release](https://img.shields.io/github/release-date/masud90/pyramid_chart) ![Static Badge](https://img.shields.io/badge/published_on-SSC-blue?color=blue&link=https%3A%2F%2Fideas.repec.org%2Fc%2Fboc%2Fbocode%2Fs459350.html)


# Population Pyramid Chart in Stata

This repository contains the `pyramid_chart` ado file for creating population pyramid charts in Stata. The `pyramid_chart` program allows you to create informative population pyramids, a type of graph that shows the distribution of a population by age groups and sex.

## Features

- Generates population pyramids based on numeric and categorical variables.
- Allows customization of decimal places for percentage labels.
- Supports additional `twoway` graph options for further customization.

## Installation

To install the `pyramid_chart` program, download the `pyramid_chart.ado` and `pyramid_chart.sthlp` files and place them in your Stata ado directory or your working directory. You can find or set your Stata ado path using the `adopath` command in Stata.

## Options
The `pyramid_chart` program supports additional options that can be passed to the `twoway` graph command for further customization. All options for `twoway` in stata should work, with the exception of `legend()`, `xlabel`, `ylabel` which are produced using existing labels from the variables passed into the syntax.

## Example
Here is an example of how to use the `pyramid_chart` program:

### Example 1:
The following example reshapes the dataset into a long dataset containing the three required variables: population, sex, and agegrp.

         sysuse pop2000.dta, clear
         keep agegrp maletotal femtotal
         rename maletotal population1
         rename femtotal population2
         reshape long population, i(agegrp) j(sex)
         label define sexlbl 1 "Male" 2 "Female"
         label values sex sexlbl
         label variable population "Population"
         label variable sex "Sex"
         pyramid_chart population, over(agegrp) by(sex) dec(0)

### Example 2:
The following example reshapes the dataset into a long dataset containing the three required variables: population, sex, and agegrp, and then adds some options from the twoway_options for additional formatting.

         sysuse pop2000.dta, clear
         keep agegrp maletotal femtotal
         rename maletotal population1
         rename femtotal population2
         reshape long population, i(agegrp) j(sex)
         label define sexlbl 1 "Male" 2 "Female"
         label values sex sexlbl
         label variable population "Population"
         label variable sex "Sex"
         pyramid_chart population, over(agegrp) by(sex) dec(1) title("Population Pyramid for year 2000") subtitle("in percentages") xtitle("Percentage") ytitle("Age groups")


## Documentation
Detailed documentation for the `pyramid_chart` program is available in the [pyramid_chart.sthlp](https://github.com/masud90/pyramid_chart/blob/main/pyramid_chart.sthlp) file. You can access it in Stata using the help command:

        help pyramid_chart

## Contributing
Contributions are welcome! If you find a bug or have a feature request, please open an issue on GitHub. If you'd like to contribute code, please fork the repository and submit a pull request.

## License
This project is licensed under the MIT License. See the [LICENSE](https://github.com/masud90/pyramid_chart/blob/main/LICENSE) file for details.
