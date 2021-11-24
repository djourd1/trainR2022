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

Let's take the gapminder data again. To help visualize how data are reshaped, let's retain only data available after 2000.


```r
library(tidyverse)
library(gapminder)
data <- gapminder %>% filter(year >2000)
data
```

```
## # A tibble: 284 x 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       2002    42.1 25268405      727.
##  2 Afghanistan Asia       2007    43.8 31889923      975.
##  3 Albania     Europe     2002    75.7  3508512     4604.
##  4 Albania     Europe     2007    76.4  3600523     5937.
##  5 Algeria     Africa     2002    71.0 31287142     5288.
##  6 Algeria     Africa     2007    72.3 33333216     6223.
##  7 Angola      Africa     2002    41.0 10866106     2773.
##  8 Angola      Africa     2007    42.7 12420476     4797.
##  9 Argentina   Americas   2002    74.3 38331121     8798.
## 10 Argentina   Americas   2007    75.3 40301927    12779.
## # ... with 274 more rows
```

You can see that for each country, there are several lines, each corresponding to one year.
This is typical of panel data where you collected the same information about a respondent at different times.

## What do you want to obtain?

If your data set is originally in a long format, there may be cases where you want to reshape it in long format. The diagram below illustrates what is the objective:

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="631px" height="301px" viewBox="-0.5 -0.5 631 301" content="&lt;mxfile host=&quot;Electron&quot; modified=&quot;2021-02-19T15:13:08.412Z&quot; agent=&quot;5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) draw.io/14.1.8 Chrome/87.0.4280.88 Electron/11.1.1 Safari/537.36&quot; etag=&quot;uE7aIz0tqJswcFRSWt1-&quot; version=&quot;14.1.8&quot; type=&quot;device&quot;&gt;&lt;diagram id=&quot;uoSsXVGMSTPBpk-EBTHv&quot; name=&quot;Page-1&quot;&gt;1Zhbk5owGIZ/DZfdIYSDXFZ0t51ppwc7XXuZlQjUQJwQVuivb4CgYjxuoarMaL43JCTP+0EwGvTi/ImhZfiZ+phohu7nGhxphjGwDPFdCkUtmK5ZCwGL/FoCG2ES/cFS1KWaRT5OWydySgmPlm1xRpMEz3hLQ4zRVfu0OSXtqy5RgBVhMkNEVZ8jn4dSNXR9U/EBR0HYXNpuamLUnC2FNEQ+XW1JcKxBj1HK61Kce5iU8BowdbvHA7XrkTGc8HMafFl8fxp76dRdvP+RZ2aYZva3d3bdyysimZyxHCwvGgSMZomPy06ABoerMOJ4skSzsnYlPBdayGMiq+cRIR4llFVt4dguD6HLy2DGcX5w/GBNRaQTpjHmrBCnyAZQciza4Wpjyzprwi1HYGMIkqkQrHvewBIFyesCdkBh93EEjuDTL8Y30MujG3xmG5+p4jP30DMGfdEz9tA7lnyX0xtWRzf03BujBxV6PxETQrf55zoj3XG6IQjeloDA6AuheQBht0noeY/i0w9C4JzJ0OqLoaUwrIv/RLALUrd2vzr3tM6eXGihvQ8e6Ave4K4WWmi87UHn9oXPvauVFjo3hq9J6ztaa83TKQj0PRDN3hiq78q3vtha8NYYqm/MkyyOUTXoeZbMeEQTUaRzzbBRXJJKXtJlRcMmYnTDF0HcDspS1Ud69WV6N09dlfHe/3SgP8jwBBTKeEgDmiDyidKlRPEbc17IzQuUcdoGhfOIT7fKv8quHiwZjXLZcxUUMthJbF18qsROOaOL9VaEubYB+8o+xo4JYk40YzN8ZPJy7hyxAPOTT0TVVYYJ4tFreyDdW2QqFpWzn8gwoYn4GV7FNUGcFdPtoG7lWE28aVdFTUPV1kMJ0KHd5rl2H7iJ/5Pd1hXtds71+wG4bcstB5zwvIq+YhYJTphdMxGscxPB6CcRRLjZBq3qtjaT4fgv&lt;/diagram&gt;&lt;/mxfile&gt;"><defs/><g><rect x="0" y="0" width="210" height="300" rx="31.5" ry="31.5" fill="#e6e6e6" stroke="#000000" pointer-events="all"/><rect x="10" y="10" width="40" height="280" fill="#808080" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 38px; height: 1px; padding-top: 150px; margin-left: 11px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">ID1</div></div></div></foreignObject><text x="30" y="154" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">ID1</text></switch></g><rect x="60" y="10" width="40" height="280" fill="#b3b3b3" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 38px; height: 1px; padding-top: 150px; margin-left: 61px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">ID2</div></div></div></foreignObject><text x="80" y="154" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">ID2</text></switch></g><rect x="110" y="10" width="40" height="120" fill="#97d077" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 38px; height: 1px; padding-top: 70px; margin-left: 111px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">Var 1</div></div></div></foreignObject><text x="130" y="74" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">Var 1</text></switch></g><rect x="110" y="140" width="40" height="150" fill="#ccffff" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 38px; height: 1px; padding-top: 215px; margin-left: 111px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">Var 2</div></div></div></foreignObject><text x="130" y="219" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">Var 2</text></switch></g><rect x="160" y="10" width="40" height="280" fill="#ffffff" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 38px; height: 1px; padding-top: 150px; margin-left: 161px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">value</div></div></div></foreignObject><text x="180" y="154" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">value</text></switch></g><rect x="270" y="0" width="360" height="210" rx="31.5" ry="31.5" fill="#e6e6e6" stroke="#000000" pointer-events="all"/><rect x="290" y="10" width="40" height="190" fill="#808080" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 38px; height: 1px; padding-top: 105px; margin-left: 291px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">ID1</div></div></div></foreignObject><text x="310" y="109" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">ID1</text></switch></g><rect x="340" y="10" width="40" height="190" fill="#b3b3b3" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 38px; height: 1px; padding-top: 105px; margin-left: 341px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">ID2</div></div></div></foreignObject><text x="360" y="109" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">ID2</text></switch></g><rect x="390" y="10" width="100" height="40" fill="#97d077" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 98px; height: 1px; padding-top: 30px; margin-left: 391px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">Var 1</div></div></div></foreignObject><text x="440" y="34" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">Var 1</text></switch></g><rect x="500" y="10" width="100" height="40" fill="#ccffff" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 98px; height: 1px; padding-top: 30px; margin-left: 501px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">Var 2</div></div></div></foreignObject><text x="550" y="34" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">Var 2</text></switch></g><rect x="390" y="60" width="210" height="140" fill="#ffffff" stroke="#000000" pointer-events="all"/><g transform="translate(-0.5 -0.5)"><switch><foreignObject style="overflow: visible; text-align: left;" pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"><div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 208px; height: 1px; padding-top: 130px; margin-left: 391px;"><div style="box-sizing: border-box; font-size: 0; text-align: center; "><div style="display: inline-block; font-size: 12px; font-family: Helvetica; color: #000000; line-height: 1.2; pointer-events: all; white-space: normal; word-wrap: normal; ">Summary function of <br />values</div></div></div></foreignObject><text x="495" y="134" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">Summary function of...</text></switch></g><path d="M 150 70 L 378.14 38.53" fill="none" stroke="#000000" stroke-width="4" stroke-miterlimit="10" pointer-events="stroke"/><path d="M 385.57 37.51 L 376.35 43.83 L 378.14 38.53 L 374.98 33.92 Z" fill="#000000" stroke="#000000" stroke-width="4" stroke-miterlimit="10" pointer-events="all"/><path d="M 150 215 L 489.29 45.35" fill="none" stroke="#000000" stroke-width="4" stroke-miterlimit="10" pointer-events="stroke"/><path d="M 496 42 L 489.29 50.94 L 489.29 45.35 L 484.82 42 Z" fill="#000000" stroke="#000000" stroke-width="4" stroke-miterlimit="10" pointer-events="all"/><path d="M 200 220 L 418.59 143.88" fill="none" stroke="#000000" stroke-width="4" stroke-miterlimit="10" pointer-events="stroke"/><path d="M 425.68 141.41 L 417.88 149.42 L 418.59 143.88 L 414.59 139.98 Z" fill="#000000" stroke="#000000" stroke-width="4" stroke-miterlimit="10" pointer-events="all"/></g><switch><g requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"/><a transform="translate(0,-5)" xlink:href="https://www.diagrams.net/doc/faq/svg-export-text-problems" target="_blank"><text text-anchor="middle" font-size="10px" x="50%" y="100%">Viewer does not support full SVG 1.1</text></a></switch></svg>


