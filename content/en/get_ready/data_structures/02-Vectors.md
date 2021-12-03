---
title: Vectors
author: Damien Jourdain
date: '2019-02-27'
slug: vectors
categories:
  - R
tags: []
type: book
weight: 20 
output:
  html_document:
    keep_md: true
---

## Why do we need vectors?

In the previous section, we dealt with different objects and variables. Each variable had only one value. So we defined one by one. Now let's consider situation where you need to enter several values for a concept. 

Think about the expenses of all the member of your household during a month, or your monthly expenses in the last twelve month. In the former case, you could create and give values to twelve variables:


```r
exp_1 <- 100
exp_2 <- 300
exp_3 <- 1000
...
exp_12 <- 500
```

Let say, you have recorded these values in one currency, and now you want to convert them into another currency. To simplify, think the exchange rate was constant during the last twelve months. To do the conversion you will need to create twelve new variables:


```r
exch_rate = 1.5
exp_1_c2 <- exp_1 / exch_rate
exp_2_c2 <- exp_2 / exch_rate
exp_3_c2 <- exp_3 / exch_rate
...
exp_12_c2 <- exp_12 / exch_rate
```

Let say, now that instead of converting, you want to calculate your expenses during the all year. Then you need to calculate the sum of the different variables:


```r
total_expenses = exp_1 + exp_2 + exp_3 + ... + exp(12)
```

In both cases, you see that you need to make some repeative work: create many variables, or add-up many variables. This is both cunbersome, and prone to many typing errors.

Vectors simplify these types of operations, by collecting objects of similar types under one composite object.

In our case, we can define a new variable monthly_expenses that will include 12 values (one for each month), and operations will be done on this unique variable.


```r
# we will declare a vector (more details to come)
monthly_expenses <- c(400, 200, 1000, 200, 550, 220, 300, 400, 888, 875, 1000, 2000) 

# convert into another currency
exch_rate = 1.5
monthly_expenses_c2 <- monthly_expenses / exch_rate

# apply a summury function to the vector
annual_expenses <- sum(monthly_expenses)
```

You can see that vectors will greatly simplify your writing and reduce the number of errors you will make.
Now you are convinced vectors are useful, let's study them in more details.

## Learning objectives

The vectors are an essential structure of R language and functionning. In this section you will learn about vectors, how to create different types of vector, and how to manipulate them.

