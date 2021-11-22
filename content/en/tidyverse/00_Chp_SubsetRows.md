---
title: "Selecting rows" 
author: Damien Jourdain
date: '2021-11-20'
slug: select-rows
categories:
  - R
tags: []
type: book
weight: 2
output:
  html_document:
    keep_md: true
---

## Learning objectives
Often when you want to understand your data or exclude some individuals that are outside the scope of your analysis, you will need to select subsamples of your data set.

In this chapter, you will learn how to select specific rows of your data frame. This is our first encounter with the package `dplyr` and the use of the pipe for chaining commands. This is an important section, as you will be using this kind of syntax for all the subsequent types of data manipulations.  

At the end of the chapter, you should be able to select specific rows of a data set that will be selected :

+ by their row number
+ a logical criteria based on the different variables describing the rows
+ randomly

First, let's rename the dataframe `gapminder` with a shorter name, and let's add a column `row_no`. That column is not really essential, but it will help visualizing the rows that have been selected.





```r
data <- gapminder
data$row_no = 1:nrow(data)
```


We have several ways to subset rows.

## Selecting rows by their position

To do this we will start to use the `dplyr` package. Note that `dplyr` is already loaded if you loaded the package `tidyverse`. 

The command `slice()` lets you index rows by their locations. It allows you to select, remove, and duplicate rows. 

The easiest way to use slice is to indicate the vector of row numbers you wish to keep as in:

```r
slice(data, 2:5)  # to select the row from 2 to 5
```

```
## # A tibble: 4 x 7
##   country     continent  year lifeExp      pop gdpPercap row_no
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Afghanistan Asia       1957    30.3  9240934      821.      2
## 2 Afghanistan Asia       1962    32.0 10267083      853.      3
## 3 Afghanistan Asia       1967    34.0 11537966      836.      4
## 4 Afghanistan Asia       1972    36.1 13079460      740.      5
```

```r
slice(data, c(7, 5, 1)) # to select specific rows in a specific order
```

```
## # A tibble: 3 x 7
##   country     continent  year lifeExp      pop gdpPercap row_no
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Afghanistan Asia       1982    39.9 12881816      978.      7
## 2 Afghanistan Asia       1972    36.1 13079460      740.      5
## 3 Afghanistan Asia       1952    28.8  8425333      779.      1
```

```r
slice(data, 10) #note that including a number and not a vector also works
```

```
## # A tibble: 1 x 7
##   country     continent  year lifeExp      pop gdpPercap row_no
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Afghanistan Asia       1997    41.8 22227415      635.     10
```

Note that all `dplyr` related commands work in the same way:

+ The first argument is a data frame.
+ The subsequent arguments describe what to do with the data frame, using the variable names (**without quotes**) or sometimes some vectors of numbers.
+ The result is a new data frame.

A very convenient feature of tidyverse is the pipe `%>%` operator. 
To understand how it works, we can reproduce the precedent example using the pipe. 


```r
data %>% slice(2:5)
```

```
## # A tibble: 4 x 7
##   country     continent  year lifeExp      pop gdpPercap row_no
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Afghanistan Asia       1957    30.3  9240934      821.      2
## 2 Afghanistan Asia       1962    32.0 10267083      853.      3
## 3 Afghanistan Asia       1967    34.0 11537966      836.      4
## 4 Afghanistan Asia       1972    36.1 13079460      740.      5
```

How did that work:

You need to read this command from left to right, and think that the results of the previous command are passed as the first argument of the next command. 

The first command is to take the data.frame data, the result data is passed on as the first argument of the command slice(...). So literally, you think about this command as: take "data", then use slice to select the row 2 to 5...

In this very simple command, the advantage of using the pipe is not obvious as you would probably be faster in writing `data[2:5, ]`, but you will soon see its main advantage when we will start "chaining" several commands.

Note you can use several variants of `slice()`, in particular, you may want to look at the commands 
`slice_head()` and `slice_tail()` to select the first or last n rows from your data set.

### Exercise 1: Select the last 10 rows

Using the command `slice_tail()`, select the last 10 rows of the dataset data.

{{% callout solution%}}
{{< spoiler text="Click to view the solution" >}}


```r
# note that it outputs a new dataframe, however you can check that the initial data 
# was not modified, and that you did not create a new object in the workspace
data %>% slice_tail(n=10)  
```

```
## # A tibble: 10 x 7
##    country  continent  year lifeExp      pop gdpPercap row_no
##    <fct>    <fct>     <int>   <dbl>    <int>     <dbl>  <int>
##  1 Zimbabwe Africa     1962    52.4  4277736      527.   1695
##  2 Zimbabwe Africa     1967    54.0  4995432      570.   1696
##  3 Zimbabwe Africa     1972    55.6  5861135      799.   1697
##  4 Zimbabwe Africa     1977    57.7  6642107      686.   1698
##  5 Zimbabwe Africa     1982    60.4  7636524      789.   1699
##  6 Zimbabwe Africa     1987    62.4  9216418      706.   1700
##  7 Zimbabwe Africa     1992    60.4 10704340      693.   1701
##  8 Zimbabwe Africa     1997    46.8 11404948      792.   1702
##  9 Zimbabwe Africa     2002    40.0 11926563      672.   1703
## 10 Zimbabwe Africa     2007    43.5 12311143      470.   1704
```

