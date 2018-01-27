
<!-- README.md is generated from README.Rmd. Please edit that file -->
statascii
=========

Create Stata-like tables in the R console

[![CRAN status](http://www.r-pkg.org/badges/version/statascii)](https://cran.r-project.org/package=statascii)

[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)

Please note that this project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree to abide by its terms.

Installation
------------

You can install statascii from GitHub with:

``` r
# install.packages("devtools")
devtools::install_github("gvelasq2/statascii")
```

Examples
--------

``` r
# setup
library(dplyr)
#> 
#> Attaching package: 'dplyr'
#> The following objects are masked from 'package:stats':
#> 
#>     filter, lag
#> The following objects are masked from 'package:base':
#> 
#>     intersect, setdiff, setequal, union
library(stringr)

# a. demonstrate 'oneway' flavor for one-way tables of frequencies
a <- mtcars %>% count(gear) %>% rename(Freq. = n)
a <- a %>% add_row(gear = "Total", Freq. = sum(a[, 2]))
# statascii(a, flavor = "oneway")

# b. demonstrate 'oneway' flavor with no padding
b <- mtcars %>% count(gear) %>% rename(Freq. = n)
b <- b %>% add_row(gear = "Total", Freq. = sum(b[, 2]))
# statascii(b, flavor = "oneway", padding = "none")

# c. demonstrate 'twoway' flavor for n-way tables of frequencies
c <- mtcars %>% count(gear, carb, am) %>% rename(Freq. = n)
c <- c %>% ungroup() %>% add_row(gear = "Total", carb = "", am = "", Freq. = sum(c[, 4]))
# statascii(c, flavor = "twoway")

# d. demonstrate 'twoway' flavor with dashed group separators
d <- mtcars %>% count(gear, carb, am) %>% rename(Freq. = n)
d <- d %>% ungroup() %>% add_row(gear = "Total", carb = "", am = "", Freq. = sum(d[, 4]))
# statascii(d, flavor = "twoway", separators = TRUE)

# e. demonstrate 'summary' flavor for summary statistics
e <- mtcars %>% group_by(gear) %>% summarize(
  Obs = n(),
  Mean = mean(gear),
  "Std. Dev." = sd(gear),
  Min = min(gear),
  Max = max(gear)
)
# statascii(e, flavor = "summary")

# f. demonstrate wrapping feature for wide tables
f <- mtcars %>%
  mutate(cyl2 = cyl, vs2 = vs, am2 = am, carb2 = carb) %>%
  filter(gear != 5) %>%
  count(gear, carb, am, vs, cyl, carb2, am2, vs2, cyl2) %>%
  rename(Freq. = n) %>%
  ungroup()
f <- f %>% add_row(gear = "Total", Freq. = sum(f[, 10]))
f[is.na(f)] <- ""
# statascii(f, flavor = "oneway", separators = TRUE)
```

Reference
---------

`statascii()` borrows heavily from `asciify()`.

The `asciify()` function was written by @gavinsimpson in StackOverflow (<https://stackoverflow.com/questions/13011383>) and GitHub Gist (<https://gist.github.com/gavinsimpson>).

The `statascii()` function was first written by @gvelasq2 in Github (<https://github.com/gvelasq2/statascii>) and Github Gist (<https://gist.github.com/gvelasq2>).
