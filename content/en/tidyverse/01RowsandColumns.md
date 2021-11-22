---
title: Work with rows and columns
linktitle: Work with rows and columns
slug: row-col-tidyverse
type: book
date: "2021-02-02T00:00:00+01:00"

weight: 30
output:
  html_document:
    keep_md: true
---

## Learning objectives

You will learn how to:

+ [Sort rows](#sort-rows)
+ [Select then order rows](#select-sort-rows)
+ [Select a subset of columns](#subsetting-columns)
+ [Select a subset of rows and columns](#subsetting-rows-and-columns)



## Sort rows

You are now familiar with `dplyr` commands and the pipe syntax. A second type of manipulation is the ordering of your data. For this, we will use the command `arrange()`.

The command `arrange()` changes the order of the rows. The argument is a set of column names to order by. 


```r
data %>% arrange(country) 
```

{{< spoiler text="Click to see the output" >}}

```
## # A tibble: 10 x 7
##    country     continent  year lifeExp      pop gdpPercap row_no
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>  <int>
##  1 Afghanistan Asia       1952    28.8  8425333      779.      1
##  2 Afghanistan Asia       1957    30.3  9240934      821.      2
##  3 Afghanistan Asia       1962    32.0 10267083      853.      3
##  4 Afghanistan Asia       1967    34.0 11537966      836.      4
##  5 Afghanistan Asia       1972    36.1 13079460      740.      5
##  6 Afghanistan Asia       1977    38.4 14880372      786.      6
##  7 Afghanistan Asia       1982    39.9 12881816      978.      7
##  8 Afghanistan Asia       1987    40.8 13867957      852.      8
##  9 Afghanistan Asia       1992    41.7 16317921      649.      9
## 10 Afghanistan Asia       1997    41.8 22227415      635.     10
```
Note: only the first 10 rows are shown here
{{< /spoiler >}}



If you provide more than one column name, the column names are separated with commas.Each additional column will be used to break ties in the values of preceding columns. 


```r
data %>% arrange(continent, country, year)
```

{{< spoiler text="Click to see the output" >}}

```
## # A tibble: 10 x 7
##    country continent  year lifeExp      pop gdpPercap row_no
##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>  <int>
##  1 Algeria Africa     1952    43.1  9279525     2449.     25
##  2 Algeria Africa     1957    45.7 10270856     3014.     26
##  3 Algeria Africa     1962    48.3 11000948     2551.     27
##  4 Algeria Africa     1967    51.4 12760499     3247.     28
##  5 Algeria Africa     1972    54.5 14760787     4183.     29
##  6 Algeria Africa     1977    58.0 17152804     4910.     30
##  7 Algeria Africa     1982    61.4 20033753     5745.     31
##  8 Algeria Africa     1987    65.8 23254956     5681.     32
##  9 Algeria Africa     1992    67.7 26298373     5023.     33
## 10 Algeria Africa     1997    69.2 29072015     4797.     34
```
Note: only the first 10 rows are shown here
{{< /spoiler >}}


Use desc() to re-order by a column in descending order:


```r
data %>% arrange(desc(continent), country)
```

{{< spoiler text="Click to see the output" >}}

```
## # A tibble: 10 x 7
##    country   continent  year lifeExp      pop gdpPercap row_no
##    <fct>     <fct>     <int>   <dbl>    <int>     <dbl>  <int>
##  1 Australia Oceania    1952    69.1  8691212    10040.     61
##  2 Australia Oceania    1957    70.3  9712569    10950.     62
##  3 Australia Oceania    1962    70.9 10794968    12217.     63
##  4 Australia Oceania    1967    71.1 11872264    14526.     64
##  5 Australia Oceania    1972    71.9 13177000    16789.     65
##  6 Australia Oceania    1977    73.5 14074100    18334.     66
##  7 Australia Oceania    1982    74.7 15184200    19477.     67
##  8 Australia Oceania    1987    76.3 16257249    21889.     68
##  9 Australia Oceania    1992    77.6 17481977    23425.     69
## 10 Australia Oceania    1997    78.8 18565243    26998.     70
```
Note: only the first 10 rows are shown here
{{< /spoiler >}}


## Chaining data treatments: selecting and sorting rows {#select-sort-rows}

You have been introduced to the chaining of commands. It is now time to start to feel the advantages of this way of writing commands. 

For exemple, let's select the data of year 2007 and the African continent. Sort the results  in descending order of GDP and show the first 5 results.


```r
data %>% 
  filter(continent == "Africa", year ==2007) %>%
  arrange(-gdpPercap) %>%
  slice_head(n=6)
```

```
## # A tibble: 6 x 7
##   country           continent  year lifeExp      pop gdpPercap row_no
##   <fct>             <fct>     <int>   <dbl>    <int>     <dbl>  <int>
## 1 Gabon             Africa     2007    56.7  1454867    13206.    552
## 2 Botswana          Africa     2007    50.7  1639131    12570.    168
## 3 Equatorial Guinea Africa     2007    51.6   551201    12154.    492
## 4 Libya             Africa     2007    74.0  6036914    12057.    912
## 5 Mauritius         Africa     2007    72.8  1250882    10957.    984
## 6 South Africa      Africa     2007    49.3 43997828     9270.   1416
```

_Note that this becomes very easy to follow the succesive data treatments. Beside, with that solution you do not need an intermediate variable._


## Select columns/variables {#subsetting-columns}

When you have a large survey with many variables, it will often be useful to extract the specific variables useful for a particular type of analysis. 

The function `select()` will allow you do create a subset of variables based on their names.

### column positions
A first approach is to list the column positions.  


```r
data %>% select(1, 3, 6) 
```

```
## # A tibble: 1,704 x 3
##    country      year gdpPercap
##    <fct>       <int>     <dbl>
##  1 Afghanistan  1952      779.
##  2 Afghanistan  1957      821.
##  3 Afghanistan  1962      853.
##  4 Afghanistan  1967      836.
##  5 Afghanistan  1972      740.
##  6 Afghanistan  1977      786.
##  7 Afghanistan  1982      978.
##  8 Afghanistan  1987      852.
##  9 Afghanistan  1992      649.
## 10 Afghanistan  1997      635.
## # ... with 1,694 more rows
```

However, this is not really recommended as the order of your columns could change, and lead to unexpected manipulations.

### Column names

A second approach is to list all the names of the variables you want to select

```r
data %>% select(country, year, gdpPercap) %>% slice_head(n=4)
```

```
## # A tibble: 4 x 3
##   country      year gdpPercap
##   <fct>       <int>     <dbl>
## 1 Afghanistan  1952      779.
## 2 Afghanistan  1957      821.
## 3 Afghanistan  1962      853.
## 4 Afghanistan  1967      836.
```

However, it can become rapidly cumbersome to have to write a long list of variable names. Hopefully, you can use a few tricks.

#### Select ranges of variables
You can provide a range of variables or a range of column positions


```r
# This will select of the variables between the column continent and pop
data %>% select(continent:pop) %>% head(4)
```

```
## # A tibble: 4 x 4
##   continent  year lifeExp      pop
##   <fct>     <int>   <dbl>    <int>
## 1 Asia       1952    28.8  8425333
## 2 Asia       1957    30.3  9240934
## 3 Asia       1962    32.0 10267083
## 4 Asia       1967    34.0 11537966
```

#### Exclude instead of selecting
If you need to select most variables and exclude only a few variables you can use the sign `-` to signal the variables you want to exclude:


```r
data %>% select(-gdpPercap) %>% head(4) 
```

```
## # A tibble: 4 x 6
##   country     continent  year lifeExp      pop row_no
##   <fct>       <fct>     <int>   <dbl>    <int>  <int>
## 1 Afghanistan Asia       1952    28.8  8425333      1
## 2 Afghanistan Asia       1957    30.3  9240934      2
## 3 Afghanistan Asia       1962    32.0 10267083      3
## 4 Afghanistan Asia       1967    34.0 11537966      4
```

#### Select by type

You can select the variables by their type or any other characteristic using `where()`. 

`where()` applies a function to all variables and selects those for which the function returns TRUE.


```r
# will select only the numeric variables
data %>% select(where(is.numeric)) %>% head(5)
```

```
## # A tibble: 5 x 5
##    year lifeExp      pop gdpPercap row_no
##   <int>   <dbl>    <int>     <dbl>  <int>
## 1  1952    28.8  8425333      779.      1
## 2  1957    30.3  9240934      821.      2
## 3  1962    32.0 10267083      853.      3
## 4  1967    34.0 11537966      836.      4
## 5  1972    36.1 13079460      740.      5
```


#### Use additional helpers on variable names
To make your life easier, you can also use a number of helper function within select


```r
# Select of the variables with names starting with C
data %>% select(starts_with("c"))
```

```
## # A tibble: 1,704 x 2
##    country     continent
##    <fct>       <fct>    
##  1 Afghanistan Asia     
##  2 Afghanistan Asia     
##  3 Afghanistan Asia     
##  4 Afghanistan Asia     
##  5 Afghanistan Asia     
##  6 Afghanistan Asia     
##  7 Afghanistan Asia     
##  8 Afghanistan Asia     
##  9 Afghanistan Asia     
## 10 Afghanistan Asia     
## # ... with 1,694 more rows
```


```r
# Select of the variables with names containing O
data %>% select(contains("O"))
```

```
## # A tibble: 1,704 x 4
##    country     continent      pop row_no
##    <fct>       <fct>        <int>  <int>
##  1 Afghanistan Asia       8425333      1
##  2 Afghanistan Asia       9240934      2
##  3 Afghanistan Asia      10267083      3
##  4 Afghanistan Asia      11537966      4
##  5 Afghanistan Asia      13079460      5
##  6 Afghanistan Asia      14880372      6
##  7 Afghanistan Asia      12881816      7
##  8 Afghanistan Asia      13867957      8
##  9 Afghanistan Asia      16317921      9
## 10 Afghanistan Asia      22227415     10
## # ... with 1,694 more rows
```

It was probably not convincing with such an exemple, but imagine that you have a data set with information collected at different time, and you have variables such as x2000, x2001, x2002, x2003


```r
# Presented for reference only, we do not have variables x2000, x2001, x2002, x2003!
data %>% select(starts_with("x"))
```

#### Use a combination of these strategies

Note that you can mix the strategies to obtain a list of variables. For example, you want to select the row_no and year variables and the variables containing "O". 


```r
data %>% select(row_no, year, contains("o")) %>% head(5)
```

```
## # A tibble: 5 x 5
##   row_no  year country     continent      pop
##    <int> <int> <fct>       <fct>        <int>
## 1      1  1952 Afghanistan Asia       8425333
## 2      2  1957 Afghanistan Asia       9240934
## 3      3  1962 Afghanistan Asia      10267083
## 4      4  1967 Afghanistan Asia      11537966
## 5      5  1972 Afghanistan Asia      13079460
```

### Reordering columns

Sometimes, you just want to re-order the variables. Indeed you can do this with select, but it can still be cumbersome if you need to move only a few among many colmuns.

In such case you can use the function `relocate()`. 

The default behaviour corresponds to the case where you want to move some variables to the front:


```r
# this will make the row_no variable as the first colum
data %>% relocate(row_no) %>% head(3)
```

```
## # A tibble: 3 x 7
##   row_no country     continent  year lifeExp      pop gdpPercap
##    <int> <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1      1 Afghanistan Asia       1952    28.8  8425333      779.
## 2      2 Afghanistan Asia       1957    30.3  9240934      821.
## 3      3 Afghanistan Asia       1962    32.0 10267083      853.
```

Use the function helper if you need more sophisticated changes.


## Selecting rows and columns {#subsetting-rows-and-columns}

You have all the tools to select rows and columns.
Using tidyverse, you will chain each activities. For example, you want to select the column country and pop, and consider only the African countries:


```r
data %>% filter(continent == "Africa") %>%
  select(country, pop)
```

```
## # A tibble: 624 x 2
##    country      pop
##    <fct>      <int>
##  1 Algeria  9279525
##  2 Algeria 10270856
##  3 Algeria 11000948
##  4 Algeria 12760499
##  5 Algeria 14760787
##  6 Algeria 17152804
##  7 Algeria 20033753
##  8 Algeria 23254956
##  9 Algeria 26298373
## 10 Algeria 29072015
## # ... with 614 more rows
```

Note that the order of the commands can be important. In our example, we selected the row where continent was "Africa" and then selected two variables country and pop. Imagine what would happen if you interchange the two commands:


```r
data %>%  select(country, pop) %>%
  filter(continent == "Africa") 
```

## Exercises

### Exercise 1: 

Show the 2007 GDP per capita of the 10 most populated countries (in 2007) in descending order of population

{{% callout solution%}}
{{< spoiler text="Click to view the solution" >}}



```r
data %>% filter(year==2007) %>%
  arrange(-pop) %>%
  select(country, pop, gdpPercap) %>%
  slice_head(n=10)
```

```
## # A tibble: 10 x 3
##    country              pop gdpPercap
##    <fct>              <int>     <dbl>
##  1 China         1318683096     4959.
##  2 India         1110396331     2452.
##  3 United States  301139947    42952.
##  4 Indonesia      223547000     3541.
##  5 Brazil         190010647     9066.
##  6 Pakistan       169270617     2606.
##  7 Bangladesh     150448339     1391.
##  8 Nigeria        135031164     2014.
##  9 Japan          127467972    31656.
## 10 Mexico         108700891    11978.
```
{{< /spoiler >}}
{{% /callout %}}

### Exercise 2: 

Show the 2007 life expectency (in 2007). Order by descending GDP per capita. Show the countries with the 5 highest GDP per Capita and the 5 lowest GDP per capita.

{{% callout solution%}}
{{< spoiler text="Click to view the solution" >}}


```r
data %>% filter(year==2007) %>%
  arrange(-gdpPercap) %>%
  select(country, lifeExp, gdpPercap) %>%
  slice(1:5, (n()-4):n())  #be careful with the parenthesis
```

```
## # A tibble: 10 x 3
##    country          lifeExp gdpPercap
##    <fct>              <dbl>     <dbl>
##  1 Norway              80.2    49357.
##  2 Kuwait              77.6    47307.
##  3 Singapore           80.0    47143.
##  4 United States       78.2    42952.
##  5 Ireland             78.9    40676.
##  6 Guinea-Bissau       46.4      579.
##  7 Zimbabwe            43.5      470.
##  8 Burundi             49.6      430.
##  9 Liberia             45.7      415.
## 10 Congo, Dem. Rep.    46.5      278.
```
{{< /spoiler >}}
{{% /callout %}}