```r
# if you want to save it with a new name
data2 <- data %>% slice_tail(n=10)  
```

{{% callout warning%}}
You may want to overwrite the old data

```r
data <- data %>% slice_tail(n=10)  
```
However, by doing so you deleted the old dataset!
Make sure this is what you wanted to do...

{{% /callout%}}

{{< /spoiler >}}
{{% /callout %}}

### Exercise 2: Select the rows 3, 50 and 200

Using a command to select the 3, 50 and 200 of the dataset `data`.

{{% callout solution%}}
{{< spoiler text="Click to view the solution" >}}


```r
data %>% slice(c(3,50, 200))
```

```
## # A tibble: 3 x 7
##   country      continent  year lifeExp      pop gdpPercap row_no
##   <fct>        <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Afghanistan  Asia       1962    32.0 10267083      853.      3
## 2 Argentina    Americas   1957    64.4 19610538     6857.     50
## 3 Burkina Faso Africa     1987    49.6  7586551      912.    200
```
{{< /spoiler >}}
{{% /callout %}}


# Randomly selecting rows

A more interesting application is the creation of a random subsample of cases (i.e., rows).
Again, you can use a slice helper: `slice_sample()` which allows you to random select with or without replacement.


```r
data %>% slice_sample(n = 5)
```

```
## # A tibble: 5 x 7
##   country           continent  year lifeExp        pop gdpPercap row_no
##   <fct>             <fct>     <int>   <dbl>      <int>     <dbl>  <int>
## 1 Nepal             Asia       1957    37.7    9682338      598.   1070
## 2 Czech Republic    Europe     2007    76.5   10228744    22833.    408
## 3 China             Asia       1987    67.3 1084035000     1379.    296
## 4 Equatorial Guinea Africa     1972    40.5     277603      672.    485
## 5 Norway            Europe     1967    74.1    3786019    16362.   1144
```

```r
data %>% slice_sample(n = 5, replace = TRUE)
```

```
## # A tibble: 5 x 7
##   country continent  year lifeExp      pop gdpPercap row_no
##   <fct>   <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Peru    Americas   2002    69.9 26769436     5909.   1211
## 2 Bahrain Asia       1957    53.8   138655    11636.     86
## 3 Israel  Asia       1967    70.8  2693585     8394.    760
## 4 Angola  Africa     1982    39.9  7016384     2757.     43
## 5 Ireland Europe     2007    78.9  4109086    40676.    756
```

### Exercise 3

1. Generate a subset of data with its first 10 rows and name it data10
2. Generate a random subsample of `data10` containing 20 rows. (tip: understand what the argument replace is doing)

{{< spoiler text="Click to see the solution" >}} 


```r
data10 <- data %>% slice_head(n=10)
```

Random sample with replacement. Note that it you do not include `replace=TRUE` argument, R will throw an error (why?)


```r
data %>% slice_sample(n = 20, replace = TRUE)
```

```
## # A tibble: 20 x 7
##    country             continent  year lifeExp        pop gdpPercap row_no
##    <fct>               <fct>     <int>   <dbl>      <int>     <dbl>  <int>
##  1 Egypt               Africa     1987    59.8   52799062     3885.    464
##  2 Cameroon            Africa     1962    42.6    5793633     1400.    231
##  3 Cameroon            Africa     2002    49.9   15929988     1934.    239
##  4 Congo, Dem. Rep.    Africa     1987    47.4   35481645      673.    332
##  5 Slovak Republic     Europe     1957    67.4    3844277     6093.   1370
##  6 Syria               Asia       1972    57.3    6701172     2571.   1493
##  7 China               Asia       1992    68.7 1164970000     1656.    297
##  8 Panama              Americas   1967    64.1    1405486     4421.   1180
##  9 Benin               Africa     1992    53.9    4981671     1191.    129
## 10 West Bank and Gaza  Asia       1962    48.1    1133134     2199.   1659
## 11 Mozambique          Africa     1997    46.3   16603334      472.   1042
## 12 Chile               Americas   1962    57.9    7961258     4519.    279
## 13 Montenegro          Europe     2007    74.5     684736     9254.   1020
## 14 Equatorial Guinea   Africa     2007    51.6     551201    12154.    492
## 15 United Kingdom      Europe     1982    74.0   56339704    18232.   1603
## 16 South Africa        Africa     1957    48.0   16151549     5487.   1406
## 17 Yemen, Rep.         Asia       1982    49.1    9657618     1978.   1675
## 18 Trinidad and Tobago Americas   1967    65.4     960155     5621.   1552
## 19 Netherlands         Europe     1967    73.8   12596822    15363.   1084
## 20 Guinea-Bissau       Africa     1952    32.5     580653      300.    625
```
{{< /spoiler >}}
