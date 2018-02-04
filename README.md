
<!-- README.md is generated from README.Rmd. Please edit that file -->

[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)

# dockerfiler

Easy creation of Dockerfiles.

## Installation

You can install dockerfiler from GitHub with:

``` r
# install.packages("devtools")
devtools::install_github("colinfay/dockerfiler")
```

## Basic worflow

By default, Dockerfiles are created with `FROM "rocker/r-base"`. You can
set another FROM in `new()`

``` r
library(dockerfiler)
# Create a dockerfile template
my_dock <- Dockerfile$new()
```

Wrap your R Code with the r\_this() function to turn it into a bash
command.

``` r
my_dock$RUN(r_this('install.packages("plumber", repo = "http://cran.irsn.fr/")'))
```

Classical Docker stuffs:

``` r
my_dock$RUN("mkdir /usr/scripts")
my_dock$RUN("cd /usr/scripts")
my_dock$COPY("plumberfile.R", "/usr/scripts/plumber.R")
my_dock$COPY("torun.R", "/usr/scripts/torun.R")
my_dock$EXPOSE("8000")
my_dock$CMD("Rscript /usr/scripts/torun.R ")
```

See your Dockerfile :

``` r
my_dock
#> FROM rocker/r-base
#> RUN R -e 'install.packages("plumber", repo = "http://cran.irsn.fr/")'
#> RUN mkdir /usr/scripts
#> RUN cd /usr/scripts
#> COPY plumberfile.R /usr/scripts/plumber.R
#> COPY torun.R /usr/scripts/torun.R
#> EXPOSE 8000
#> CMD Rscript /usr/scripts/torun.R
```

Save your Dockerfile

``` r
my_dock$write()
```

Please note that this project is released with a [Contributor Code of
Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree
to abide by its terms.