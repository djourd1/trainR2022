---
title: "From long to wide" 
author: Damien Jourdain
date: '2020-08-20'
slug: long-to-wide
type: book
weight: 2
output:
  html_document:
    keep_md: true
---


## The original data

Let's take the gapminder data again. 


```r
library(tidyverse)
library(gapminder)
gapminder
```

```
## # A tibble: 1,704 x 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
## # ... with 1,694 more rows
```

You can see that for each country, there are several lines, each corresponding to one year.
This is typical of panel data where you collected the same information about a respondent at different times.

## What do you want to obtain: one variable over time

### Objective
If you want to reshape this table such as the information about a country are all presented in the same line (from long to wide format).

Let's first assume that you want to have a new table:

+ each line corresponds to a unique country
+ each line contains information about the population for the 2000's


### Use pivot_wider()

To do so, we will use the command `pivot_wider()`. 

It is part of the `tidyr` package, a member of the tidyverse ecosystem, so do not forget to load the `tidyverse` package:


```r
gapminder %>% 
  filter(year > 2000) %>%   # we keep only interesting years
  select(country, continent,year, pop) %>%  # we keep the interesting variables
  pivot_wider(names_from = year, values_from = c(pop), names_prefix = "popul") %>%
  arrange(continent, country) # we sort
```

```
## # A tibble: 142 x 4
##    country                  continent popul2002 popul2007
##    <fct>                    <fct>         <int>     <int>
##  1 Algeria                  Africa     31287142  33333216
##  2 Angola                   Africa     10866106  12420476
##  3 Benin                    Africa      7026113   8078314
##  4 Botswana                 Africa      1630347   1639131
##  5 Burkina Faso             Africa     12251209  14326203
##  6 Burundi                  Africa      7021078   8390505
##  7 Cameroon                 Africa     15929988  17696293
##  8 Central African Republic Africa      4048013   4369038
##  9 Chad                     Africa      8835739  10238807
## 10 Comoros                  Africa       614382    710960
## # ... with 132 more rows
```

It created two new columns which names are a combination of the names_prefix (here popul) and the year (which we declared using the argument names_from). Each cell in these columns correspond to the countries' population in the corresponding years.

## What do you want to obtain: two variables over time

### Objective

Let's now assume that you want to have a new table:

+ each line corresponds to a unique country
+ each line contains information about the population and life expectancy for the 2000's

### Use pivot_wider()


```r
gapminder %>% 
  filter(year > 2000) %>%   # we keep only interesting years
  select(country, continent,year, pop, lifeExp) %>%  # we keep the interesting variables
  pivot_wider(names_from = year, values_from = c(pop, lifeExp)) %>%
  arrange(continent, country) # we sort
```

```
## # A tibble: 142 x 6
##    country                 continent pop_2002 pop_2007 lifeExp_2002 lifeExp_2007
##    <fct>                   <fct>        <int>    <int>        <dbl>        <dbl>
##  1 Algeria                 Africa    31287142 33333216         71.0         72.3
##  2 Angola                  Africa    10866106 12420476         41.0         42.7
##  3 Benin                   Africa     7026113  8078314         54.4         56.7
##  4 Botswana                Africa     1630347  1639131         46.6         50.7
##  5 Burkina Faso            Africa    12251209 14326203         50.6         52.3
##  6 Burundi                 Africa     7021078  8390505         47.4         49.6
##  7 Cameroon                Africa    15929988 17696293         49.9         50.4
##  8 Central African Republ~ Africa     4048013  4369038         43.3         44.7
##  9 Chad                    Africa     8835739 10238807         50.5         50.7
## 10 Comoros                 Africa      614382   710960         63.0         65.2
## # ... with 132 more rows
```

Since there are two variables retained, the names_prefix is no automtically taken from the names_from variables.


More details about pivot_wider can be found at: <a href="https://tidyr.tidyverse.org/reference/pivot_wider.html" target="_blank">pivot_wider</a>


## Exercises:
