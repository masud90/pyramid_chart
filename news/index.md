# Changelog

## pyramid_chart 1.2

*Released 10 July 2024; submitted to SSC.*

- New `bar1()` and `bar2()` options set the appearance of the left and
  right bars independently
  ([@friosavila](https://github.com/friosavila)).
- New `sctopt()` option passes `scatter` options to the bar labels,
  e.g. `sctopt(mlabcolor(blue))`
  ([@friosavila](https://github.com/friosavila)).
- Fixed: bar and bar-label colours can now be controlled with regular
  `graph`/`twoway` options.
- Added a Stata version check; the command is tested on Stata 17 and
  later.
- Fixed typos in the help file.

## pyramid_chart 1.1

*Released 7 July 2024.*

- First release, available from SSC with `ssc install pyramid_chart`.
