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


## Questions

- How can I plot my data?
- What type of plot should I use for my data?


Today we're going to learn how to plot in R using ggplot2.
This is a package designed to streamline the production
of complicated and aesthetically pleasing plots. You can also
make plots in R without ggplot2, but its easier to learn ggplot2
and almost any plot can be constructed with it. Open Rstudio and 
install packages. For the lab computers, everything is reset when
you log out, so you need to install packages again.



```R
#This has example data for plotting
install.packages("palmerpenguins")
#This has different themes for designing your plots
install.packages("ggthemes")
#This has the main packages for plotting and data transformation
install.packages("tidyverse")

```

Now we load packages.

```R
library("tidyverse")
library("palmerpenguins")
library("ggthemes")
```

Now we're going to work through [Chapter 1 of R for Data Science](https://r4ds.hadley.nz/data-visualize).

If you finish Chapter 1 early, [Chapter 9](https://r4ds.hadley.nz/layers)
extends into more complicated plots.