+ [Numeric Vectors: Definition and Creation ](#numeric-vectors)
+ [Generating regular sequences ](#generating-regular-sequences)
  + [The : operator](#the-colon-operator)
  + [The seq() function](#the-seq-function)
  + [The rep() function](#the-rep-function)
+ [Generating a vector of random number](#generating-random)
+ [Vector Arithmetic](#vector-arithmetic)
+ [Useful functions applied to a vector](#useful-functions-applied-to-a-vector)
+ [Vectors with other types of data](#vectors-with-other-types-of-data)
+ [Automatic coercing within vectors](#automatic-coercing-within-vectors)


## Numeric vectors

A numeric vector is  an *ordered collection of numbers*. To set up a vector, we use the R function `c()` which can take an arbitrary number of arguments:


```r
x <- c(10.4, 5.6, 3.1, 6.4, 21.7)
x
```

```
## [1] 10.4  5.6  3.1  6.4 21.7
```

When you search the class of a vector, you can again use the function `class()`:

```r
class(x)
```

```
## [1] "numeric"
```

Note that the class is still "numeric", however, you are dealing now with a collection of numbers.


You can also use the function `c()` to concatenate several existing vectors:


```r
x <- c(10.4, 5.6, 3.1 )
y <- c(2,3,4)
z <- c(x,y)
z
```

```
## [1] 10.4  5.6  3.1  2.0  3.0  4.0
```

```r
class(z)
```

```
## [1] "numeric"
```


## Generating regular sequences

R has also a number of built-in functions for generating commonly used sequences of numbers. 

### The colon operator `:`

The `:`  creates regular sequence of integers. Its syntax is quite intuitive: to create a regular sequence of integers from 1 to 10, just type:

```r
x<- 1:10
x
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
class(x)
```

```
## [1] "integer"
```

The colon operator has high priority within an expression. Check the results of the following expressions to understand the order in which the operators are working: 


```r
2*1:5
```

```
## [1]  2  4  6  8 10
```

```r
n <- 6
1:n-1
```

```
## [1] 0 1 2 3 4 5
```

```r
1:(n-1)
```

```
## [1] 1 2 3 4 5
```

### The `seq()` function  
The function seq() is more general. 
It has five arguments, only some of which may be specified in any one call. Consult the help file for more details. Look at the few examples to understand the function seq() 


```r
seq(1, 2, by=.2) 
```

```
## [1] 1.0 1.2 1.4 1.6 1.8 2.0
```

```r
seq(length=10, from =-1, by=2)
```

```
##  [1] -1  1  3  5  7  9 11 13 15 17
```

### The `rep()` function  

The function rep() can be used for replicating an object in several ways and creating vectors.

To put, several copies end-to-end, use the argument `times`:

```r
x <- c(1.34, 2.56)
(s5 <- rep(x, times=5))
```

```
##  [1] 1.34 2.56 1.34 2.56 1.34 2.56 1.34 2.56 1.34 2.56
```

To repeat each element of three times before moving on to the next, use the argument `each`:

```r
(s6 <- rep(x, each=3))
```

```
## [1] 1.34 1.34 1.34 2.56 2.56 2.56
```

## Generating a vector of random numbers {#generating-random}

### from the Normal distribution

To generate numbers from a Normal Distribution, you use the `rnorm()` function. This function has 3 arguments, namely the sample size, and the mean and standard deviation of the normal distribution.

The mean and standard deviation are optional arguments. If you do not include them, you will create a vector of random numbers from the Standard Normal Distribution, i.e. where mean = 0 and sd =1.

To create a sample of 10 observations from the Standard Normal Distribution. That is to say, a normal distribution with a mean of 0 and a standard deviation of 1.


```r
(tennorm <-rnorm(10))
```

```
##  [1]  0.09261951  0.63977460 -1.77004474  1.20425305  0.20217454  1.09582156
##  [7]  2.26276934 -1.26445180 -1.21526793 -0.57470793
```

To create a sample of 15 observations from the Normal Distribution with mean  20 and standard deviation 2. 


```r
(fifteennorm <-rnorm(n=15, mean=20, sd=2))
```

```
##  [1] 23.42073 19.80675 22.75684 19.10192 20.73464 16.68803 19.73379 18.93500
##  [9] 21.44864 18.98346 20.40214 20.59522 20.13611 21.54356 18.26494
```

### from a uniform distribution

You create a vector with randomly generated numbers falling between two values with the `runif()` function. This R function has three arguments:

+ A positive integer that specifies the sample size.
+ The minimum of the sample population. For example, if you want to generate a sample with numbers between 0 and 1, you should set this argument to 0.
+ The maximum of the sample population. For example, if you want to genereate a sample with numbers between 0 and 1, you should set this argument to 1.

To create a sample of 10 observations with numbers between 0 and 1. 


```r
(tenunif <- runif(10))
```

```
##  [1] 0.53198543 0.98721679 0.18808858 0.70985926 0.73208100 0.07419112
##  [7] 0.23576100 0.39783108 0.96724771 0.12302788
```

To create a sample of 10 observations with numbers between 10 and 20. 


```r
(tenunif2 <- runif(n=10, min=10, max=20))
```

```
##  [1] 16.37070 12.64834 19.20492 17.66582 18.57329 11.68217 15.38139 10.75216
##  [9] 15.57836 17.26091
```

### with random integers

You create a vector with random integers with the `sample()` function. This function has two obligatory arguments, namely the integers from which you want to sample, and the number of samples you want to take. A third, optional argument, tells R to sample with or without replacement.

The first argument, the population from which R takes a sample, can have different forms. For examples:

+ A continuous list of integers, e.g., 1:200
+ A discontinuous list of integers, e.g. c(1, 3, 5, 15, 30)
+ A combination of the two list above, e.g., c(1:100, -16, -2)

The second argument, the sample size, must be a positive whole number.

To create a sample of 20 randomly generated integers between -20 and 20 without replacement:

```r
set.seed(1234)
sample20 <- sample(-20:20, 20, replace = FALSE)
sample20
```

```
##  [1]   7  -5   1  16 -12 -16  19 -17  18   5 -15  -6  -7  -1   8   3  13   4   0
## [20] -13
```

{{% callout note %}} 
{{< spoiler text="A note on the use of seeds" >}}
You probably noticed that the random samples you obtained on your computer are different from the example displayed on this web site, and are different each time you run the commands. This is what randomness is about. However, sometimes you want to make sure you can retrieve the exact same random sample to make the output of your R code reproducible. 

The random numbers that your computer generates are in fact *pseudorandom* numbers that aim to simulate randomness.A seed is a number that initializes a pseudorandom number generator.  By setting a specific **seed**, the random processes in our script always start at the same point and hence lead to the same result.

First, let's generate some random numbers using the `sample()` function:

```r
sample(1:200, 5)  # sample out 5 numbers from the vector 1:200
```

```
## [1] 184 195  93 122 133
```

Let's execute exactly the same R code again:


```r
sample(1:200, 5)  # sample out 5 numbers from the vector 1:200
```

```
## [1]  66 175 168 123  48
```
The results should be different.

Second, let's set a seed before the command and see what happens:

```r
set.seed(12345)  # set the seed for pseudorandom generator
sample(1:200, 5)  # sample out 5 numbers from the vector 1:200
```

```
## [1] 142  51 152  58  93
```

Let’s do this again with the same seed as before:

```r
set.seed(12345) # set the seed for pseudorandom generator
sample(1:200, 5)  # sample out 5 numbers from the vector 1:200
```

```
## [1] 142  51 152  58  93
```

The output is exactly the same for the two experiences, and they should also be the same than the one on your computer.
{{< /spoiler>}}
{{% /callout %}}



## Vector arithmetic

Vectors can be used in arithmetic expressions, in which case the operations are performed **element by element**. 
For example, if you add two vectors:

\\begin{equation}
\\left(\\begin{matrix}a\\\b\\\c\\\d \\end{matrix} \\right)  + 
\\left(\\begin{matrix}e\\\f\\\g\\\h  \\end{matrix} \\right) =
\\left(\\begin{matrix}a+e\\\b+f\\\c+g\\\d+h \\end{matrix} \\right) 
\\end{equation}

In R:


```r
x <- c(10.4, 5.6, 3.4)
y <- c(3.1, 6.4, -2.1)
(z <- x + y)
```

```
## [1] 13.5 12.0  1.3
```

However, vectors occurring in the same expression do not need to be of the same length. If they are not, the value of the expression is a vector with the same length as the longest vector which occurs in the expression. Shorter vectors in the expression are recycled as often as need be (perhaps fractionally) until they match the length of the longest vector. 
In particular a constant is simply repeated. 

For example: 
\\begin{equation}
\\left(\\begin{matrix}a\\\b\\\c \\end{matrix} \\right)  +  e =
\\left(\\begin{matrix}a+e\\\b+e\\\c+e \\end{matrix} \\right) 
\\end{equation}


```r
x <- c(10.4, 5.6)
y <- 3.1
(z <- x + y)
```

```
## [1] 13.5  8.7
```

{{% callout warning %}}

While this is a very convenient R feature, it can be potentially misleading as it brings unruly results when the vectors are of different length, as in the following case:

\\begin{equation}
\\left(\\begin{matrix}a\\\b\\\c\\\d \\end{matrix} \\right)  +  
\\left(\\begin{matrix}e\\\f\\\g \\end{matrix} \\right) =
\\left(\\begin{matrix}a+e\\\b+f\\\c+g\\\d+e \\end{matrix} \\right) 
\\end{equation}

{{% /callout %}}




```r
x <- c(10.4, 5.6, 3.5)
y <- c(1,2)
z <- x + y
```

```
## Warning in x + y: longer object length is not a multiple of shorter object
## length
```

```r
z
```

```
## [1] 11.4  7.6  4.5
```

Just to remind you of the potential danger, R will throw a warning message.

The elementary arithmetic operators are the usual +, -, *, / and ^ for raising to a power. In addition all of the common arithmetic functions are available. `log`, `exp`, `sin`, `cos`, `tan`, `sqrt` and so on, all have their usual meaning. 


```r
x <- c(10.4, 5.6, 3.5)
log(x)
```

```
## [1] 2.341806 1.722767 1.252763
```

`max` and `min` select the largest and smallest elements of a vector respectively.

```r
min(x)
```

```
## [1] 3.5
```


## Useful functions applied to a vector

Several functions are very useful for basic statistics. 

The `sum()` function, sums up all the elements of the vector. The `mean()` function calculates the mean value of all the elements of the vector. We can combine vector arithmetics with these functions to calculate very quickly some interesting statistics of the vector.


```r
x <- c(10.4, 5.6, 3.5, 3.1, 7.2, 3.8, 10.2, 7.8)
mean(x)
```

```
## [1] 6.45
```

```r
x - mean(x)  #calculate the deviation from the mean
```

```
## [1]  3.95 -0.85 -2.95 -3.35  0.75 -2.65  3.75  1.35
```

We can calculate easily the sample variance 
$$ var(x) = \frac{1}{n-1} \sum_{i=1}^n (x_i - \bar{x})^2 $$


```r
x <- c(10.4, 5.6, 3.5, 3.1, 7.2, 3.8, 10.2, 7.8)
mean(x)
```

```
## [1] 6.45
```

```r
v<- sum((x - mean(x))^2) / (length(x)-1)  #calculate the variance
v
```

```
## [1] 8.531429
```

```r
# note that a variance function already exists!
var(x)
```

```
## [1] 8.531429
```
Note that base R already has a built-in function for calculating the sample variance `var()`.


## Vectors with other types of data

Vectors can contain other types of data, in particular characters and logicals.  

The elements of a logical vector can have the values `TRUE`, `FALSE`, and `NA` (for "not available"). The first two are often abbreviated as T and F, respectively. Note however that T and F are just variables which are set to TRUE and FALSE by default, but are not reserved words and hence can be overwritten by the user. **Hence, it is better to always use TRUE and FALSE.**


```r
c("this", "is", "a", "string", "vector")
```

```
## [1] "this"   "is"     "a"      "string" "vector"
```

However, an important feature of vectors is that they store the same types of data. 

## Automatic coercing within vectors

In general, coercion is an attempt by R to be flexible with data types. When an entry does not match the expected type, R tries to guess what you meant. But this can also lead to confusion.

We earlier said that vectors must be all of the same type. So if we try to combine say numbers and characters, you might expect an error. But let's try to combine numeric and characters in the same vector:


```r
c(1, "South Africa", 3, "England")
```

```
## [1] "1"            "South Africa" "3"            "England"
```

You do not even get a warning !! So what has happened?

R has converted the 1 and the 3 to character strings. And the class of the vector is character. Even though 1 and 3 were originally numbers when we wrote it out, R has converted them to character.

*R coerced the data into a character string.* It guessed that because we put a character string there in the middle, we meant the 1 and the 3 to actually also be character strings.

Again, be aware that R will coerce variables without any warning !

It will only throw an error, when it attemps to coerce a variable into a new type, but cannot do it.


## Index vectors to obtain subsets of a vector

Subsets of the elements of a vector may be selected by appending to the name of the vector an *index vector* in square brackets. More generally any expression that evaluates to a vector may have subsets of its elements similarly selected by appending an index vector in square brackets immediately after the expression.

Such index vectors can be any of four distinct types.

### Index = A logical vector. 

In this case the index vector is recycled to the same length as the vector from which elements are to be selected. Values corresponding to TRUE in the index vector are selected and those corresponding to FALSE are omitted. For example, the following lines creates (or re-creates) an object `y` which will contain the values of x greater than 4, in the same order. 

```r
y <- x[x > 4]
y
```

```
## [1] 10.4  5.6  7.2 10.2  7.8
```

### Index = A vector of integers. 

In this case the values in the index vector must lie in the set {1, 2, …, length(x)}. The corresponding elements of the vector are selected and concatenated, in that order, in the result. The index vector can be of any length and the result is of the same length as the index vector. 


```r
(x <- rnorm(10))
```

```
##  [1]  0.6058875 -1.8179560  0.6300986 -0.2761841 -0.2841597 -0.9193220
##  [7] -0.1162478  1.8173120  0.3706279  0.5202165
```

```r
(y <- x[1:5])
```

```
## [1]  0.6058875 -1.8179560  0.6300986 -0.2761841 -0.2841597
```

```r
(z <- x[c(1,3,2,7)])
```

```
## [1]  0.6058875  0.6300986 -1.8179560 -0.1162478
```

The use of index vectors can be very powerful to create complex sequences:


```r
c("x","y")[rep(c(1,2,2,1), times=2)]
```

```
## [1] "x" "y" "y" "x" "x" "y" "y" "x"
```

### Index = A vector of negative Integers 

Such an index vector specifies the values *to be excluded* rather than included. Thus:


```r
x[-(1:5)]  # gives y all but the first five elements of x.
```

```
## [1] -0.9193220 -0.1162478  1.8173120  0.3706279  0.5202165
```

###  Index = A vector of character strings. 

This possibility only applies where an object has a names attribute to identify its components. In this case a sub-vector of the names vector may be used in the same way as the positive integral labels in item 2 further above.


```r
fruit <- c(5, 10, 1, 20)
names(fruit) <- c("orange", "banana", "apple", "peach")  # we add a name attribute
lunch <- fruit[c("apple","orange")]  # then we can select the elements by their names
lunch
```

```
##  apple orange 
##      1      5
```

Note in this case, that we added a feature to the vector, because each element of the vector is associated with a name. This is obtained by using the function `names()`. 

The advantage is that alphanumeric names are often easier to remember than numeric indices. 

## Exercices

### Exercise 1

Create vectors that correspond to the following names

+ themonths 
+ primes1to7

{{< spoiler text="Show the answer" >}}

```r
themonths <- c("Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sept", "Oct", "Nov", "Dec")
themonths
```

```
##  [1] "Jan"  "Feb"  "Mar"  "Apr"  "May"  "Jun"  "Jul"  "Aug"  "Sept" "Oct" 
## [11] "Nov"  "Dec"
```

```r
primes1to7 <- c(1,2,3,5,7)
primes1to7
```

```
## [1] 1 2 3 5 7
```
{{< /spoiler >}}

### Exercise 2

+ Create a vector "noDays" of the number of days for each month of the year. 
+ Add a "names" attribute to each number to make it easier to identify them (use first three letters of the month names to identify them)
+ Create a "spring" vector that only shows the number of days per month but only for the months of April, May and June
+ What happens if you type noDays[c("Jan", "Juin")] ? (try to guess before running it with R)

{{< spoiler text="Show the answer" >}}

```r
noDays <- c(31, 28, 31,30,31,30,31,31, 30, 31, 30, 31 )
names(noDays) <- c("Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec")
noDays
```

```
## Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec 
##  31  28  31  30  31  30  31  31  30  31  30  31
```

```r
spring <- noDays[c("Apr", "May", "Jun")]
spring
```

```
## Apr May Jun 
##  30  31  30
```
{{< /spoiler >}}


### Exercise 3

Create a vector seq96 that contains 96 values and follows the sequence 1, 2, 3, 1, 2, 3, …., 1, 2, 3

{{< spoiler text="Show the answer" >}}

```r
seq96 <- rep(1:3,times = 96/3)
seq96
```

```
##  [1] 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2
## [39] 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1
## [77] 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3
```
{{< /spoiler >}}

### Exercise 4

+ Create a vector seq10 that contains the following 10 values in that order 1,1,2,2,3,3,4,4,5,5 without having to key in that sequence
+ Create a subset  of seq10 that includes only the values 1 to 3 and save it into a variable named seq6

{{< spoiler text="Show the answer" >}}

```r
seq10 <- rep(1:5, each=2)
seq10
```

```
##  [1] 1 1 2 2 3 3 4 4 5 5
```

```r
seq6 <- seq10[seq10<4]
seq6
```

```
## [1] 1 1 2 2 3 3
```
{{< /spoiler >}}

### Problem 1 

*Study the normal distribution*

Sample 400 numbers from a normal distribution with mean 10 and standard deviation 2.
From these 400 numbers, select the numbers that are higher than 14. 
How many numbers did you find? Save this size in a variable `l1`
Repeat this exercise another 2 times, and save the size into variables `l2`and `l3`
Take the mean value of the sizes.

Comment your results. (is this in line with what you know about the normal distribution?)

{{< spoiler text="Show the calculations" >}}

```r
v400 <- rnorm(400, mean=10, sd=2)
v400_sup14 <- v400[v400>14]
v400_sup14
```

```
##  [1] 14.39367 14.09838 14.29013 14.66102 14.95422 14.31544 14.36000 15.31158
##  [9] 14.96710 14.13249 15.49481 14.32519 14.53572 14.15342 14.51954 14.48407
```

```r
l1 <- length(v400_sup14)

# let's repeat it
v400 <- rnorm(400, mean=10, sd=2)
v400_sup14 <- v400[v400>14]
v400_sup14
```

```
##  [1] 14.47825 16.66147 14.22826 14.25493 14.18614 14.23701 14.63445 14.33866
##  [9] 14.60725 14.04179 14.22526 16.18739 14.99384 14.23403
```

```r
l2 <- length(v400_sup14)

# let's repeat it
v400 <- rnorm(400, mean=10, sd=2)
v400_sup14 <- v400[v400>14]
v400_sup14
```

```
## [1] 14.72872 14.64848 14.91415 14.04389
```

```r
l3 <- length(v400_sup14)

(l1+l2 + l3)/3
```

```
## [1] 11.33333
```

{{< spoiler text="Show the comments" >}}
The number 14 corresponds to `mean + 2 sd`; When we draw repeated samples from the normally distributed variable, we know we have a probability of 2.5% to get number larger than  `mean + 2 sd`. 
So, if we take repeated samples, the mean size of the number above 14 should be: 400 *(1-0.975) = `400 *(1-0.975)`

You took only 3 samples, so you may have found a different result. But if you repeat this experience more times, you should see that the average sample size is converging towards the value of ten.

{{< /spoiler >}}

