---
title: "Select rows" 
author: Damien Jourdain
date: '2021-11-20'
slug: select-rows-tidyverse
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

{{% callout note%}}

`n()` is a very useful `dplyr` function that outputs the number of rows of the dataset.
For example, you could use it with `slice` to discard the first 200 rows.


```r
slice(data, 200:n()) %>% head(10)
```

```
## # A tibble: 10 x 7
##    country      continent  year lifeExp      pop gdpPercap row_no
##    <fct>        <fct>     <int>   <dbl>    <int>     <dbl>  <int>
##  1 Burkina Faso Africa     1987    49.6  7586551      912.    200
##  2 Burkina Faso Africa     1992    50.3  8878303      932.    201
##  3 Burkina Faso Africa     1997    50.3 10352843      946.    202
##  4 Burkina Faso Africa     2002    50.6 12251209     1038.    203
##  5 Burkina Faso Africa     2007    52.3 14326203     1217.    204
##  6 Burundi      Africa     1952    39.0  2445618      339.    205
##  7 Burundi      Africa     1957    40.5  2667518      380.    206
##  8 Burundi      Africa     1962    42.0  2961915      355.    207
##  9 Burundi      Africa     1967    43.5  3330989      413.    208
## 10 Burundi      Africa     1972    44.1  3529983      464.    209
```
(note we have chained the command with head(10) using the pipe)

{{% /callout%}}


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


## Randomly selecting rows

A more interesting application is the creation of a random subsample of cases (i.e., rows).
Again, you can use a slice helper: `slice_sample()` which allows you to random select with or without replacement.


```r
data %>% slice_sample(n = 5)
```

```
## # A tibble: 5 x 7
##   country      continent  year lifeExp      pop gdpPercap row_no
##   <fct>        <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Spain        Europe     1992    77.6 39549438    18603.   1425
## 2 Finland      Europe     1972    70.9  4639657    14359.    521
## 3 Sierra Leone Africa     1962    32.8  2467895     1117.   1347
## 4 Mozambique   Africa     1967    38.1  8680909      567.   1036
## 5 El Salvador  Americas   1987    63.2  4842194     4140.    476
```

```r
data %>% slice_sample(n = 5, replace = TRUE)
```

```
## # A tibble: 5 x 7
##   country  continent  year lifeExp      pop gdpPercap row_no
##   <fct>    <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Togo     Africa     1982    55.5  2644765     1345.   1543
## 2 Pakistan Asia       1962    47.7 53100671      803.   1167
## 3 Kuwait   Asia       2007    77.6  2505559    47307.    864
## 4 Niger    Africa     1997    51.3  9666252      580.   1126
## 5 Gabon    Africa     1952    37.0   420702     4293.    541
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
data10 %>% slice_sample(n = 20, replace = TRUE)
```

```
## # A tibble: 20 x 7
##    country     continent  year lifeExp      pop gdpPercap row_no
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
##  1 Afghanistan Asia       1977    38.4 14880372      786.      6
##  2 Afghanistan Asia       1952    28.8  8425333      779.      1
##  3 Afghanistan Asia       1987    40.8 13867957      852.      8
##  4 Afghanistan Asia       1992    41.7 16317921      649.      9
##  5 Afghanistan Asia       1952    28.8  8425333      779.      1
##  6 Afghanistan Asia       1997    41.8 22227415      635.     10
##  7 Afghanistan Asia       1997    41.8 22227415      635.     10
##  8 Afghanistan Asia       1992    41.7 16317921      649.      9
##  9 Afghanistan Asia       1967    34.0 11537966      836.      4
## 10 Afghanistan Asia       1987    40.8 13867957      852.      8
## 11 Afghanistan Asia       1977    38.4 14880372      786.      6
## 12 Afghanistan Asia       1967    34.0 11537966      836.      4
## 13 Afghanistan Asia       1982    39.9 12881816      978.      7
## 14 Afghanistan Asia       1972    36.1 13079460      740.      5
## 15 Afghanistan Asia       1962    32.0 10267083      853.      3
## 16 Afghanistan Asia       1952    28.8  8425333      779.      1
## 17 Afghanistan Asia       1982    39.9 12881816      978.      7
## 18 Afghanistan Asia       1952    28.8  8425333      779.      1
## 19 Afghanistan Asia       1957    30.3  9240934      821.      2
## 20 Afghanistan Asia       1992    41.7 16317921      649.      9
```
{{< /spoiler >}}

