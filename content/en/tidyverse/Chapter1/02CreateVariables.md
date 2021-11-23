---
title: "Create Variables" 
author: Damien Jourdain
date: '2020-08-02'
slug: create-variables
categories:
  - R
tags: []
type: book
weight: 5

output:
  html_document:
    keep_md: true

---






Surveys provides raw data. Often you will need to combine information collected with different variables into a new variable. So you will need to add new columns that are functions of existing columns. 

`mutate()` adds new columns at the end of the table. 


```r
data %>% 
  select(row_no, gdpPercap, pop) %>%
  mutate(GDP = gdpPercap * pop)
```

```
## # A tibble: 1,704 x 4
##    row_no gdpPercap      pop          GDP
##     <int>     <dbl>    <int>        <dbl>
##  1      1      779.  8425333  6567086330.
##  2      2      821.  9240934  7585448670.
##  3      3      853. 10267083  8758855797.
##  4      4      836. 11537966  9648014150.
##  5      5      740. 13079460  9678553274.
##  6      6      786. 14880372 11697659231.
##  7      7      978. 12881816 12598563401.
##  8      8      852. 13867957 11820990309.
##  9      9      649. 16317921 10595901589.
## 10     10      635. 22227415 14121995875.
## # ... with 1,694 more rows
```

Arithmetic operators can also be very useful. For example, `x / sum(x)` calculates the proportion of a total, and `y - mean(y)` computes the difference from the mean.


```r
data %>% 
  select(row_no, gdpPercap) %>%
  mutate(diffMeansGdp = gdpPercap - mean(gdpPercap) )
```

```
## # A tibble: 1,704 x 3
##    row_no gdpPercap diffMeansGdp
##     <int>     <dbl>        <dbl>
##  1      1      779.       -6436.
##  2      2      821.       -6394.
##  3      3      853.       -6362.
##  4      4      836.       -6379.
##  5      5      740.       -6475.
##  6      6      786.       -6429.
##  7      7      978.       -6237.
##  8      8      852.       -6363.
##  9      9      649.       -6566.
## 10     10      635.       -6580.
## # ... with 1,694 more rows
```


### Exercise: 

Show a table that displays, the annual GDP of Zimbabwe after 1972 xpressed in million USD and sort the results by year.

{{< spoiler text="Click to see the solution" >}} 

```r
data %>% filter(country=="Zimbabwe" & year >1972) %>%
  mutate(GDP = gdpPercap * pop / 1000000) %>%
  select(country, year, GDP) %>%
  arrange(year)
```

```
## # A tibble: 7 x 3
##   country   year   GDP
##   <fct>    <int> <dbl>
## 1 Zimbabwe  1977 4554.
## 2 Zimbabwe  1982 6024.
## 3 Zimbabwe  1987 6508.
## 4 Zimbabwe  1992 7423.
## 5 Zimbabwe  1997 9038.
## 6 Zimbabwe  2002 8015.
## 7 Zimbabwe  2007 5783.
```

{{< /spoiler >}}


### Exercise: 
Show a table that displays, the annual 2007 GDP of African countries whose name starts with an A or a B expressed in million USD and sort the results by increasing GDP.

{{< spoiler text="Click to see the solution" >}} 

```r
data %>% filter(continent=="Africa" & substr(country,1,1) %in% c("A", "B") & year==2007) %>%
  mutate(GDP = gdpPercap * pop / 1000000) %>%
  select(country,  GDP) %>%
  arrange(GDP)
```

```
## # A tibble: 6 x 2
##   country          GDP
##   <fct>          <dbl>
## 1 Burundi        3609.
## 2 Benin         11643.
## 3 Burkina Faso  17435.
## 4 Botswana      20604.
## 5 Angola        59584.
## 6 Algeria      207445.
```

{{< /spoiler >}}