### Reshape one variable only

Let say you want to have a new table where:

+ each line corresponds to a unique country
+ each line contains information about the population for the different years

Since our original data has a long format, we will use the command `pivot_wider()`. This function is part of the `tidyr` package, a member of the tidyverse ecosystem, so do not forget to load `tidyr` or the `tidyverse` package:


```r
data %>% 
  select(country, continent, year, pop) %>%  # we keep the interesting variables
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

We created two new columns which names are a combination of the names_prefix (here `popul`) and the year (which we declared using the argument names_from). Each cell in these columns correspond to the countries' population in the corresponding years.

### Reshape several variables

Let's now assume that you want to have another table where:

+ each line corresponds to a unique country
+ each line contains information about the population *and* life expectancy for the 2000's

Since there are two variables to be reshaped, the names_prefix is now automatically taken from the names_from variables.



```r
data %>% 
  select(country, continent, year, pop, lifeExp) %>%  # we keep the interesting variables
  pivot_wider(names_from = year, values_from = c(pop, lifeExp)) %>% #select variables to reshape
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

## To go further

The cases presented are the most basic uses of `pivot_wider()`. 

More details about pivot_wider can be found at: <a href="https://tidyr.tidyverse.org/reference/pivot_wider.html" target="_blank">pivot_wider</a>

