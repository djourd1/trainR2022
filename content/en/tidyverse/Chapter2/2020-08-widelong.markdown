---
title: "From wide to long" 
author: Damien Jourdain
date: '2020-08-20'
slug: widetolong
type: book
weight: 2
---



## The original data

Let's suppose that you capture data in the following format:

+ one line per respondent
+ one column per repeated answer (recorded here as ans_1, ans_2 and ans_3)
+ additional columns to describe the respondents



```
## # A tibble: 5 x 6
##   resp_id   age city      ans_1 ans_2 ans_3
##     <int> <int> <chr>     <int> <int> <int>
## 1       1    43 Cape Town     2     1     1
## 2       2    48 Pretoria     NA     2     2
## 3       3    45 Cape Town     3     3     2
## 4       4    40 Pretoria      2     1     2
## 5       5    31 Cape Town     2     2     1
```

## What do you want to obtain?

If you want to reshape your table as a long format, you will shape in order to obain

+ one line per answer, per respondent
+ one column indicating the actual answer
+ additional columns to describe the respondents

## Use pivot_longer()

To do so, we will use the command `pivot_longer()` that is part of the `tidyr` package (a member of the tidyverse family)

So do not forget to load the tidyverse package:

```r
library(tidyverse)
```


The easiest way to pivot the table is to identify the columns that will go from columns to rows.
In our case, we want the columns ans_1, ans_2, ans_3 to be appearing as rows. We will provide the vector of columns that will be pivoted:


```r
pivot_longer(dt, cols=c(ans_1, ans_2, ans_3))
```

```
## # A tibble: 15 x 5
##    resp_id   age city      name  value
##      <int> <int> <chr>     <chr> <int>
##  1       1    43 Cape Town ans_1     2
##  2       1    43 Cape Town ans_2     1
##  3       1    43 Cape Town ans_3     1
##  4       2    48 Pretoria  ans_1    NA
##  5       2    48 Pretoria  ans_2     2
##  6       2    48 Pretoria  ans_3     2
##  7       3    45 Cape Town ans_1     3
##  8       3    45 Cape Town ans_2     3
##  9       3    45 Cape Town ans_3     2
## 10       4    40 Pretoria  ans_1     2
## 11       4    40 Pretoria  ans_2     1
## 12       4    40 Pretoria  ans_3     2
## 13       5    31 Cape Town ans_1     2
## 14       5    31 Cape Town ans_2     2
## 15       5    31 Cape Town ans_3     1
```


In the new table:

+ the three columns ans_1 to ans_3 have disappeared
+ two new columns name and value have been added

  + name corresponds to string identifying the former column name 
  + value corresponds to the value of the cell in the former column


Note that since the question have an easy pattern for names, we could obtain the same result with less effort:


```r
pivot_longer(dt, cols=contains("ans"))
```

```
## # A tibble: 15 x 5
##    resp_id   age city      name  value
##      <int> <int> <chr>     <chr> <int>
##  1       1    43 Cape Town ans_1     2
##  2       1    43 Cape Town ans_2     1
##  3       1    43 Cape Town ans_3     1
##  4       2    48 Pretoria  ans_1    NA
##  5       2    48 Pretoria  ans_2     2
##  6       2    48 Pretoria  ans_3     2
##  7       3    45 Cape Town ans_1     3
##  8       3    45 Cape Town ans_2     3
##  9       3    45 Cape Town ans_3     2
## 10       4    40 Pretoria  ans_1     2
## 11       4    40 Pretoria  ans_2     1
## 12       4    40 Pretoria  ans_3     2
## 13       5    31 Cape Town ans_1     2
## 14       5    31 Cape Town ans_2     2
## 15       5    31 Cape Town ans_3     1
```

The column name and value are not very explicit, so you can rename them with the options names_to, and values_to. 


```r
pivot_longer(dt, cols=contains("ans"), 
             names_to = "Question", values_to = "Answer")
```

```
## # A tibble: 15 x 5
##    resp_id   age city      Question Answer
##      <int> <int> <chr>     <chr>     <int>
##  1       1    43 Cape Town ans_1         2
##  2       1    43 Cape Town ans_2         1
##  3       1    43 Cape Town ans_3         1
##  4       2    48 Pretoria  ans_1        NA
##  5       2    48 Pretoria  ans_2         2
##  6       2    48 Pretoria  ans_3         2
##  7       3    45 Cape Town ans_1         3
##  8       3    45 Cape Town ans_2         3
##  9       3    45 Cape Town ans_3         2
## 10       4    40 Pretoria  ans_1         2
## 11       4    40 Pretoria  ans_2         1
## 12       4    40 Pretoria  ans_3         2
## 13       5    31 Cape Town ans_1         2
## 14       5    31 Cape Town ans_2         2
## 15       5    31 Cape Town ans_3         1
```

Finally, since the column names had the pattern 'ans_', it may appear strange to have ans_1, ans_2, etc.  as possible values for questions !  You can use the option names_prefix to eliminate it when it is pivoted


```r
pivot_longer(dt, cols=contains("ans"), 
             names_prefix = "ans_", names_to = "Question", values_to = "Answer")
```

```
## # A tibble: 15 x 5
##    resp_id   age city      Question Answer
##      <int> <int> <chr>     <chr>     <int>
##  1       1    43 Cape Town 1             2
##  2       1    43 Cape Town 2             1
##  3       1    43 Cape Town 3             1
##  4       2    48 Pretoria  1            NA
##  5       2    48 Pretoria  2             2
##  6       2    48 Pretoria  3             2
##  7       3    45 Cape Town 1             3
##  8       3    45 Cape Town 2             3
##  9       3    45 Cape Town 3             2
## 10       4    40 Pretoria  1             2
## 11       4    40 Pretoria  2             1
## 12       4    40 Pretoria  3             2
## 13       5    31 Cape Town 1             2
## 14       5    31 Cape Town 2             2
## 15       5    31 Cape Town 3             1
```

Full details about pivot_long can be found at: <a href="https://tidyr.tidyverse.org/reference/pivot_longer.html" target="_blank">pivot_longer</a>

