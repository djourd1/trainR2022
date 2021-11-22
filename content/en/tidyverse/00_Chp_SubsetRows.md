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

```r
# You may want to overwrite the old data
data <- data %>% slice_tail(n=10)  

# However, you have deleted all the other data from the memory!
# Make sure this is what you wanted to do...
```
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


