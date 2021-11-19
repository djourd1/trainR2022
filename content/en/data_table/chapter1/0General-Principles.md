---
title: data.table principles
author: Damien Jourdain
date: '2020-08-20'
slug: dt-principles
categories:
  - R
tags: []
type: book
weight: 10
output:
  html_document:
    keep_md: true
---

In the data.frame context, you can select subsetting rows and columns by identifying them. For example, you can select the rows ranging from 1 to 10 and the first three columns using the following commands:

`data[1:10, 1:3]`

With a data.table you can do a lot more when using the brackets[...]. The general form of data.table syntax is of the form:

`DT[i, j, by]`

The way to read it (out loud) is: 

+ Take `DT`
+ subset/reorder rows using `i`
+ then calculate `j`, (by summarizing results by `by` 

There is a large set of possibilities to act upon your data set that include:

+ subsettting the data you want to see or manage
+ summarizing the data available
+ transform the data (create new columns)  

In addition, the package allow to perform some additional manipulations such as

+ merging two different tables
+ reshaping the tables 

We can learn progressively these functions using the `data.table` help and vignettes. 
In this course I will present the functions that you will be using to manipulate our data.



