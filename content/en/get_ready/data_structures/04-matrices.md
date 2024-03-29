---
title: Matrices
author: Damien Jourdain
slug: matrices
categories:
  - R
  - data structure
tags: [R, matrix]
type: book
weight: 40 
output:
  html_document:
    keep_md: true
---

## Learning objectives

In this section you will learn about matrices, how to create and manipulate them.

+ [What is a matrix? ](#what-is-a-matrix)
+ [Create a matrix ](#create-a-matrix)
+ [Access elements of a matrix](#access-elements-of-a-matrix)
+ [Matrix calculations](#matrix-calculations)
+ [Common matrix functions](#common-matrix-functions)

## What is a matrix

A matrix is a collection of data elements **of the same basic type** arranged in a two-dimensional rectangular layout. 

\\begin{pmatrix}
    x_{11} & x_{12} & x_{13} & \\dots  & x_{1n} \\\\
    x_{21} & x_{22} & x_{23} & \\dots  & x_{2n} \\\\
    \\vdots & \\vdots & \\vdots & \\ddots & \\vdots \\\\
    x_{d1} & x_{d2} & x_{d3} & \\dots  & x_{dn}
\\end{pmatrix}

We can create a matrix containing only characters or only logical values. However, it is most common to use matrices containing *numeric* elements for mathematical calculations.
 
## Create a matrix

### Using the `matrix()` function

A matrix can be created directly using the `matrix(data, nrow, ncol, byrow, dimnames)` function, where

+ *data* is an input vector which becomes the data elements of the matrix.
+ *nrow* is the number of rows to be created.
+ *ncol* is the number of columns to be created.
+ *byrow* is a logical. If TRUE then the input vector elements are arranged by row.
+ *dimnames* is the names assigned to the rows and columns.

In the following example, we create a vector with 12 values that we feed as an argument to the function `matrix()`. Note that once I gave one of the two arguments, `nrow` or `ncol`, the other one is deducted by the function. 


```r
# Elements are arranged sequentially by row.
# Note: you do not have to enter all the arguments
matrix(c(1:12), nrow = 4, byrow = TRUE)
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    4    5    6
## [3,]    7    8    9
## [4,]   10   11   12
```

To see the effect of the argument `byrow`, you can compare the above matrix with another matrix where you gave the argument `byrow=FALSE`.


```r
# Elements are now arranged sequentially by column.
matrix(c(1:12), nrow = 4, byrow = FALSE)
```

```
##      [,1] [,2] [,3]
## [1,]    1    5    9
## [2,]    2    6   10
## [3,]    3    7   11
## [4,]    4    8   12
```

{{% callout warning %}}

Be aware that `matrix()` will only issue a warning if the vector length is not a multiple of the number of rows. In such case, the last row of the matrix will be completed with the first components of the vector: this is very unlikely it was the matrix you wanted to work with. 

Look at what happens in the following example:


```r
# Elements are arranged sequentially by row.
# Note: you do not have to enter all the arguments
ans <- matrix(c(1:14), nrow = 4, byrow = TRUE)
```

```
## Warning in matrix(c(1:14), nrow = 4, byrow = TRUE): data length [14] is not a
## sub-multiple or multiple of the number of rows [4]
```

```r
ans
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    2    3    4
## [2,]    5    6    7    8
## [3,]    9   10   11   12
## [4,]   13   14    1    2
```
{{% /callout %}}

The names of the rows and the columns should appear as a list of two vectors. (more on lists later)


```r
# Define the column and row names.
names_row <- c("row1", "row2", "row3", "row4")
names_col <- c("col1", "col2", "col3")

matrix(c(1:12), nrow = 4, byrow = TRUE, dimnames = list(names_row, names_col))
```

```
##      col1 col2 col3
## row1    1    2    3
## row2    4    5    6
## row3    7    8    9
## row4   10   11   12
```

Note that you can also change the column and row names after the matrix was created using the function rownames and colnames.


```r
# first create a matrix of 12 numbers into 4 columns
m <- matrix(c(1:12), ncol=4)

# Define the column and row names after the definition
rownames(m) <- c("row1", "row2", "row3")
colnames(m) <- c("col1", "col2", "col3", "col4")
m
```

```
##      col1 col2 col3 col4
## row1    1    4    7   10
## row2    2    5    8   11
## row3    3    6    9   12
```

Finally, note that the function matrix can be used to create an "empty" matrix, i.e. a matrix filled with `NA` with the following instructions:

```r
matrix(nrow = 2, ncol = 4)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]   NA   NA   NA   NA
## [2,]   NA   NA   NA   NA
```

Here is a video, that reviews the functions you just read about

{{<youtube Cmn7IEUDRu8 >}}

### Using cbind() and rbind() functions

You can also create a matrix by collating (binding) vectors together. Examples below show you different way to create column, row or rectangular matrices with `cbind` and `rbind` functions.  


```r
#create a three columns matrix from a vector
rbind(1:3)
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
```


```r
ans <- cbind(rep(0, 3))
ans
```

```
##      [,1]
## [1,]    0
## [2,]    0
## [3,]    0
```

You can create a rectangular matrix, binding several vectors of the same size

```r
cbind(1:3, 4:6, 7:9)
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

{{% callout note %}}
Note that the `cbind` and `rbind` functions can be used to combine matrices. But you have to make sure the matrices have compatible dimensions


```r
A <- matrix(1:12, ncol=3); A
```

```
##      [,1] [,2] [,3]
## [1,]    1    5    9
## [2,]    2    6   10
## [3,]    3    7   11
## [4,]    4    8   12
```

```r
B <- matrix(1:9, ncol=3); B
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

```r
# the matrices A and B have the same number of columns
# you can combine the using rbind()
rbind(A, B)
```

```
##      [,1] [,2] [,3]
## [1,]    1    5    9
## [2,]    2    6   10
## [3,]    3    7   11
## [4,]    4    8   12
## [5,]    1    4    7
## [6,]    2    5    8
## [7,]    3    6    9
```


```r
# But it will throw an error if we use cbind
cbind(A, B)
```

```
## Error in cbind(A, B): number of rows of matrices must match (see arg 2)
```
{{% /callout %}}

### Using specialized functions

The function diag() is very handy to create an identity matrix of a given size

```r
diag(4)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    0    0    0
## [2,]    0    1    0    0
## [3,]    0    0    1    0
## [4,]    0    0    0    1
```

### Exercises

#### Exercise 1: matrix of random numbers

1. Create a vectors with 20 numbers taken from a standardized normal distribution
2. Use that vector to create a matrix with 5 columns, where the distribution of the numbers is done by rows.
3. Change the names of the rows to "row_1", "row_2", etc. and display the final matrix

{{< spoiler text="Show the answer" >}}

For the first question we will use the function `rnorm()`

```r
vec <- rnorm(n = 20) # the default is standardized normal distribution
```
For the second question, you can use the function matrix

```r
mat <- matrix(vec, ncol=5, byrow=TRUE)
```
For the third question, you can use the function rownames(). 
You have two solutions

1. You can enter the different names manually

```r
rownames(mat) = c("row_1", "row_2", "row_3", "row_4")
```
2. A more elegant solution, especially when there are many rows is to use the function `paste()`
(*search for help on that function if you are not familiar with it*)

```r
rownames(mat) = paste("row", 1:4, sep="_")
```

Note that you can use a single line of command for these different operations. But the code becomes a bit more difficult to read. 


```r
(mat <- matrix(rnorm(20), ncol=5, byrow=TRUE, dimnames = list(paste("row", 1:4, sep="_"), c())))
```

```
##              [,1]       [,2]       [,3]       [,4]        [,5]
## row_1  0.28580248  0.6341208 -0.7095475 -0.8197365 -0.55631912
## row_2 -0.08880623  0.9047568 -1.2499325 -1.6045304 -0.01835697
## row_3  0.82379945  0.3816980 -0.1459957  1.5717562  0.12109315
## row_4 -1.27597201 -0.9873149  0.7187094  1.0661660  1.75006955
```
{{< /spoiler >}}


#### Exercise 2: Create matrices with a pattern

1. Create a 6x5 matrix where the first line is composed of 1, the second line contains only 2, the third line contains only 3, etc. (tip: use the function `rep`); save your result as mat.
2. Comment the results of the command `diag(mat)`

{{< spoiler text="Show the answer" >}}
To rapidly create this matrix, you can create a vector such as the first 5 numbers are 1 (corresponding to your first row), the following 5 numbers are 2 (second row), etc.  

For the first question, we have seen that a very convenient way to create such a vector is to use the function `rep()`


```r
vec <- rep(1:6, each =5) 
mat <- matrix(vec, ncol=5, byrow=TRUE); mat
```

```
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    1    1    1    1    1
## [2,]    2    2    2    2    2
## [3,]    3    3    3    3    3
## [4,]    4    4    4    4    4
## [5,]    5    5    5    5    5
## [6,]    6    6    6    6    6
```

For the second question:

```r
diag(mat)
```

```
## [1] 1 2 3 4 5
```
The matrix mat is not square. The diagonal of a non-square matrix is not properly defined. The function `diag()` is actually taking the arbitrary decision to take the diagonal of the 5x5 matrix (a matrix where the sixth row has been eliminated). 

**Be careful, since the function does throw neither an error nor a warning ! **

{{< /spoiler >}}

## Access elements of a matrix

What you've learned for vectors can be applied for matrices. Just remember that the first index is for the rows, and the second index is for the columns.


```r
ans <- matrix(1:12, nrow = 3)
ans
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    4    7   10
## [2,]    2    5    8   11
## [3,]    3    6    9   12
```

```r
ans[1, 3] # will return the element in row 1 and column 3
```

```
## [1] 7
```

```r
ans[1,]  #will return the first row
```

```
## [1]  1  4  7 10
```

```r
ans[,2] # will return the second column
```

```
## [1] 4 5 6
```

```r
ans[, 1:2] #will return the first two columns
```

```
##      [,1] [,2]
## [1,]    1    4
## [2,]    2    5
## [3,]    3    6
```

You can also create a boolean matrix and use it to screen out the elements

```r
indices <- ans > 5   #will create a boolean matrix
indices  
```

```
##       [,1]  [,2] [,3] [,4]
## [1,] FALSE FALSE TRUE TRUE
## [2,] FALSE FALSE TRUE TRUE
## [3,] FALSE  TRUE TRUE TRUE
```

```r
ans[indices]  # will return a vector 
```

```
## [1]  6  7  8  9 10 11 12
```


## Matrix calculations
### Multiplication by a Scalar

```r
A <- matrix(1:12, ncol=4, byrow=FALSE); A
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    4    7   10
## [2,]    2    5    8   11
## [3,]    3    6    9   12
```

```r
A * 0.1
```

```
##      [,1] [,2] [,3] [,4]
## [1,]  0.1  0.4  0.7  1.0
## [2,]  0.2  0.5  0.8  1.1
## [3,]  0.3  0.6  0.9  1.2
```

### Matrix Addition & Subtraction

```r
B <- matrix(2:13, ncol=4, byrow=FALSE); B
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    2    5    8   11
## [2,]    3    6    9   12
## [3,]    4    7   10   13
```

```r
A + B
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    3    9   15   21
## [2,]    5   11   17   23
## [3,]    7   13   19   25
```

```r
A - B
```

```
##      [,1] [,2] [,3] [,4]
## [1,]   -1   -1   -1   -1
## [2,]   -1   -1   -1   -1
## [3,]   -1   -1   -1   -1
```

### Element-wise multiplication
$$
\\begin{pmatrix}
    x_{11} & x_{12}  \\\\
    x_{21} & x_{22}  
\\end{pmatrix}
\\times
\\begin{pmatrix}
    y_{11} & y_{12}  \\\\
    y_{21} & y_{22}  
\\end{pmatrix}
\=
\\begin{pmatrix}
   x_{11} \\times y_{11} & x_{12} \\times y_{12}   \\\\
   x_{21} \\times y_{21} & x_{22} \\times y_{22} 
\\end{pmatrix} 
$$



```r
A*B
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    2   20   56  110
## [2,]    6   30   72  132
## [3,]   12   42   90  156
```

### Transpose of a matrix

$$ \\begin{pmatrix}
    x_{11} & x_{12}  \\\\
    x_{21} & x_{22}  
\\end{pmatrix}'
\=
\\begin{pmatrix}
    y_{11} & x_{21}  \\\\
    x_{12} & x_{22}  
\\end{pmatrix} $$


```r
BT <- t(B); BT 
```

```
##      [,1] [,2] [,3]
## [1,]    2    3    4
## [2,]    5    6    7
## [3,]    8    9   10
## [4,]   11   12   13
```

### Matrix multiplication
With two conformable matrices, the matrice multiplication is done with the operator `%*%`

$$ \\begin{pmatrix}
    x_{11} & x_{12} & x_{13}  \\\\
    x_{21} & x_{22} & x_{23} 
\\end{pmatrix}
\\times
\\begin{pmatrix}
    y_{11} & y_{12}  \\\\
    y_{21} & y_{22}  \\\\
    y_{31} & y_{32}  
\\end{pmatrix}
\=
\\begin{pmatrix}
    x_{11}.y_{11} + x_{12} . y_{21} + x_{13}.y_{31}  & 
      x_{21}.y_{12} + x_{22}.y_{22} + x_{23}.y_{32}  \\\\ 
    x_{11}.y_{11} + x_{12}.y_{21} + x_{13}.y_{31} & 
      x_{21}.y_{12} + x_{22}.y_{22} + x_{23}.y_{32}
\\end{pmatrix} $$




```r
A
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    4    7   10
## [2,]    2    5    8   11
## [3,]    3    6    9   12
```

```r
BT
```

```
##      [,1] [,2] [,3]
## [1,]    2    3    4
## [2,]    5    6    7
## [3,]    8    9   10
## [4,]   11   12   13
```

```r
A %*% BT
```

```
##      [,1] [,2] [,3]
## [1,]  188  210  232
## [2,]  214  240  266
## [3,]  240  270  300
```

This operator also allows the multiplication of a matrix by a vector.

$$ \\begin{pmatrix}
    x_{11} & x_{12}  \\\\
    x_{21} & x_{22}  
\\end{pmatrix}
\\times
\\begin{pmatrix}
    y_1  \\\\
    y_2  
    \\end{pmatrix}
\=
\\begin{pmatrix}
   x_{11} . y_1 + x_{12} . y_2   \\\\
    x_{21} . y_1 + x_{22} . y_2 
\\end{pmatrix} $$



```r
A <- matrix(1:4, ncol=2); A
```

```
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
```

```r
V <- c(2,5); V
```

```
## [1] 2 5
```

```r
A %*% V
```

```
##      [,1]
## [1,]   17
## [2,]   24
```

### Exercises

#### Sum of values 

When postmultiplying a one row matrix with a compatible vector of 1, you obtain the sum of the numbers in that row.

Use this property to calculate rapidly the sum of integers between 1 and 200.

{{< spoiler text="Show the answer" >}}


```r
x <- 1:200
i = rep(1, 200)
x%*%i
```

```
##       [,1]
## [1,] 20100
```

{{< /spoiler>}}

#### Idempotent matrices

In econometrics, you will often encounter idempotent matrices. A matrix is idempotent when multiplied by itself, yields itself. 

That is, the matrix $A$ is idempotent if and only if $A^{2}=A$. For this product $A^{2}$ to be defined, $A$ must necessarily be a square matrix. 

For any $\theta$, the following matrix will be idempotent:

$$ 
ID=\\frac{1}{2}{\\begin{pmatrix}1-\\cos \\theta &\\sin \\theta \\\ \\sin \\theta &1+ \\cos \\theta \\end{pmatrix}}
$$
In this exercise, you will not prove it, but you can quickly check whether this is true for a few values of theta.

1. Initiate a variable `theta` and assign the value of $\pi / 6$
2. Create the matrix $ID$
3. Calculate $ID^2$ and check whether $ID^2 = ID$
5. Check that it is still working with another value of `theta`

{{< spoiler text="Show the answer" >}}


```r
theta <- pi/6;  #note that pi is predefined in R
ID <- matrix(c(1- cos(theta), sin(theta), sin(theta), 1+ cos(theta)), ncol=2, byrow=TRUE)/2; ID
```

```
##           [,1]      [,2]
## [1,] 0.0669873 0.2500000
## [2,] 0.2500000 0.9330127
```

```r
ID %*% ID
```

```
##           [,1]      [,2]
## [1,] 0.0669873 0.2500000
## [2,] 0.2500000 0.9330127
```

{{< /spoiler>}}

## Common matrix functions
For a matrix A


```r
A <- matrix(seq(2, 8, 2), ncol=2); A
```

```
##      [,1] [,2]
## [1,]    2    6
## [2,]    4    8
```

### Number of Rows and Columns

```r
dim(A)
```

```
## [1] 2 2
```

```r
nrow(A)
```

```
## [1] 2
```

```r
ncol(A)
```

```
## [1] 2
```

### Computing Column and Row sums


```r
colSums(A)
```

```
## [1]  6 14
```

```r
rowSums(A)
```

```
## [1]  8 12
```

```r
sum(A)  #total over all elements
```

```
## [1] 20
```


### Diagonal elements 

```r
diag(A)
```

```
## [1] 2 8
```

{{% callout note %}}

The attentive reader will have noticed that the function `diag()` will react differently depending on the arguments that was given. For those familiar with object programming, this should not come as a surprise. For others, it is an invitation to be careful when providing an argument to a  function. 


```r
diag(3) ## when provided a scalar, it will output a identity matrix of that size
```

```
##      [,1] [,2] [,3]
## [1,]    1    0    0
## [2,]    0    1    0
## [3,]    0    0    1
```

```r
diag(c(1,4,5)) ## when provided a vector, it will output a diagonal matrix
```

```
##      [,1] [,2] [,3]
## [1,]    1    0    0
## [2,]    0    4    0
## [3,]    0    0    5
```

```r
(m <- matrix(1:9, ncol=3))
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

```r
diag(m) ## when provided a matrix, it will output its main diagonal
```

```
## [1] 1 5 9
```

{{% /callout %}}



### Determinant

```r
det(A)
```

```
## [1] -8
```


### Eigenvalues and eigenvectors

```r
eigA <- eigen(A); eigA
```

```
## eigen() decomposition
## $values
## [1] 10.7445626 -0.7445626
## 
## $vectors
##            [,1]       [,2]
## [1,] -0.5657675 -0.9093767
## [2,] -0.8245648  0.4159736
```
Note that the output of the function eigen is a list. If you are only interested by the eigenvalues, just use

```r
eigA <- eigen(A) 
eigA$values
```

```
## [1] 10.7445626 -0.7445626
```

### Inverse 
if A is a regular matrix (square and inversible), the inverse of A is found using the function `solve()`


```r
invA <- solve(A); invA
```

```
##      [,1]  [,2]
## [1,] -1.0  0.75
## [2,]  0.5 -0.25
```

```r
invA %*% A
```

```
##      [,1] [,2]
## [1,]    1    0
## [2,]    0    1
```

```r
A %*% invA
```

```
##      [,1] [,2]
## [1,]    1    0
## [2,]    0    1
```

### Exercise: solve a system of linear equations

So far, we have used the function `solve()` with only one argument, the matrix to be inversed. When we do this, the output is the inverse of the matrix.

However, `solve()`, as its name suggests, can be also used to solve systems of linear equations. You need then to include two arguments: the matrix corresponding to the LHS of the system, and the vector of the RHS.

In this exercise, you will use the function `solve()` to resolve the following system of equation.

$$ \\begin{matrix}
    3 u_1 + 2 u_2  & = & 1 \\\\
   -3 u_1 + 4 u_2  & = & 0.5 
\\end{matrix} $$

This system can be written in the matrix format A.x = b, 

1. Identify the matrix A and the vector b corresponding to this system
2. Create the matrix A and the vector b
3. Solve the system of equation and save the result as a vector named sol
4. Check that your solution is correct

{{< spoiler text="Show the answer" >}}

Let's create the square matrix A and the vector b

```r
A <- matrix(c(3,-3,2,4), ncol=2); A
```

```
##      [,1] [,2]
## [1,]    3    2
## [2,]   -3    4
```

```r
b <- c(1, 0.5); b
```

```
## [1] 1.0 0.5
```
Now we can find the vector $sol = (u_1, u_2)$ using the function `solve()`


```r
sol <- solve(A,b)
sol
```

```
## [1] 0.1666667 0.2500000
```

You can check your solutions by the outer product of A and sol, and verify it is equal to b

```r
A %*% sol
```

```
##      [,1]
## [1,]  1.0
## [2,]  0.5
```

{{< /spoiler >}}