## Selecting rows based on a logical criterion

Let say you want to look only at South Africa data. 

We will now use another command from the dplyr package : `filter()`


```r
SAdat <- filter(data, country=="South Africa")
head(SAdat)  #the function head selects the first 6 rows of a data frame
```

```
## # A tibble: 6 x 7
##   country      continent  year lifeExp      pop gdpPercap row_no
##   <fct>        <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 South Africa Africa     1952    45.0 14264935     4725.   1405
## 2 South Africa Africa     1957    48.0 16151549     5487.   1406
## 3 South Africa Africa     1962    50.0 18356657     5769.   1407
## 4 South Africa Africa     1967    51.9 20997321     7114.   1408
## 5 South Africa Africa     1972    53.7 23935810     7766.   1409
## 6 South Africa Africa     1977    55.5 27129932     8029.   1410
```

Note again how the command worked :

+ The first argument is a data frame.
+ The subsequent arguments describe what to do with the data frame, using the variable names *without quotes*.
+ The result is a new data frame.

You can create a more complicated criteria using functions and operators such as

+ `==`, `>`, `>=`,  `<`, `<=`  (when constructing comparison expressions)
+ `%in%` when checking presence in a list of possible value
+ `&` (and), `|` (or), `!`  (not) (when combining expression)

In the following example, we are selecting South Africa records for years after 2000:


```r
SA2000 <- filter(data, country=="South Africa" & year > 2000)
head(SA2000,3)
```

```
## # A tibble: 2 x 7
##   country      continent  year lifeExp      pop gdpPercap row_no
##   <fct>        <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 South Africa Africa     2002    53.4 44433622     7711.   1415
## 2 South Africa Africa     2007    49.3 43997828     9270.   1416
```

### Exercise 4: Select recent African data

+ Select records from African country with a population greater than 30 million for years after 2003 (note there are three selection criteria)

{{< spoiler text="Click to see the solution" >}} 

```r
filter(data, continent=="Africa" , year > 2003, pop > 30000000)
```

```
## # A tibble: 10 x 7
##    country          continent  year lifeExp       pop gdpPercap row_no
##    <fct>            <fct>     <int>   <dbl>     <int>     <dbl>  <int>
##  1 Algeria          Africa     2007    72.3  33333216     6223.     36
##  2 Congo, Dem. Rep. Africa     2007    46.5  64606759      278.    336
##  3 Egypt            Africa     2007    71.3  80264543     5581.    468
##  4 Ethiopia         Africa     2007    52.9  76511887      691.    516
##  5 Kenya            Africa     2007    54.1  35610177     1463.    828
##  6 Morocco          Africa     2007    71.2  33757175     3820.   1032
##  7 Nigeria          Africa     2007    46.9 135031164     2014.   1140
##  8 South Africa     Africa     2007    49.3  43997828     9270.   1416
##  9 Sudan            Africa     2007    58.6  42292929     2602.   1452
## 10 Tanzania         Africa     2007    52.5  38139640     1107.   1524
```

{{< /spoiler >}}

### Exercise 5: Select the data that from South Africa except those from 1962, 1977 and 2003


{{< spoiler text="Click to see the solution" >}} 

```r
filter(data, country=="South Africa" & !(year %in% c(1962, 1977, 2003)))
```

```
## # A tibble: 10 x 7
##    country      continent  year lifeExp      pop gdpPercap row_no
##    <fct>        <fct>     <int>   <dbl>    <int>     <dbl>  <int>
##  1 South Africa Africa     1952    45.0 14264935     4725.   1405
##  2 South Africa Africa     1957    48.0 16151549     5487.   1406
##  3 South Africa Africa     1967    51.9 20997321     7114.   1408
##  4 South Africa Africa     1972    53.7 23935810     7766.   1409
##  5 South Africa Africa     1982    58.2 31140029     8568.   1411
##  6 South Africa Africa     1987    60.8 35933379     7826.   1412
##  7 South Africa Africa     1992    61.9 39964159     7225.   1413
##  8 South Africa Africa     1997    60.2 42835005     7479.   1414
##  9 South Africa Africa     2002    53.4 44433622     7711.   1415
## 10 South Africa Africa     2007    49.3 43997828     9270.   1416
```

{{< /spoiler >}}

