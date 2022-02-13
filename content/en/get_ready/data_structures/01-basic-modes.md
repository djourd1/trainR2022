---
title: Basic Modes
author: Damien Jourdain
date: '2019-02-19'
slug: basic-modes
categories:
  - data structure
tags: []
type: book
weight: 10
output:
  html_document:
    keep_md: true
---


<div data-datacamp-exercise data-lang="r">
	<code data-type="sample-code">
		# Create a variable a, equal to 5


		# Print out a


	</code>
	<code data-type="solution">
		# Create a variable a, equal to 5
		a <- 5

		# Print out a
		print(a)
	</code>
	<code data-type="sct">
		test_object("a")
		test_function("print")
		success_msg("Great job!")
	</code>
	<div data-type="hint">Use the assignment operator (<code><-</code>) to create the variable <code>a</code>.</div>
</div>


## Learning objectives

All objects have two intrinsic attributes: *mode* and *length*. The mode is the basic type of the elements of the object. There are four main modes: numeric, character, complex, and logical (FALSE or TRUE). 

Once the mode of an object is known, it they can be assembled into into more complex data structures (vectors, arrays, dataframes, etc.) that we will study later.

In this section you will learn about the modes you will be using for most econometric analyses.  

+ [Numerics (real numbers) ](#numeric)
+ [Logicals](#logicals)
+ [Characters](#characters)
+ [Other objects](#other-objects)

## Numerics 
Numbers are stored as *numerics* in R.  

You may want to create some numbers with decimal values. It is the default computational mode for numbers. However, sometimes you will want to create integer values (i.e. to store values such number of kids in a household).

If you assign a decimal value to a variable $ x $, it will be of numeric mode. Note that, even if we assign an integer to a variable $ y $, it is still being saved as a numeric value.


```r
(x <-  10.5)   # assign a decimal value 
```

```
## [1] 10.5
```

```r
mode(x)       # print the mode of x 
```

```
## [1] "numeric"
```

```r
y <- 1 
mode(y)
```

```
## [1] "numeric"
```


## Integers

If you want to work with integers and not real numbers, you have to indicate it clearly. This is done by creating an additional attribute indicating it is an integer. 

There are two ways to do this. The first one is to add an "L" after the number. The second one is to use the function `as.integer()`. 

If you want to check, whether the number you have stored is treated as an integer, you can use the function `is.integer()`. 


```r
(z <- 3L)  # declare an integer by appending an L suffix
```

```
## [1] 3
```

```r
is.integer(z)
```

```
## [1] TRUE
```

```r
(z <- as.integer(3))  # use the as.integer( ) function
```

```
## [1] 3
```

```r
is.integer(z)
```

```
## [1] TRUE
```

Note that the function `as.integer` can *coerce* a real number or even some character variables into an integer value. 

```r
(z <- as.integer(3.14))
```

```
## [1] 3
```

```r
is.integer(z)
```

```
## [1] TRUE
```

```r
(z <- as.integer("5.27"))  # coerce a decimal string 
```

```
## [1] 5
```

```r
is.integer(z)
```

```
## [1] TRUE
```

`as.integer()` will also be useful to transform a variable into an integer.


```r
z <- 3.2
zL  #adding L after the name of the variable will not work
```

```
## Error in eval(expr, envir, enclos): object 'zL' not found
```


```r
z <- 3.2
int_z <- as.integer(z)
mode(int_z)
```

```
## [1] "numeric"
```

```r
is.integer(int_z)
```

```
## [1] TRUE
```

Note that the mode is still "numeric", but you have added an additional attribute saying it is an integer.

### Arithemic operators with numerics and integers

Type the following in your console


```r
(x <- 1L + 2)
```

```
## [1] 3
```

```r
mode(x)
```

```
## [1] "numeric"
```

```r
(y <- 1L + 2L)
```

```
## [1] 3
```

```r
mode(y)
```

```
## [1] "numeric"
```

In the first case (x), you are asking R to add-up an integer and a real number. R cannot do that immediately since they are objects of different types. However, R does not throw an error. Instead, R will first convert (coerce) the integer into a numeric, and then make the addition between two numerics and output a numeric. 

Again, R is very helpful and make this operation transparent to you. However, if you really expect an integer as a result, you need to be extra careful. 

## Logicals

A logical value is either `TRUE` or `FALSE`

A logical is often created via comparison between variables.


```r
x <- 1; y <- 2   # sample values 
(z <-  x > y)      # is x larger than y? 
```

```
## [1] FALSE
```

```r
class(z)
```

```
## [1] "logical"
```

Standard logical operations are "&" (and), "|" (or), and "!" (negation).


```r
u <- TRUE; v <- FALSE 
u & v          # u AND v 
```

```
## [1] FALSE
```

```r
u | v          # u OR v 
```

```
## [1] TRUE
```

```r
!u             # negation of u 
```

```
## [1] FALSE
```

Further details and related logical operations can be found when typing `help("&")` 

Finally, it is often useful to perform arithmetic on logical values. You just have to remember that the `TRUE` has the value 1, while `FALSE` has value 0.

```r
(as.integer(TRUE))  # is k an integer? 
```

```
## [1] 1
```

```r
TRUE + TRUE
```

```
## [1] 2
```

{{% callout note %}}
A logical variable can take the values "TRUE" et "FALSE". You will sometimes find them in their abbreviated form as  "T" and "F". However, you need to know that  "T" and "F" are  variables pre-defined as TRUE and FALSE, but are not reserved names and therefore are not protected; as a result you can change their values and this may lead to confusions. **Therefore, it will always be preferable to use  TRUE and FALSE.**
{{% /callout %}}


## Characters

In R, strings are stored as a `character` object. Strings are surrounded by two `"`.   

```r
(x <- "This is a string") 
```

```
## [1] "This is a string"
```

```r
class(x)
```

```
## [1] "character"
```

If you do not use the `"`, R will look for a variable instead of a string of characters, and will most likely throw an error message.  


```r
(x <- This is a string) 
class(x)
```

```
## Error: <text>:1:12: unexpected symbol
## 1: (x <- This is
##                ^
```


We can convert objects into character values with the `as.character()` function:

```r
(x = as.character(3.14)) 
```

```
## [1] "3.14"
```

Two character values can be concatenated with the `paste()` function.

```r
fname = "Joe"; lname ="Smith" 
paste(fname, lname) 
```

```
## [1] "Joe Smith"
```

To extract a substring, we use the `substr()` function. Here is an example showing how to extract the substring between the third and fourteenth positions in a string.

```r
substr("Mary has a little lamb.", start=3, stop=14) 
```

```
## [1] "ry has a lit"
```

And to replace the first occurrence of the word "little" by another word "big" in the string, we apply the `sub()` function.

```r
sub("little", "big", "Mary has a little lamb.") 
```

```
## [1] "Mary has a big lamb."
```

## Other Objects {#other-objects}

R also provides special objects that are useful for missing data, or for specific number like Infinity. 

NA is in fact a logical constant of length 1 which contains a missing value indicator. It will appear in your datasets and answers, when a value is missing.

```r
class(NA)
```

```
## [1] "logical"
```

NULL represents the null object in R. NULL is returned by expressions and functions whose value is undefined.
Contrary to NA, it is not a logical variable.


```r
class(NULL)
```

```
## [1] "NULL"
```

`Inf` and `-Inf` are positive and negative infinity. Their class is numeric

```r
1+ Inf 
```

```
## [1] Inf
```

```r
class(Inf)
```

```
## [1] "numeric"
```

NaN is a reserved word in R. Although it means "Not a Numeric", it belongs also the class numeric.



```r
class(NaN)
```

```
## [1] "numeric"
```

```r
# See the difference between the two expressions
3 / 0 ## = Inf a non-zero number divided by zero creates infinity
```

```
## [1] Inf
```

```r
0 / 0  ## =  NaN   # 0/0 is not a valid operation
```

```
## [1] NaN
```


## Exercises

### Exercise 1

Suppose you have typed this command, 



```r
x <- 2L + TRUE
y <- 3 + FALSE
```

+ what is the value of x? what is be the mode of x? Is x an integer?
+ what is the value of y? what is the mode of y? Is x an integer? 

Before answering this question, try to imagine in what mode some elements may have been coerced by R? 

{{< spoiler text="Show the result" >}}


```r
x <- 2L + TRUE
x
```

```
## [1] 3
```

```r
mode(x)
```

```
## [1] "numeric"
```

```r
is.integer(x)
```

```
## [1] TRUE
```


```r
y <- 3 + FALSE
y
```

```
## [1] 3
```

```r
mode(y)
```

```
## [1] "numeric"
```

```r
is.integer(y)
```

```
## [1] FALSE
```

{{< /spoiler >}}

### Exercise 2: Include " in a character variable

A value of mode character is input with double quotes ". It is possible to include this character in the variable if it follows a backslash \. The two charaters altogether \" will be treated in a specific way by some functions such as cat for display on screen.

+ Create a variable x that contains the string "Double quotes \" delimitate R’s strings."
+ Consult the help file of the functions print() and cat()
+ See the difference between print(x) and cat(x)
+ Using the function `sub()`, create a new variable `y` from the variable x where you eliminate the two characters \"
+ use the function cat to display y

{{< spoiler text="Show the solution" >}}


```r
x <- "Double quotes \" delimitate R’s strings."
print(x)
```

```
## [1] "Double quotes \" delimitate R’s strings."
```

```r
cat(x)
```

```
## Double quotes " delimitate R’s strings.
```


```r
y <- sub(" \"","", x)
cat(y)
```

```
## Double quotes delimitate R’s strings.
```

{{< /spoiler >}}

### Exercise 3

Suppose you have typed this command, 


```r
x <- as.numeric(2L) + 3L
```

Tip: Instead of using `as.integer()`, we used `as.numeric()`. Check the help file of as.numeric before answering the question.

What is the mode of x? 

{{< spoiler text="Show the solution" >}}


```r
x <- as.numeric(2L) + 3L
mode(x)
```

```
## [1] "numeric"
```


{{< /spoiler >}}
