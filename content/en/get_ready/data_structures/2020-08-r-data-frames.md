---
title: "R Data Frames"
author: "Damien Jourdain"
date: '2020-08-20'
slug: r-data-frames
categories: R
tags: []
type: book
weight: 60
output:
  html_document:
    keep_md: true

---

## Learning objectives

`data.frame` objects allow to store data of *different basic types* into a single tabular structure. I will use the "Data frame" spelling.
 
Data frames are very handy to work with survey data since they are usually stored in tabular form, with many columns corresponding to the responses to the survey and many rows (usually, but not always, one row corresponding to one survey). 

In this section, you will learn:

+ [What is a data frame](#what-is-a-data-frame)
+ [How to create a data.frame object](#create-a-data-frame)
+ [How to access the data](#how-to-retrieve-the-data)
+ [How to sort the data](#how-to-sort-a-data-frame)
+ [How to quickly summarize the data](#quick-summary)
+ [Other classes of data frames](#other-classes-of-data-frames)
  + [Tibbles](#tibbles)
  + [data.table](#datatable)
  
## What is a data frame? 

A data frame is a *table* or a *two-dimensional array-like structure* in which each column contains values of one variable and each row contains one set of values from each column.

Following are the characteristics of a data frame.

+    The column names should be non-empty;
+    The row names should be unique;
+    The data stored in a data frame can be of numeric, factor or character type;
+    Each column should contain same number of data items.

> Data frames are particularly useful because we can combine different types into one single object and are easier to handle than lists.  

We can use built-in dataframes in R to understand this. For example, here is a built-in data frame in R, called `mtcars`. You can access it by just simplying typing its name.


```r
mtcars   
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

The top line of the table, called the *header*, contains the *column names*. Each horizontal line afterward denotes a *data row*, which begins with the name of the row, and then followed by the actual data. Each data member of a row is called a *cell*. 

You can easily interpret this as a data frame describing cars. Each row corresponds to a unique model of car, each column corresponds to the different characteristics of the cars.

If you want to know the exact structure of the object mtcars, you can use the function `str()`

```r
str(mtcars)
```

```
## 'data.frame':	32 obs. of  11 variables:
##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
##  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
##  $ disp: num  160 160 108 258 360 ...
##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
##  $ qsec: num  16.5 17 18.6 19.4 17 ...
##  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
##  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
##  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
##  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
```
We are learning that mtcars is an R object of type `data.frame`, that contains 32 observations (i.e. 32 rows) of 11 variables (i.e., 11 columns). Then each column is described (name, type, first values stored).

## Create a Data Frame

Typically, `data.frames` are imported from other programs rather than created from scratch (refer to the Import Data section for details). Nevertheless, it is important to be aware that you can generate a data.frame on your own.

You create a data frame by supplying *name-vector* pairs to the function `data.frame()`:


```r
df1 <- data.frame(
   emp_id = c (1:5), 
   emp_name = c("Rick","Dan","Michelle","Ryan","Gary"),
   salary = c(623.3, 515.2, 611.0, 729.0, 843.25), 
   start_date = as.Date(c("2012-01-01", "2013-09-23", "2021-11-15", "2014-05-11", "2015-03-27"))
)

str(df1)
```

```
## 'data.frame':	5 obs. of  4 variables:
##  $ emp_id    : int  1 2 3 4 5
##  $ emp_name  : chr  "Rick" "Dan" "Michelle" "Ryan" ...
##  $ salary    : num  623 515 611 729 843
##  $ start_date: Date, format: "2012-01-01" "2013-09-23" ...
```

## How to retrieve the data? 

### Data in a cell

To retrieve data in a cell, you will enter its row and column coordinates in the single square bracket `[ ]` operator. The two coordinates are separated by a comma. In other words, the coordinates begins with row position, then followed by a comma, and ends with the column position. **The order is important**.

Here is the cell value from the first row, second column of mtcars.


```r
mtcars[1, 2]
```

```
## [1] 6
```

Moreover, we can use the row and column names instead of the numeric coordinates.

```r
mtcars["Mazda RX4", "cyl"] 
```

```
## [1] 6
```
{{< spoiler text="A note on row and column names" >}}
{{% callout note %}}
If you do not remember the names of the columns you can use either `names()` or `colnames()`:


```r
names(mtcars)
```

```
##  [1] "mpg"  "cyl"  "disp" "hp"   "drat" "wt"   "qsec" "vs"   "am"   "gear"
## [11] "carb"
```

```r
# colnames(mtcars)  #colnames give the same results
```

If you do not remember the names of the rows you can use `rownames()`:

```r
rownames(mtcars)
```

```
##  [1] "Mazda RX4"           "Mazda RX4 Wag"       "Datsun 710"         
##  [4] "Hornet 4 Drive"      "Hornet Sportabout"   "Valiant"            
##  [7] "Duster 360"          "Merc 240D"           "Merc 230"           
## [10] "Merc 280"            "Merc 280C"           "Merc 450SE"         
## [13] "Merc 450SL"          "Merc 450SLC"         "Cadillac Fleetwood" 
## [16] "Lincoln Continental" "Chrysler Imperial"   "Fiat 128"           
## [19] "Honda Civic"         "Toyota Corolla"      "Toyota Corona"      
## [22] "Dodge Challenger"    "AMC Javelin"         "Camaro Z28"         
## [25] "Pontiac Firebird"    "Fiat X1-9"           "Porsche 914-2"      
## [28] "Lotus Europa"        "Ford Pantera L"      "Ferrari Dino"       
## [31] "Maserati Bora"       "Volvo 142E"
```
{{% /callout %}}
{{< /spoiler>}}

### Data contained in a column

There are several ways to retrieve the data contained into the columns.

First, we can retrieve a single column with the "$" operator:

```r
mtcars$hp 
```

```
##  [1] 110 110  93 110 175 105 245  62  95 123 123 180 180 180 205 215 230  66  52
## [20]  65  97 150 150 245 175  66  91 113 264 175 335 109
```

```r
class(mtcars$hp)
```

```
## [1] "numeric"
```

Note that you need to know the name of the column you want to retrieve. However, if using R Studio, if you once you have typed mtcars$ a list of possible names will be suggested to you. (yet another good reason to use R Studio instead of R)

Second, we can retrieve the same column with the single square bracket "[]" operator. To do this, we have to prepend the column name (or column number) with a comma character, which signals that we want to take consider all the rows:


```r
mtcars[,"hp"] 
```

```
##  [1] 110 110  93 110 175 105 245  62  95 123 123 180 180 180 205 215 230  66  52
## [20]  65  97 150 150 245 175  66  91 113 264 175 335 109
```

Alternatively, we can use the column number. 


```r
mtcars[,4] 
```

```
##  [1] 110 110  93 110 175 105 245  62  95 123 123 180 180 180 205 215 230  66  52
## [20]  65  97 150 150 245 175  66  91 113 264 175 335 109
```

However, it is often clearer to use the column name than the column number. Besides, if you change the structure of the data frame, the ordering of the columns may change, and you may unknowingly refer to a different number if you use column numbers.

{{% callout note %}}
In both cases: 

  + the object `mtcars$hp` or `mtcars[,4] ` is a vector containing 32 numbers (i.e., the number of rows).
+ the order of the entries in the `mtcars$hp` vector preserves the order of the rows in our data frame. This is important as this allows us to manipulate one variable based on the results of another.
{{% /callout %}}


### Data contained in several columns

This use of brackets is becoming especially useful when you want to retrieve several columns. Try this command:


```r
carsub <- mtcars[,2:4]
head(carsub)  # the function head() displays the first six rows of the table
```

```
##                   cyl disp  hp
## Mazda RX4           6  160 110
## Mazda RX4 Wag       6  160 110
## Datsun 710          4  108  93
## Hornet 4 Drive      6  258 110
## Hornet Sportabout   8  360 175
## Valiant             6  225 105
```

This example shows that you can extract the columns 2, 3 and 4 of the table `mtcars` and store them in a new data.frame called `carsub`. 

If you want to show several columns that are not in the same sequence of numbers, you can use the function `c()`. 

For example, if you want to select the columns 3,7 and 11:


```r
carsub <- mtcars[,c(3,7,11)]
head(carsub)
```

```
##                   disp  qsec carb
## Mazda RX4          160 16.46    4
## Mazda RX4 Wag      160 17.02    4
## Datsun 710         108 18.61    1
## Hornet 4 Drive     258 19.44    1
## Hornet Sportabout  360 17.02    2
## Valiant            225 20.22    1
```

Again, you can use a vector of column names instead of column numbers:


```r
carsub <- mtcars[ ,c("disp", "qsec","carb")]
```


### Data contained in the rows

We retrieve rows from a data frame with the single square bracket operator, just like what we did with columns. However, in additional to an index vector of row positions, we append an extra comma character. This is important, as the extra comma signals a wildcard match for the second coordinate for column positions. 

To extract rows from a data frame, we use the single square bracket operator, just as we did with columns. However, instead of just providing a vector of row positions, we add a comma after it. 


```r
mtcars[1:2, ] 
```

```
##               mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4      21   6  160 110  3.9 2.620 16.46  0  1    4    4
## Mazda RX4 Wag  21   6  160 110  3.9 2.875 17.02  0  1    4    4
```
This comma is crucial because it indicates a wildcard match for the second coordinate for column positions. In other words, the comma tells R to include all columns when sub-setting the data frame. Without the comma, R would interpret the index vector as selecting columns, which is not what you wanted.
To convince yourself, evaluate the following command:

```r
mtcars[1:2] 
```

You can also select rows that are not part of a sequence:


```r
mtcars[c(1,3,10), ] 
```

```
##             mpg cyl  disp  hp drat   wt  qsec vs am gear carb
## Mazda RX4  21.0   6 160.0 110 3.90 2.62 16.46  0  1    4    4
## Datsun 710 22.8   4 108.0  93 3.85 2.32 18.61  1  1    4    1
## Merc 280   19.2   6 167.6 123 3.92 3.44 18.30  1  0    4    4
```

Once you understand this syntax and what was said in the previous section about selection of columns, you should be able to select different row selections using the `c()` function and either row number or row names.

### Sub-set of rows and columns


```r
mtcars[c(1,3,10), c(2, 4:5) ] 
```

```
##            cyl  hp drat
## Mazda RX4    6 110 3.90
## Datsun 710   4  93 3.85
## Merc 280     6 123 3.92
```

### Subset based on logical criteria

When looking to create subsets based on a logical condition, you can use the `subset()` function. 


```r
subset(mtcars, hp < 90)
```

```
##                 mpg cyl  disp hp drat    wt  qsec vs am gear carb
## Merc 240D      24.4   4 146.7 62 3.69 3.190 20.00  1  0    4    2
## Fiat 128       32.4   4  78.7 66 4.08 2.200 19.47  1  1    4    1
## Honda Civic    30.4   4  75.7 52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla 33.9   4  71.1 65 4.22 1.835 19.90  1  1    4    1
## Fiat X1-9      27.3   4  79.0 66 4.08 1.935 18.90  1  1    4    1
```

Note that: 

+ the first argument you pass to `subset()` is the name of the dataframe you want to subset
+ you shouldn't use quotes around hp
+ you should use the traditional comparators to contrusct your conditions: `==`, `>`, etc. 

## How to sort a data.frame?

To sort a data.frame, use the `order( )` function. By default, sorting is ASCENDING. Prepend the sorting variable by a minus sign to indicate DESCENDING order. 


```r
#order the cars using the column mpg (ascending order)
mtcars[order(mtcars$mpg),]
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
```


```r
#sort the cars using two keys
mtcars[order(mtcars$mpg, -mtcars$cyl),]
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
```

{{% callout note %}}

Note that you need to repeat the mtcars$ part within the order function; not doing so will produce an error, because the order function will expect a variable that does not exist.


```r
mtcars[order(mpg),]
```

```
## Error in order(mpg): object 'mpg' not found
```

{{% /callout %}}

## How to quickly summarize the data {#quick-summary}

The statistical summary and nature of the data can be obtained by applying summary() function.


```r
summary(carsub)
```

```
##       disp            qsec            carb      
##  Min.   : 71.1   Min.   :14.50   Min.   :1.000  
##  1st Qu.:120.8   1st Qu.:16.89   1st Qu.:2.000  
##  Median :196.3   Median :17.71   Median :2.000  
##  Mean   :230.7   Mean   :17.85   Mean   :2.812  
##  3rd Qu.:326.0   3rd Qu.:18.90   3rd Qu.:4.000  
##  Max.   :472.0   Max.   :22.90   Max.   :8.000
```



## Other classes of data frames

### Tibbles

Tibble is a new class of data frames (developed by RStudio). They are also capturing data in tabular forms, and have internal features that make working with some packages a little easier. 

You can work with tibbles by installing the suite of packages `tidyverse` or the specific package `tibble`.


```r
# install a suite of related packages
install.packages("tidyverse")

# Alternatively, install just tibble:
install.packages("tibble")
```

Then load the package tibble

```r
library(tibble)
```


#### Creation

Like data.frame, you can create a new tibble from individual vectors with tibble(). 

However, there are a few differences: 

tibble() will: 

+ keep strings as characters (and not convert them automatically into factors)
+ allows you to refer to variables that you just created
+ automatically recycle inputs of length 1


```r
tibble(
  x = 1:5, 
  y = 1,    # this will be automatically transformed into a vector with length 5
  z = x^2 + y,  # it will recognize x and y that were just created
  t = letters[1:5]   # strings will not be converted into factors
)
```

```
## # A tibble: 5 × 4
##       x     y     z t    
##   <int> <dbl> <dbl> <chr>
## 1     1     1     2 a    
## 2     2     1     5 b    
## 3     3     1    10 c    
## 4     4     1    17 d    
## 5     5     1    26 e
```

It also checks that columns have the same lengths (except, as seen above for unique values)


```r
tibble(
  x = 1:5, 
  t = letters[1:3]
)
```

```
## Error in `tibble()`:
## ! Tibble columns must have compatible sizes.
## • Size 5: Existing data.
## • Size 3: Column `t`.
## ℹ Only values of size one are recycled.
```

#### Display 

With large data frames, it will show only the first 10 rows, and all the columns that fit on screen and in addition to its name, each column reports its type.


```r
ans <- tibble(
  x = 1:50, 
  t = letters[1:50],
  x2 = x^2,
  e = sample(letters, 50, replace = TRUE)
)
ans
```

```
## # A tibble: 50 × 4
##        x t        x2 e    
##    <int> <chr> <dbl> <chr>
##  1     1 a         1 v    
##  2     2 b         4 l    
##  3     3 c         9 i    
##  4     4 d        16 x    
##  5     5 e        25 u    
##  6     6 f        36 m    
##  7     7 g        49 d    
##  8     8 h        64 b    
##  9     9 i        81 f    
## 10    10 j       100 w    
## # … with 40 more rows
```


#### Subsetting

Compared to a data.frame, tibbles are stricter; in particular they never do *partial matching*, and they will generate a warning if the column you are trying to access does not exist.

Compare the two codes:


```r
df <- data.frame(
  x1 = runif(5),
  y1 = rnorm(5),
  y2 = rnorm(5)
)
df$x # Extract by name
```

```
## [1] 0.4185595 0.7090068 0.8417981 0.4979531 0.7715919
```

```r
df$y
```

```
## NULL
```

```r
df[, 1] # Extract by position
```

```
## [1] 0.4185595 0.7090068 0.8417981 0.4979531 0.7715919
```


```r
df <- tibble(
  x1 = runif(5),
  y1 = rnorm(5),
  y2 = rnorm(5)
)
df$x # Extract by name
```

```
## Warning: Unknown or uninitialised column: `x`.
```

```
## NULL
```

```r
df$x1
```

```
## [1] 0.82821475 0.02363167 0.99581781 0.88277135 0.64373937
```

```r
df$y
```

```
## Warning: Unknown or uninitialised column: `y`.
```

```
## NULL
```

```r
df[, 1 ] # Extract by position
```

```
## # A tibble: 5 × 1
##       x1
##    <dbl>
## 1 0.828 
## 2 0.0236
## 3 0.996 
## 4 0.883 
## 5 0.644
```


### data.table

data.table is a package that extends data.frames. Two of its most notable features are speed and cleaner syntax.
I will give much more details about data.table in a separate section.


## Exercises

### Exercise 1: 

1. Create the following vectors corresponding to Name, Age, Height, Weigth and Sex. Make sure Sex is a factor.

```
## [1] 25 31 23 52 76 49 26
```

```
## [1] 177 163 190 179 163 183 164
```

```
## [1] 57 69 83 75 70 83 53
```

```
## [1] F F M M F M F
## Levels: F M
```

2. Using these vectors, create the following data frame. (Be careful the vector Name must be used for the creation of row names)


```
##         Age Height Weight Sex
## Alex     25    177     57   F
## Moses    31    163     69   F
## Stephan  23    190     83   M
## Zakhele  52    179     75   M
## Leane    76    163     70   F
## Lucas    49    183     83   M
## Nobhule  26    164     53   F
```

{{< spoiler text="Show the answer" >}}

Question 1:


```r
Name <- c("Alex", "Moses", "Stephan", "Zakhele", "Leane", "Lucas", "Nobhule")
Age <- c(25, 31, 23, 52, 76, 49, 26)
Height <- c(177, 163, 190, 179, 163, 183, 164)
Weight <- c(57, 69, 83, 75, 70, 83, 53)
Sex <- as.factor(c("F", "F", "M", "M", "F", "M", "F"))
Age
```

```
## [1] 25 31 23 52 76 49 26
```

```r
Height
```

```
## [1] 177 163 190 179 163 183 164
```

```r
Weight
```

```
## [1] 57 69 83 75 70 83 53
```

```r
Sex
```

```
## [1] F F M M F M F
## Levels: F M
```

Question 2:


```r
df <- data.frame (row.names = Name, Age, Height, Weight, Sex)
df
```

```
##         Age Height Weight Sex
## Alex     25    177     57   F
## Moses    31    163     69   F
## Stephan  23    190     83   M
## Zakhele  52    179     75   M
## Leane    76    163     70   F
## Lucas    49    183     83   M
## Nobhule  26    164     53   F
```

{{< /spoiler >}}


### Exercise 2

Using the dataframe `mtcars`, create a new data frame that: 

+ only includes cars with a horse power (hp) greater or equal to 120 and lower or equal to 200, 
+ only contains the columns `disp`, `drat` and `hp`,
+ is sorted by decreasing `hp`

{{< spoiler text="Show the answer" >}}

```r
sub <- subset(mtcars[, c("disp", "drat", "hp")], hp >= 120 & hp <=200)
sub2 <- sub[order(-sub$hp),]
sub2
```

```
##                    disp drat  hp
## Merc 450SE        275.8 3.07 180
## Merc 450SL        275.8 3.07 180
## Merc 450SLC       275.8 3.07 180
## Hornet Sportabout 360.0 3.15 175
## Pontiac Firebird  400.0 3.08 175
## Ferrari Dino      145.0 3.62 175
## Dodge Challenger  318.0 2.76 150
## AMC Javelin       304.0 3.15 150
## Merc 280          167.6 3.92 123
## Merc 280C         167.6 3.92 123
```
{{< /spoiler >}}

