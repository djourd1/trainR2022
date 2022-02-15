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

If you want exclude some individuals that are outside the scope of your analysis, you need to be able to select subsamples of your data set. In this section, you will learn how to select specific rows of your data set. This is our first encounter with the package `dplyr` and the use of pipes (`%>%`) for chaining commands. **This is an important section**, as you will use this syntax for all types of data manipulation that follow.  

At the end of this section, you should be able to select specific rows of a data set :

+ [using their row number](#select-rows-position)
+ [using a logical criteria based on the different variables describing the rows](#random-rows)
+ [randomly](#logical-criteria)

## Data set up

First, let's rename the dataframe `gapminder` with a shorter name, and let's add a column `row_no` using the data.frame commands. That column is not really essential, but it will help visualizing the rows that have been selected.


```r
library(tidyverse)
library(gapminder)

data <- gapminder
data$row_no = 1:nrow(data)
```

## Selecting rows by their position {#select-rows-position}

To do this we will start to use the `dplyr` package. Note that `dplyr` was loaded when you loaded the package `tidyverse`, so you do not need an additional command. 

### The command slice()

The command `slice()` lets you index rows by their positions. It allows you to select, remove, and duplicate rows. 

The easiest way to use slice is to indicate the vector of row numbers you wish to keep as in:


```r
slice(data, 2:5)  # to select the row from 2 to 5
slice(data, c(7, 5, 1)) # to select specific rows in a specific order
slice(data, 10) #note that including a numeric and not a vector also works
```

{{< spoiler text="Click to view the results" >}}

```
## # A tibble: 4 x 7
##   country     continent  year lifeExp      pop gdpPercap row_no
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Afghanistan Asia       1957    30.3  9240934      821.      2
## 2 Afghanistan Asia       1962    32.0 10267083      853.      3
## 3 Afghanistan Asia       1967    34.0 11537966      836.      4
## 4 Afghanistan Asia       1972    36.1 13079460      740.      5
```

```
## # A tibble: 3 x 7
##   country     continent  year lifeExp      pop gdpPercap row_no
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Afghanistan Asia       1982    39.9 12881816      978.      7
## 2 Afghanistan Asia       1972    36.1 13079460      740.      5
## 3 Afghanistan Asia       1952    28.8  8425333      779.      1
```

```
## # A tibble: 1 x 7
##   country     continent  year lifeExp      pop gdpPercap row_no
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Afghanistan Asia       1997    41.8 22227415      635.     10
```
{{< /spoiler >}}


Note that all `dplyr` related commands work in the same way:

+ The first argument is a data frame.
+ The subsequent arguments describe what to do with the data frame, using the variable names **without quotes** or sometimes some vectors of numbers.
+ The result is a new data frame.


### Using the `%>%` (pipe) operator

A very convenient feature of tidyverse is the `%>%` operator. 
To understand how it works, we can reproduce the precedent example using it. 


```r
data %>% slice(2:5)
```

{{< spoiler text="Click to view the results" >}}

```
## # A tibble: 4 x 7
##   country     continent  year lifeExp      pop gdpPercap row_no
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Afghanistan Asia       1957    30.3  9240934      821.      2
## 2 Afghanistan Asia       1962    32.0 10267083      853.      3
## 3 Afghanistan Asia       1967    34.0 11537966      836.      4
## 4 Afghanistan Asia       1972    36.1 13079460      740.      5
```
{{< /spoiler >}}


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
(note: we have chained the command with head(10) using the pipe to display only the first ten rows of the new data set)

{{% /callout%}}

### slice helpers

Note you can use several variants of `slice()` for specific uses - we will sometimes call them helpers. In particular, you may want to look at the commands `slice_head()` and `slice_tail()` to select the first or last $n$ rows of your data set.

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


## Randomly selecting rows{#random-rows}

A more interesting application is the creation of a random subsample of cases (i.e., rows).
Again, you can use a slice helper`slice_sample()` which allows you to random select with or without replacement.


```r
set.seed(12345)
data %>% slice_sample(n = 5)
data %>% slice_sample(n = 5, replace = TRUE)
```

{{< spoiler text="Click to view the results" >}}

```
## # A tibble: 5 x 7
##   country   continent  year lifeExp       pop gdpPercap row_no
##   <fct>     <fct>     <int>   <dbl>     <int>     <dbl>  <int>
## 1 Bolivia   Americas   1997    62.0   7693188     3326.    142
## 2 Argentina Americas   1962    65.1  21283783     7133.     51
## 3 Indonesia Asia       2007    70.6 223547000     3541.    720
## 4 Iran      Asia       1997    68.0  63327987     8264.    730
## 5 Portugal  Europe     1987    74.1   9915289    13039.   1244
```

```
## # A tibble: 5 x 7
##   country          continent  year lifeExp      pop gdpPercap row_no
##   <fct>            <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Hong Kong, China Asia       1967    70    3722800     6198.    664
## 2 Kenya            Africa     1997    54.4 28263827     1360.    826
## 3 Guatemala        Americas   1972    53.7  5149581     4031.    605
## 4 Ghana            Africa     2002    58.5 20550751     1112.    587
## 5 Slovak Republic  Europe     1987    71.1  5199318    12037.   1376
```
{{< /spoiler >}}



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
##  1 Afghanistan Asia       1987    40.8 13867957      852.      8
##  2 Afghanistan Asia       1957    30.3  9240934      821.      2
##  3 Afghanistan Asia       1977    38.4 14880372      786.      6
##  4 Afghanistan Asia       1977    38.4 14880372      786.      6
##  5 Afghanistan Asia       1982    39.9 12881816      978.      7
##  6 Afghanistan Asia       1997    41.8 22227415      635.     10
##  7 Afghanistan Asia       1952    28.8  8425333      779.      1
##  8 Afghanistan Asia       1987    40.8 13867957      852.      8
##  9 Afghanistan Asia       1982    39.9 12881816      978.      7
## 10 Afghanistan Asia       1977    38.4 14880372      786.      6
## 11 Afghanistan Asia       1952    28.8  8425333      779.      1
## 12 Afghanistan Asia       1967    34.0 11537966      836.      4
## 13 Afghanistan Asia       1987    40.8 13867957      852.      8
## 14 Afghanistan Asia       1997    41.8 22227415      635.     10
## 15 Afghanistan Asia       1962    32.0 10267083      853.      3
## 16 Afghanistan Asia       1992    41.7 16317921      649.      9
## 17 Afghanistan Asia       1967    34.0 11537966      836.      4
## 18 Afghanistan Asia       1997    41.8 22227415      635.     10
## 19 Afghanistan Asia       1982    39.9 12881816      978.      7
## 20 Afghanistan Asia       1957    30.3  9240934      821.      2
```
{{< /spoiler >}}

## Selecting rows based on logical criteria{#logical-criteria}

Let say you want to look only at South Africa data. 

We will now use another command from the dplyr package : `filter()`


```r
SAdata <- filter(data, country=="South Africa")
SAdata
```

{{< spoiler text="Click to view the results" >}}

```
## # A tibble: 12 x 7
##    country      continent  year lifeExp      pop gdpPercap row_no
##    <fct>        <fct>     <int>   <dbl>    <int>     <dbl>  <int>
##  1 South Africa Africa     1952    45.0 14264935     4725.   1405
##  2 South Africa Africa     1957    48.0 16151549     5487.   1406
##  3 South Africa Africa     1962    50.0 18356657     5769.   1407
##  4 South Africa Africa     1967    51.9 20997321     7114.   1408
##  5 South Africa Africa     1972    53.7 23935810     7766.   1409
##  6 South Africa Africa     1977    55.5 27129932     8029.   1410
##  7 South Africa Africa     1982    58.2 31140029     8568.   1411
##  8 South Africa Africa     1987    60.8 35933379     7826.   1412
##  9 South Africa Africa     1992    61.9 39964159     7225.   1413
## 10 South Africa Africa     1997    60.2 42835005     7479.   1414
## 11 South Africa Africa     2002    53.4 44433622     7711.   1415
## 12 South Africa Africa     2007    49.3 43997828     9270.   1416
```
{{< /spoiler >}}


Note again how the command worked :

+ The first argument is a data frame.
+ The subsequent arguments describe what to do with the data frame, using the variable names *without quotes*.
+ The result is a new data frame that we named `SAdata`

You can create more complicated criteria using functions and operators such as:

+ `==`, `>`, `>=`,  `<`, `<=`  (when constructing comparison expressions)
+ `%in%` when checking presence in a list of possible value
+ `&` (and), `|` (or), `!`  (not) (when combining expression)

In the following example, we are selecting South Africa records for years after 2000:



```r
SA2000 <- filter(data, country=="South Africa" & year > 2000)
```

{{< spoiler text="Click to see the result" >}} 

{{< /spoiler >}} 


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

## More advanced sampling

We will now use again with the gapminder data. 

Let's first select a sub-sample. We want to select only two years (2002 and 2007), and we want to select randomly ten countries.

A first idea would be to use the command `slice_sample()`, as in:


```r
set.seed(123)
data <- gapminder %>% 
  filter(year > 2000 ) %>%
  slice_sample(n=10) %>% arrange(country)
data
```

{{< spoiler text="Click to view the results" >}}

```
## # A tibble: 10 x 6
##    country         continent  year lifeExp        pop gdpPercap
##    <fct>           <fct>     <int>   <dbl>      <int>     <dbl>
##  1 Austria         Europe     2007    79.8    8199783    36126.
##  2 France          Europe     2007    80.7   61083916    30470.
##  3 Gabon           Africa     2002    56.8    1299304    12522.
##  4 India           Asia       2007    64.7 1110396331     2452.
##  5 Madagascar      Africa     2002    57.3   16473477      895.
##  6 Nepal           Asia       2002    61.3   25873917     1057.
##  7 Pakistan        Asia       2002    63.6  153403524     2093.
##  8 Slovak Republic Europe     2002    73.8    5410052    13639.
##  9 Swaziland       Africa     2007    39.6    1133066     4513.
## 10 Zimbabwe        Africa     2002    40.0   11926563      672.
```
{{< /spoiler >}}

The problem with this approach is that each country has two records. So with "blind" line sampling, we will encounter cases where we only have one record for a country. That is what happened here, and that is not what we wanted.

What we really want is a subsample of countries, and once a country is selected, we want to see all the records of that selected country. To do this, we need a different approach and use the base command `sample()`:


```r
# using tidyverse approach:
random_names <- gapminder %>% 
  select(country) %>%  
  unique() %>%      # take only one record per country
  as_vector() %>%   # as_vector transforms a data.frame into a vector
  sample(10, replace = FALSE)   # take a 10 random sample 

# sometimes tidyverse can be long
# it might be easier to use the following command:
set.seed(123)
random_names <- sample(unique(data$country), 10, replace=FALSE)
random_names
```

```
##  [1] Gabon           Zimbabwe        France          Slovak Republic
##  [5] Nepal           Swaziland       Austria         Pakistan       
##  [9] Madagascar      India          
## 142 Levels: Afghanistan Albania Algeria Angola Argentina Australia ... Zimbabwe
```

Now, we can use this vector of country names to select the corresponding countries


```r
gapminder %>% filter(country %in% random_names & year > 2000) 
```

{{< spoiler text="Click to view the results" >}}

```
## # A tibble: 20 x 6
##    country         continent  year lifeExp        pop gdpPercap
##    <fct>           <fct>     <int>   <dbl>      <int>     <dbl>
##  1 Austria         Europe     2002    79.0    8148312    32418.
##  2 Austria         Europe     2007    79.8    8199783    36126.
##  3 France          Europe     2002    79.6   59925035    28926.
##  4 France          Europe     2007    80.7   61083916    30470.
##  5 Gabon           Africa     2002    56.8    1299304    12522.
##  6 Gabon           Africa     2007    56.7    1454867    13206.
##  7 India           Asia       2002    62.9 1034172547     1747.
##  8 India           Asia       2007    64.7 1110396331     2452.
##  9 Madagascar      Africa     2002    57.3   16473477      895.
## 10 Madagascar      Africa     2007    59.4   19167654     1045.
## 11 Nepal           Asia       2002    61.3   25873917     1057.
## 12 Nepal           Asia       2007    63.8   28901790     1091.
## 13 Pakistan        Asia       2002    63.6  153403524     2093.
## 14 Pakistan        Asia       2007    65.5  169270617     2606.
## 15 Slovak Republic Europe     2002    73.8    5410052    13639.
## 16 Slovak Republic Europe     2007    74.7    5447502    18678.
## 17 Swaziland       Africa     2002    43.9    1130269     4128.
## 18 Swaziland       Africa     2007    39.6    1133066     4513.
## 19 Zimbabwe        Africa     2002    40.0   11926563      672.
## 20 Zimbabwe        Africa     2007    43.5   12311143      470.
```
{{< /spoiler >}}

[A publication]({{< relref "x" >}})