## Exercise: Wage data

For this exercise, we will use the data set `WageData` that is coming with the `panelr` package. The data come from the years 1976-1982 in the Panel Study of Income Dynamics (PSID), with information about the demographics and earnings of 595 individuals.


```r
library(panelr)
data("WageData")
head(WageData)
```

```
##   exp wks occ ind south smsa ms fem union ed blk   lwage t id
## 1   3  32   0   0     1    0  1   0     0  9   0 5.56068 1  1
## 2   4  43   0   0     1    0  1   0     0  9   0 5.72031 2  1
## 3   5  40   0   0     1    0  1   0     0  9   0 5.99645 3  1
## 4   6  39   0   0     1    0  1   0     0  9   0 5.99645 4  1
## 5   7  42   0   1     1    0  1   0     0  9   0 6.06146 5  1
## 6   8  35   0   1     1    0  1   0     0  9   0 6.17379 6  1
```
The key columns are `id` and `t`. They tell you which respondent and which time point the row refers to, respectively. We will also retain the variable `ed` that records the education level.


```r
(wages <- WageData %>% select(id, t, lwage, ed)%>% as_tibble() )
```

```
## # A tibble: 4,165 x 4
##       id     t lwage    ed
##    <dbl> <dbl> <dbl> <dbl>
##  1     1     1  5.56     9
##  2     1     2  5.72     9
##  3     1     3  6.00     9
##  4     1     4  6.00     9
##  5     1     5  6.06     9
##  6     1     6  6.17     9
##  7     1     7  6.24     9
##  8     2     1  6.16    11
##  9     2     2  6.21    11
## 10     2     3  6.26    11
## # ... with 4,155 more rows
```

__Task:__ Reshape wages it into a wide format, so that:

+ one line corresponds to one id
+ on each line, you can find the variables id, ed, and seven variables corresponding the lwages for each t.

{{< spoiler text="Click to see the solution" >}} 

```r
wages %>% pivot_wider(names_from = t, values_from=lwage, names_prefix = "lwage")
```

```
## # A tibble: 595 x 9
##       id    ed lwage1 lwage2 lwage3 lwage4 lwage5 lwage6 lwage7
##    <dbl> <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1     1     9   5.56   5.72   6.00   6.00   6.06   6.17   6.24
##  2     2    11   6.16   6.21   6.26   6.54   6.70   6.79   6.82
##  3     3    12   5.65   6.44   6.55   6.60   6.70   6.78   6.86
##  4     4    10   6.16   6.24   6.30   6.36   6.47   6.56   6.62
##  5     5    16   6.44   6.62   6.63   6.98   7.05   7.31   7.30
##  6     6    12   6.91   6.91   6.91   7.00   7.07   7.52   7.34
##  7     7    12   6.13   6.17   6.21   6.31   6.38   6.45   6.52
##  8     8    10   6.33   6.40   6.54   6.56   6.59   6.82   6.89
##  9     9    16   6.55   6.55   6.80   6.91   7.09   7.17   7.21
## 10    10    16   6.40   6.44   6.44   6.44   6.52   6.61   6.74
## # ... with 585 more rows
```
{{< /spoiler >}}

