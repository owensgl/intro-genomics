---
title: Data visualization
element: lab
layout: default
---

## Objectives

- Understand the grammar of ggplot
- Apply aesthetic changes to a plot
- Visualize a distribution
- Visualize a relationship between variables
- Explain

## Questions

- How can I plot my data?
- What type of plot should I use for my data?


Today we're going to learn how to plot in R using ggplot2.
This is a package designed to streamline the production
of complicated and aesthetically pleasing plots. You can also
make plots in R without ggplot2, but its easier to learn ggplot2
and almost any plot can be constructed with it.

We first have to login to the <b>VPN</b> and then the [Rstudio Server](https://indri.rcs.uvic.ca/)
in your browser. We already installed most of the tidyverse packages
last lab (go back to lab 3 if you didn't do that). Today we're going to install
a few more packages used in the tutorial

```R
#This has example data for plotting
install.packages("palmerpenguins")
#This has different themes for designing your plots
install.packages("ggthemes")

```

Then we want to load all the packages. We're doing this instead of loading
tidyverse (which has issues on our system)

```R
library("dplyr")
library("ggplot2")
library("nycflights13")
library("forcats")
library("stringr")
library("readr")
library("tibble")
library("tidyr")
library("purrr")
library("stringr")
library("palmerpenguins")
library("ggthemes")
```

Now we're going to work through [Chapter 1 of R for Data Science](https://r4ds.hadley.nz/data-visualize).

If you finish Chapter 1 early, [Chapter 9](https://r4ds.hadley.nz/layers)
extends into more complicated plots.
