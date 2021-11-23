---
title: "From long to wide" 
author: Damien Jourdain
date: '2020-08-20'
slug: longtowide
type: book
weight: 3
---



## The original data

Let's suppose that you capture or obtained data in the following format:

+ one line per answer, per respondent
+ one column indicating the actual question that was answered
+ additional columns to describe the respondents



```
## # A tibble: 21 x 5
##    resp_id   age city      Question Answer
##      <int> <int> <chr>     <chr>     <int>
##  1       1    43 Pretoria  1             2
##  2       1    43 Pretoria  2             1
##  3       1    43 Pretoria  3             2
##  4       2    48 Cape Town 1             2
##  5       2    48 Cape Town 2             2
##  6       2    48 Cape Town 3             1
##  7       3    45 Cape Town 1             1
##  8       3    45 Cape Town 2             3
##  9       3    45 Cape Town 3             2
## 10       4    40 Pretoria  1             2
## # ... with 11 more rows
```

## What do you want to obtain?

If you want to reshape your table as a wide format, you will shape in order to obain

+ one line per respondent with three columns corresponding to the answers to the three questions
+ additional columns detailing characteristics of the respondent



## Use pivot_wider()

To do so, we will use the command `pivot_wider()`. It is part of the `tidyr` package, a member of the tidyverse family, so do not forget to load the tidyverse package:


```r
library(tidyverse)
```


```r
dt %>%
  pivot_wider(names_from = "Question", values_from = "Answer")
```

```
## # A tibble: 7 x 6
##   resp_id   age city        `1`   `2`   `3`
##     <int> <int> <chr>     <int> <int> <int>
## 1       1    43 Pretoria      2     1     2
## 2       2    48 Cape Town     2     2     1
## 3       3    45 Cape Town     1     3     2
## 4       4    40 Pretoria      2     2     2
## 5       5    31 Cape Town     3     2     1
## 6       6    40 Cape Town     1     3     2
## 7       7    35 Pretoria      2     1     1
```

It has create three new columns which names were the unique values of the column Question. Each cell in these columns correspond to the answers of respondents to the three questions. 


```r
dt %>%
  pivot_wider(names_from = "Question", names_prefix = "quest_", values_from = "Answer")
```

```
## # A tibble: 7 x 6
##   resp_id   age city      quest_1 quest_2 quest_3
##     <int> <int> <chr>       <int>   <int>   <int>
## 1       1    43 Pretoria        2       1       2
## 2       2    48 Cape Town       2       2       1
## 3       3    45 Cape Town       1       3       2
## 4       4    40 Pretoria        2       2       2
## 5       5    31 Cape Town       3       2       1
## 6       6    40 Cape Town       1       3       2
## 7       7    35 Pretoria        2       1       1
```

Full details about pivot_long can be found at: <a href="https://tidyr.tidyverse.org/reference/pivot_wider.html" target="_blank">pivot_wider</a>
