---
linktitle: Manipulez vos données avec tidyverse
summary: Manipulez vos données avec tidyverse
weight: 1
icon: book
icon_pack: fas

# Page metadata.
title: "Manipulez vos données avec tidyverse"
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

```{r, echo=TRUE, warning=TRUE, message=TRUE}
library(tidyverse)
```
```
-- Attaching packages -------------------------------------------------------------------------------- tidyverse 1.3.1 --
v ggplot2 3.3.5     v purrr   0.3.4
v tibble  3.1.5     v dplyr   1.0.7
v tidyr   1.1.4     v stringr 1.4.0
v readr   2.0.2     v forcats 0.5.1
-- Conflicts ----------------------------------------------------------------------------------- tidyverse_conflicts() --
x dplyr::filter() masks stats::filter()
x dplyr::lag()    masks stats::lag()
Warning messages:
1: package ‘tidyverse’ was built under R version 4.0.5 
2: package ‘ggplot2’ was built under R version 4.0.5 
3: package ‘tibble’ was built under R version 4.0.5 
4: package ‘tidyr’ was built under R version 4.0.5 
5: package ‘readr’ was built under R version 4.0.5 
6: package ‘dplyr’ was built under R version 4.0.5 
```

The message indicates the packages that you have just loaded, the potential conflicts. Note again, that red color does not necessarily means it did not work.



*You are now ready to work!*







