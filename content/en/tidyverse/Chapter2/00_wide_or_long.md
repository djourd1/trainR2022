---
title: "Wide or Long" 
author: Damien Jourdain
date: '2020-08-20'
slug: wide-long-formats
type: book
weight: 1
output:
  html_document:
    keep_md: true
---


## When do we encounter wide or long formats ? 

{{<figure src="whatLongWide.svg">}}

Wide and long formats are generic terms to express how data are stored when you have repeated measurement for one observed unit. You will encounter wide or long formats when you will work with panel data such as time series and results from choice experiments. 

## Wide format
With a wide format, a subject's repeated responses will be in a single row, and each response is in a separate column.

In the following table, the respondents responded three times to a question. The answers are stored in the three columns (from ans_1 to ans_3). 

All the other columns correspond to unique characteristics (or answers) of the respondents. They are not repeated.


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

## Long format

With a long format, each row correspond to one response. So each respondent will have data in multiple rows. 

```
## # A tibble: 15 x 5
##    resp_id   age city      Question Answer
##      <int> <int> <chr>        <int>  <int>
##  1       1    43 Cape Town        1      2
##  2       1    43 Cape Town        2      1
##  3       1    43 Cape Town        3      1
##  4       2    48 Pretoria         1     NA
##  5       2    48 Pretoria         2      2
##  6       2    48 Pretoria         3      2
##  7       3    45 Cape Town        1      3
##  8       3    45 Cape Town        2      3
##  9       3    45 Cape Town        3      2
## 10       4    40 Pretoria         1      2
## 11       4    40 Pretoria         2      1
## 12       4    40 Pretoria         3      2
## 13       5    31 Cape Town        1      2
## 14       5    31 Cape Town        2      2
## 15       5    31 Cape Town        3      1
```

As you can see, each respondents are described in three rows, one per question. The question is described by a question id, and an answer. 

Note also variables that don’t change across responses will have the same value in all the rows of the corresponding respondent. This leads to some repetitions, but may be useful when conducting data analysis. 

Depending on data analysis needs, you will have to change from one format to another. Therefore, it is important that you master the functions to reshape data in one format or another. 

## A graphical representation

{{< figure src="wideLong.svg" >}}
