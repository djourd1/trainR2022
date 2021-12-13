---
title: Factors
author: Damien Jourdain
slug: factors
categories:
  - data structure
tags: []
type: book
weight: 30
output:
  html_document:
    keep_md: true

---

## Learning objectives

Factors are used to store categorical variables, i.e. variables that take a limited number of different values. Categorical variables enter statistical models differently than continuous variables, which is why R developers have created a specific type of data to ensure that the modeling functions will handle this data correctly. Another reason why factors were created is that they store data in levels to reduce data redundancy and save a lot of space in the memory.

Here you will learn how to create and manipulate factors. 


## Factors

Factors are stored as a vector of strings - the levels.

### Convert a vector 

To create a factor, we use the function `factor()`. The only required argument to factor is a vector of values which will be returned as a vector of factor values. Both numeric and character variables can be made into factors, but a factor's levels will always be character values. You can see the possible levels for a factor through the `levels()` command. 


```r
# Transform character vector into a factor
gender = c("male","female","female","male","female","female")
gender.fac = factor(gender)
gender.fac
```

```
## [1] male   female female male   female female
## Levels: female male
```

You can examine the structure of a factor function by using `str(gender.fac)`

```r
# examine the structure of the factor gender.fac
str(gender.fac)
```

```
##  Factor w/ 2 levels "female","male": 2 1 1 2 1 1
```

You can see that gender.fac is a factor with 2 levels. The function `factor` converted the character vector into a vector of integer values. "Female" is the first level encoded as 1 whereas the "Male" is the second level, encoded as 2.

We can see that creating a factor is an efficient way to store a vector of character values, because each unique character value is stored only once, and the data itself is stored as a vector of integers. 

### Changing the order of the levels

The levels of a factor are used when displaying the factor. You can change these levels at the time you create a factor by passing a vector with the new values through the `levels= argument`. 

To convert the vector gender into a factor where Male is coded 1, and Female is coded 2, we use the following command:


```r
gender
```

```
## [1] "male"   "female" "female" "male"   "female" "female"
```

```r
gender.fac2 = factor(gender, levels=c("Male", "Female"))
gender.fac2
```

```
## [1] <NA> <NA> <NA> <NA> <NA> <NA>
## Levels: Male Female
```

```r
str(gender.fac2)
```

```
##  Factor w/ 2 levels "Male","Female": NA NA NA NA NA NA
```

The manipulation of factor levels becomes very interesting when you want to control the order of appearance of the different levels.

Consider a vector theMonths containing a list of months:


```r
theMonths = c("March","April","March", "January","November","January",
 "September","October","September","November","August",
 "January","November","November","February","May","August",
 "July","December","August","August","September","November",
 "February","April")
 theMonths <- factor(theMonths)
 theMonths
```

```
##  [1] March     April     March     January   November  January   September
##  [8] October   September November  August    January   November  November 
## [15] February  May       August    July      December  August    August   
## [22] September November  February  April    
## 11 Levels: April August December February January July March May ... September
```
By default, the levels will be coded using alphabetic order. As a result, April is coded 1, August is coded 2, etc. In the case of Months, this will make the readings of results a bit difficult when summarizing information.
 
For example, the function `table()` will tell us how many times each month has appeared in our vector theMonths


```r
table(theMonths)
```

```
## theMonths
##     April    August  December  February   January      July     March       May 
##         2         4         1         2         3         1         2         1 
##  November   October September 
##         5         1         3
```

If you want to circonvent this problem, you can use the levels argument when creating your vector theMonths


```r
theMonths_ord <- factor(theMonths,levels=c("January","February","March",
             "April","May","June","July","August","September",
             "October","November","December"))

table(theMonths_ord)
```

```
## theMonths_ord
##   January  February     March     April       May      June      July    August 
##         3         2         2         2         1         0         1         4 
## September   October  November  December 
##         3         1         5         1
```


### Ordered vs. Unordered Factors

Although the months clearly have an ordering, this is not reflected in the output of the table function. 


```r
theMonths <- factor(theMonths,levels=c("January","February","March",
             "April","May","June","July","August","September",
             "October","November","December"))

table(theMonths)
```

```
## theMonths
##   January  February     March     April       May      June      July    August 
##         3         2         2         2         1         0         1         4 
## September   October  November  December 
##         3         1         5         1
```

```r
theMonths[2] > theMonths[3]
```

```
## Warning in Ops.factor(theMonths[2], theMonths[3]): '>' not meaningful for
## factors
```

```
## [1] NA
```
 
As the results of the last operation shows, the comparison operators are not supported for unordered factors. Creating an ordered factor solves this problem: 


```r
theMonths <- factor(theMonths,levels=c("January","February","March",
             "April","May","June","July","August","September",
             "October","November","December"), ordered = TRUE)

theMonths[2] > theMonths[3]  # now we can compare January and July
```

```
## [1] TRUE
```

### Creating a factor using the function cut()

Another common way to create factors is to split a continuous variables into intervals using the `cut` function. The `breaks= argument` to cut is used to describe how ranges of numbers will be converted to factor values. If a number is provided through the `breaks= argument`, the resulting factor will be created by dividing the range of the variable into that number of equal length intervals; if a vector of values is provided, the values in the vector are used to determine the breakpoint. Note that if a vector of values is provided, the number of levels of the resultant factor will be one less than the number of values in the vector.

For example, consider a vector `women.heights` where you recorded the height for a sample of women. If we wanted to create a factor corresponding to weight, with three equally-spaced levels, we could use the following:


```r
women.heights <- c(158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171, 172, 155)
women.heights.factor = cut(women.heights,3)  
women.heights.factor
```

```
##  [1] (155,161] (155,161] (155,161] (161,166] (161,166] (161,166] (161,166]
##  [8] (161,166] (161,166] (166,172] (166,172] (166,172] (166,172] (166,172]
## [15] (166,172] (155,161]
## Levels: (155,161] (161,166] (166,172]
```

```r
table(women.heights.factor)
```

```
## women.heights.factor
## (155,161] (161,166] (166,172] 
##         4         6         6
```

If you want to change the way the factor levels are displayed, use the `labels= argument`. It allows you to specify the levels of the factors, instead of the intervals:


```r
women.heights.factor2 = cut(women.heights,3,labels=c('Low','Medium','High'))
table(women.heights.factor2)
```

```
## women.heights.factor2
##    Low Medium   High 
##      4      6      6
```

## Exercises

### Exercise 1 

R provides a predefined variable `month.abb` with the first three letters of months:


```
##  [1] "Jan" "Feb" "Mar" "Apr" "May" "Jun" "Jul" "Aug" "Sep" "Oct" "Nov" "Dec"
```

1. Using month.abb, create a random sample of 20 months selected from month.abb and transform it as a factor; when doing so, make sure the coding will not be made according to alphabetic order of the months.
2. Count the number of times each month has been sampled using the function table()


{{< spoiler text="Show the answer" >}}

For the first question, you can use the function sample with the argument `replace = TRUE`

```r
themonths <- sample(month.abb, 30, replace = TRUE)
themonths.factor <- factor(themonths, levels=month.abb)
themonths.factor
```

```
##  [1] Jun Jun Mar Sep Apr Jun Jul Jun Sep May Nov Aug Jul May Sep Sep Aug Jan May
## [20] Apr Dec Feb Jan Jun Mar May Jul Apr Aug May
## Levels: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec
```

For the second question, you can use the function table

```r
table(themonths.factor)
```

```
## themonths.factor
## Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec 
##   2   1   2   3   5   5   3   3   4   0   1   1
```

{{< /spoiler >}}

