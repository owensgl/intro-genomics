---
title: R and data wrangling
element: lab
layout: default
---

# Section 1

## Objectives

- Load Libraries in R studio
- Load data from a text file into R
- Describe the structure of a dataframe
- Convert a dataframe to tidy format
- Filter a dataframe
- Summarize and transforma a dataframe

## Activities

### R and Rstudio

R is a programming language, but is also the software that runs R code. 
We are going to use <i>Rstudio Server</i> to run R code. This is an
integrated development environment, so it lets you write code, run the code,
and see the results all in one environment. We are using this on the servers,
which is why it is Rstudio <i>Server</i>, but you can also run this on your own
computer using Rstudio Desktop. The first step is to log into the UVic VPN. 
Next open a web browser and go [here](https://indri.rcs.uvic.ca/). It will
give you a warning that the website is not secure because we didn't pay 
for a valid security token. Ignore the security warning and proceed to the website.
On Safari, this involves clicking "Show Details" and then "visit this website". 

![private connection](figs/private_connection.png)

Next you will be prompted to "Sign in to Rstudio". Here the username and password
is your UVic Netlink ID and password. After entering that you will see the Rstudio
Server window which looks something like this:

![Rstudio](figs/rstudio_server.png)

In class, I'll walk through what the different windows are used for now.

### Setup and library installation
Many people using R have developed packages. Theses are sets of commands that extend
the capability of R. For example, there are packages for manipulating map data 
or making map figures. Many of these packages are in CRAN and can be installed
directly from R itself. After installed, you can load these packages and use them in R, 
you don't need to reinstall them every time you use them.

We're going to install a series of packages for handling data. 
```R
install.packages("dplyr")
install.packages("ggplot2")
install.packages("gapminder")
install.packages("forcats")
install.packages("stringr")
install.packages("readr")
install.packages("tibble")
```



