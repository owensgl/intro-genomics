---
title: R and data wrangling
element: lab
layout: default
---

## Objectives

- Load Libraries in R studio
- Describe the structure of a dataframe
- Filter a dataframe
- Summarize and transform a dataframe

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

![](figs/private_connection.png)

Next you will be prompted to "Sign in to Rstudio". Here the username and password
is your UVic Netlink ID and password. After entering that you will see the Rstudio
Server window which looks something like this:

![](figs/rstudio_server.png)

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
install.packages("nycflights13")
install.packages("forcats")
install.packages("stringr")
install.packages("readr")
install.packages("tibble")
install.packages("tidyr")
install.packages("purrr")
install.packages("stringr")

```

Those packages are installed, but we also have to load them before they can be used.

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
```

### Dataframes

One of the ways of storing data in R is in a dataframe. This is essentially a table with column names
and rows of data. Importantly a dataframe is always rectangular, which means each column has the same
number of rows. We're going to make our own dataframe to start.

```R
df <- data.frame(
  Name = c("Alice", "Bob", "Charlie", "David"),
  Age = c(25, 30, 22, 28),
  Salary = c(50000, 60000, 45000, 55000)
)
```

This table has three columns: Name, Age and Salary. It is stored in the object "df", although this
is arbitrary, you could call this dataframe whatever you want. 

We can look at the dataframe by calling the object.

```R
df
```

```output
     Name Age Salary
1   Alice  25  50000
2     Bob  30  60000
3 Charlie  22  45000
4   David  28  55000
```

#### Challenge

We want to change our dataframe and remove the row with Bob in it. In addition,
we want to add 72 year old Abe, who has a salary of 92000. Make the new changed dataframe
and call it df2.


### Accessing Data

Once our data is in a dataframe, we will want to be access it, so lets learn about several ways you
can pull out specific parts of the data.

First method is to access specific named columns using the "$". In this case, we are pulling it out based on
the name, it doesn't matter the relative order of columns.

```R
names <- df$Name
names
```

```output
[1] "Alice"   "Bob"     "Charlie" "David"
```

The '[1]' indicates the positione number of the first item in that line. In this case,
there is only one line of items but this can be helpful if you print out very long 
lists of items. 

The result of our command is a list of all names, in their original order. 

We can also access a column by using its relative position. The Name column is the first 
column, so we're going to print out the first column.

```R
column_1 <- df[,1]
column_1
```

```output
[1] "Alice"   "Bob"     "Charlie" "David"
```

This is a general way of accessing data in R. It uses the format [Row_number,Column_number]. 
We didn't put anything in the "Row number" portion, so it printed out all rows. This same function
can be used to pull out specific rows, or even single values by triangulating their row and column.


Lastly, we can access specific columns by using the "pull()" function.

```R
names <- pull(df, Name)
names
```
```output
[1] "Alice"   "Bob"     "Charlie" "David"
```

For this command, we're telling it to pull out the "Name" column from the "df" dataframe. 
We will see this command more when we start chaining together different commands.

### Challenge

Write two different sets of commands to extract the age of Charlie. 
Hint: You can use the [n] notation on a list. In this case, you only need to give it
one number, since this is is 1-dimensional.


### Using the pipe function.

When we used the pull function, we could also organize it using a pipe. This
is similar to using pipes in command line, where you're passing the output
of one command to the next.

```R
df |>
  pull(Name)

```
```output
[1] "Alice"   "Bob"     "Charlie" "David"
```

In this case, we're taking the dataframe "df" and passing it to the pull command.
You'll notice that we don't have to tell pull which dataframe to use, it knows that it is
using the dataframe passed to it from the previous part of the code.

### Transform data!

We're going to be following along with the textbook "R for data science". We're working on 
chapter 3, Data Transformation: !(LINK)[https://r4ds.hadley.nz/data-transform]

Important: In the tutorial, it loads "tidyverse". We have already installed and loaded
almost all of the tidyverse packages, but due to some quirks in the server it's easier
to use it the way listed here. So, where it says "library(tidyverse)", skip that command.









