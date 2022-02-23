---
title: "Group and summarize your data" 
author: Damien Jourdain
date: '2022-02-23'
slug: summarize-data
categories:
  - R
tags: []
type: book
weight: 30

output:
  html_document:
    keep_md: true
---



## Objectives

At times, you do not need all the detailed information available, but you will only be interested in the summarized data. As an example, with our gapminder data, you may only be interested by the country means of population or GDP data. 

To do that, you will need to use the `summarize` and `group_by` functions.

## Group and summarize data

+ group_by: allows you to group by a one or more variables.
+ summarize:  creates a new data.frame containing calculated summary information about a grouped variable.

For example, if you want the mean of each country's population, you would use the following sequence of commands:

1. indicate what grouping you want to use using the `group_by` function
2. create the summary variables using the `summarize` function.

In our case, to make things easier to read, we divided the data per 1,000,000 and rounded the figures.


```r
data %>% 
  group_by(country) %>%
  summarize(m_pop = round(mean(pop/1000000),1))
```

```
## # A tibble: 142 x 2
##    country     m_pop
##    <fct>       <dbl>
##  1 Afghanistan  15.8
##  2 Albania       2.6
##  3 Algeria      19.9
##  4 Angola        7.3
##  5 Argentina    28.6
##  6 Australia    14.6
##  7 Austria       7.6
##  8 Bahrain       0.4
##  9 Bangladesh   90.8
## 10 Belgium       9.7
## # ... with 132 more rows
```

Note that you created a new data set with only 142 rows (one per available country). 

The data available per country may vary between countries, so you may want to add another variable indicating the number of records are summarized for each country. For this, you can add another variable and use the function `n()`.


```r
data %>% 
  group_by(country) %>%
  summarize(m_pop = round(mean(pop/1000000),1), N = n())
```

```
## # A tibble: 142 x 3
##    country     m_pop     N
##    <fct>       <dbl> <int>
##  1 Afghanistan  15.8    12
##  2 Albania       2.6    12
##  3 Algeria      19.9    12
##  4 Angola        7.3    12
##  5 Argentina    28.6    12
##  6 Australia    14.6    12
##  7 Austria       7.6    12
##  8 Bahrain       0.4    12
##  9 Bangladesh   90.8    12
## 10 Belgium       9.7    12
## # ... with 132 more rows
```

You can see that you can create as many new variables as you want, and organize them as needed.

Sometimes, the analysis you will want to carry can be more complex. For example, you may want to calculate the percentage growth over the year for each country, and sort them by growth rate.

This is quite complex. But in fact, you can see that tidyverse can help us in decomposing this seemingly complex task into simple sub-tasks.

As a piece of advice, before you start to code, think about your ultimate goal. Rather than simply jumping into coding, you will be better of if you plan things out !

In particular, what we call **pseudocode** can be very useful for organization your coding operations. It allows you to think through what you wish to accomplish and the most efficient way to go about it BEFORE you write your code.

To do so, write out the steps that you will need to implement in order to achieve your goal. It's ok if you don't know all of the functions yet to implement this. Organize first, look up functions second. 

In our case, we want to see the difference in population between the first year (1952) and the last year (2007). So we will want 

1. keep data related to 1952 and 2007
2. you want to summarize the information at the country level, group the information per country
3. find a function that can make calculations using previous rows (a research will lead you to the function lag()), and calculate the difference in population between these two years.
4. Keep the columns that are useful 
4. Sort the results per growth rate.

Once you have figured out the right sequence of elementary tasks, writing the code becomes very easy. As usual, for readibilty of your code, you will find useful to use one row for one task:



```r
data %>% 
  filter(year %in% c(1952, 2007)) %>%
  group_by(country) %>%
  mutate(diffPop = pop - lag(pop), growthPop = diffPop/lag(pop)) %>%
  select(country, year, pop, diffPop, growthPop) %>% 
  filter(!is.na(growthPop)) %>%
  arrange(desc(growthPop))
```

```
## # A tibble: 142 x 5
## # Groups:   country [142]
##    country        year      pop  diffPop growthPop
##    <fct>         <int>    <int>    <int>     <dbl>
##  1 Kuwait         2007  2505559  2345559     14.7 
##  2 Jordan         2007  6053193  5445279      8.96
##  3 Djibouti       2007   496374   433225      6.86
##  4 Saudi Arabia   2007 27601038 23595361      5.89
##  5 Oman           2007  3204897  2697064      5.31
##  6 Cote d'Ivoire  2007 18013409 15036390      5.05
##  7 Gambia         2007  1688359  1404039      4.94
##  8 Libya          2007  6036914  5017185      4.92
##  9 Bahrain        2007   708573   588126      4.88
## 10 Kenya          2007 35610177 29146131      4.51
## # ... with 132 more rows
```


### Exercise: 

Calculate the average and standard deviation of the growth of the GDP between 2002 and 2007 per capita for each continent. Sort your results by decreasing growth. You will need some pseudo-coding !!


{{< spoiler text="Click to see the solution" >}} 
Let's think about the different elementary tasks before we can reach our goal:

1. You will need the growth of GDP for each country first

  +  Select the years 2002 and 2007
  + Calculate the growth per country
  + Eliminate the NA values

2. Summarize the information per continent. So we need to:

  + Group by continent
  + Create summary variables

3. Sort your final results



```r
data %>% 
  filter(year %in% c(2002, 2007)) %>%
  group_by(country) %>%
  mutate(diffGDP = gdpPercap - lag(gdpPercap), growthGDP = diffGDP/lag(gdpPercap)) %>%
  filter(!is.na(growthGDP)) %>%
  group_by(continent) %>%
  summarize(mGrowth = mean(growthGDP), sdGrowth = sd(growthGDP))  %>%
  arrange(desc(mGrowth))
```

```
## # A tibble: 5 x 3
##   continent mGrowth sdGrowth
##   <fct>       <dbl>    <dbl>
## 1 Asia        0.238   0.213 
## 2 Europe      0.199   0.114 
## 3 Americas    0.199   0.151 
## 4 Africa      0.139   0.172 
## 5 Oceania     0.104   0.0255
```

{{< /spoiler >}}

