---
# Title, summary, and page position.
linktitle: Manipulate your data with tidyverse
summary: Manipulate your data with tidyverse
weight: 3
icon: book
icon_pack: fas

# Page metadata.
title: "Chapter 3: Manipulate your data"
date: "2020-08-20T00:00:00Z"
type: book  
---

In most real-life cases, data is not always ready to use immediately. Most of the time it has missing values, missing variable names, or variables scattered into multiple columns that you will need to synthesize. When this kind of data is presented for analysis, it needs intervention to make it *tidy*. 

Up to now, we have been manipulating vectors, matrix and data.frames by reordering them and subsetting them through indexing and the operator `[ ]`. But once we start more advanced analyses, we will want to manipulate with more efficient tools.
{{< figure library="true" src="OIP.jpg" >}}  

To manipulate data, you have different options. Among them, 3 environments are popular:

+ The original Data.frame
+ The data.table package
+ The tidyverse ecosystem

In this section we describe the basic function of the `tidyverse` ecosystems that I find most useful.


## The data set

In this chapter, we will work with the datasets made available by the package `gapminder`.
You should now be able to install the package on your own (if not, please see early sections on how to install packages on your computer)

Load the `gapminder` package:
```r
library(gapminder)
```

If everyting went ok you should be able to access the gapminder data directly

```r
gapminder
```

```
## # A tibble: 1,704 x 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
## # ... with 1,694 more rows
```

## Load tidyverse

Let's now load the tidyverse ecosystems, we call it "ecosystems" because it is composed of several packages. However, you can install these different packages using one command

```{r, echo=TRUE}
library(tidyverse)
```
The message indicates the packages that you have just loaded.



*You are now ready to work!*







