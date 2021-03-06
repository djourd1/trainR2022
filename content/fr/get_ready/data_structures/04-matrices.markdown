---
title: Les matrices
author: Damien Jourdain
date: '2020-08-20'
slug: matrices
categories:
  - R
tags: []
type: book
weight: 40 

---

## Objectifs d'apprentissage

Dans cette section, vous découvrirez les matrices, comment les créer et les manipuler.

+ [Qu'est-ce qu'une matrice ? ](#what-is-a-matrix)
+ [Créer une matrice ](#create-a-matrix)
+ [Accès aux éléments d'une matrice](#access-elements)
+ [Calcul matriciel](#calcul-matriciel)
+ [Fonctions matricielles communes](#fonctions-matricielles-communes)

## Qu'est-ce qu'une matrice ? {#what-is-a-matrix}

Une matrice est un tableau rectangulaire de $ i \times j $ de données **d'un même type**. Il y a $ i $ lignes, et dans chaque ligne $ j $ éléments. 

\\begin{pmatrix}
    x_{11} & x_{12} & x_{13} & \\dots  & x_{1n} \\\\
    x_{21} & x_{22} & x_{23} & \\dots  & x_{2n} \\\\
    \\vdots & \\vdots & \\vdots & \\ddots & \\vdots \\\\
    x_{d1} & x_{d2} & x_{d3} & \\dots  & x_{dn}
\\end{pmatrix}

Vous pouvez créer une matrice contenant uniquement des caractères ou uniquement des valeurs logiques. Cependant, il est plus courant d'utiliser des matrices contenant des éléments numériques pour les calculs mathématiques.
 
## Créer une matrice {#create-a-matrix}

### Utilisation de la fonction `matrix()`.

Une matrice peut être créée directement en utilisant la fonction `matrix(data, nrow, ncol, byrow, dimnames)`, où

+ *data* est le vecteur d'entrée;
+ *nrow* est le nombre de lignes à créer;
+ *ncol* est le nombre de colonnes à créer;
+ *byrow* est une valeur logique. Si TRUE, alors les éléments du vecteur d'entrée sont disposés ligne par ligne.
+ *dimnames* est le nom attribué aux lignes et aux colonnes.

Dans l'exemple suivant, nous créons d'abord un vecteur avec 12 valeurs que nous fournissons en argument à la fonction `matrix()`. Notez qu'il suffit de donner l'un des deux arguments, `nrow` ou `ncol`, l'autre est déduit par la fonction. 

Pour voir l'effet de l'argument `byrow`, vous pouvez comparer les deux matrices suivantes:


```r
# Les éléments sont disposés en séquence par ligne.
# Note : il n'est pas nécessaire d'entrer tous les arguments
matrix(c(1:12), nrow = 4, byrow = TRUE)
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    4    5    6
## [3,]    7    8    9
## [4,]   10   11   12
```

```r
# Les éléments sont maintenant disposés en séquence par colonne.
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

Sachez que matrix() émettra un avertissement si la longueur du vecteur n'est pas un multiple du nombre de lignes, mais ne produira pas d'erreur. Dans ce cas, la dernière ligne de la matrice sera complétée par les premières composantes du vecteur : il est très peu probable que ce soit la matrice avec laquelle vous vouliez travailler. 

{{% /callout %}}

Regardez ce qui se passe dans l'exemple suivant :


```r
matrix(c(1:14), nrow = 4, byrow = TRUE)
```

```
## Warning in matrix(c(1:14), nrow = 4, byrow = TRUE): data length [14] is not a
## sub-multiple or multiple of the number of rows [4]
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    2    3    4
## [2,]    5    6    7    8
## [3,]    9   10   11   12
## [4,]   13   14    1    2
```

Les noms des lignes et des colonnes doivent apparaître sous la forme d'une liste de deux vecteurs (plus de détails sur les listes plus tard).


```r
# Définissez les noms des colonnes et des lignes.
rownames <- c("row1", "row2", "row3", "row4")
colnames <- c("col1", "col2", "col3")

matrix(c(1:12), nrow = 4, byrow = TRUE, dimnames = list(rownames, colnames))
```

```
##      col1 col2 col3
## row1    1    2    3
## row2    4    5    6
## row3    7    8    9
## row4   10   11   12
```

Enfin, notez que la fonction `matrix()` peut être utilisée pour créer une matrice "vide", c'est-à-dire une matrice remplie de "NA" avec les instructions suivantes :


```r
matrix(, nrow = 2, ncol = 4)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]   NA   NA   NA   NA
## [2,]   NA   NA   NA   NA
```


### Utilisation des fonctions cbind() et rbind()

Vous pouvez également créer une matrice en assemblant (liant) des vecteurs. Les exemples ci-dessous vous montrent différentes façons de créer des matrices avec les fonctions "cbind" et "rbind".  


```r
#créer une matrice à trois colonnes à partir d'un vecteur
rbind(1:3)
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
```

```r
#créer une matrice à 1 colonne à partir d'un vecteur
cbind(1:3)
```

```
##      [,1]
## [1,]    1
## [2,]    2
## [3,]    3
```

```r
# bien noter la différence avec le vecteur 
1:3
```

```
## [1] 1 2 3
```

Vous pouvez créer une matrice rectangulaire, liant plusieurs vecteurs de même taille


```r
cbind(1:3, 4:6, 7:9)
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

{{% callout warning %}}
Notez que les fonctions cbind et rbind peuvent être utilisées pour combiner des matrices. Mais vous devez vous assurer que les matrices aient des dimensions compatibles.

{{% /callout %}}


```r
A <- matrix(1:12, ncol=3) ; A
```

```
##      [,1] [,2] [,3]
## [1,]    1    5    9
## [2,]    2    6   10
## [3,]    3    7   11
## [4,]    4    8   12
```

```r
B <- matrix(1:9, ncol=3) ; B
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

```r
# les matrices A et B ont le même nombre de colonnes
# vous pouvez combiner l'utilisation de rbind()
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
# Mais il y aura une erreur si nous utilisons cbind
cbind(A, B)
```

```
## Error in cbind(A, B): number of rows of matrices must match (see arg 2)
```


### Utilisation de fonctions spécialisées

La fonction `diag(n)` crée une matrice identité de taille n:


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

## Accès aux éléments d'une matrice {#access-elements}

Ce que vous avez appris pour les vecteurs peut être appliqué aux matrices. N'oubliez pas que le premier index est pour les lignes et le second pour les colonnes.


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
ans[1, 3] # renvoie l'élément de la ligne 1 et de la colonne 3
```

```
## [1] 7
```

```r
ans[1,] #remplacera la première ligne
```

```
## [1]  1  4  7 10
```

```r
ans[,2] # renvoie la deuxième colonne
```

```
## [1] 4 5 6
```

```r
ans[, 1:2] # renvoie les deux premières colonnes
```

```
##      [,1] [,2]
## [1,]    1    4
## [2,]    2    5
## [3,]    3    6
```

Vous pouvez également créer une matrice booléenne et l'utiliser pour filtrer les éléments


```r
indices <- ans > 5 #créera une matrice booléenne
indices  
```

```
##       [,1]  [,2] [,3] [,4]
## [1,] FALSE FALSE TRUE TRUE
## [2,] FALSE FALSE TRUE TRUE
## [3,] FALSE  TRUE TRUE TRUE
```

```r
ans [indices] # renvoie un vecteur 
```

```
## [1]  6  7  8  9 10 11 12
```


## Calcul matriciel 

### Multiplication par un scalaire

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

### Addition et Soustraction

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

### Multiplication éléments à éléments

$$  \\begin{pmatrix}
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
\\end{pmatrix} $$


```r
A*B
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    2   20   56  110
## [2,]    6   30   72  132
## [3,]   12   42   90  156
```

### Transposition

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

### Multiplication matricielle

Etant données 2 matrices conformes, la multiplication matricielle utilise l'opérateur `%*%`

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

Cet opérateur permet également la multiplication d'une matrice par un vecteur.

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

## Les fonctions matricielles communes {#fonctions-matricielles-communes}



```r
A <- matrix(seq(2, 8, 2), ncol=2); A
```

```
##      [,1] [,2]
## [1,]    2    6
## [2,]    4    8
```

### Nombre de lignes ou de colonnes


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

### Diagonale de la matrice

```r
diag(A)
```

```
## [1] 2 8
```

{{% callout note %}}

Le lecteur attentif aura remarqué que la fonction `diag()` réagira différemment en fonction des arguments qui lui sont donnés. Pour ceux qui sont familiers avec la programmation objet, cela ne devrait pas être une surprise. Pour les autres, c'est une invitation à la prudence lorsqu'on fournit un argument à une fonction. 


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


### Valeurs et vecteurs propres


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

Notez que `eigen()` fournit une liste. Si vous n'êtes intéressé que par les valeurs propres, utilisez:


```r
eigA <- eigen(A) 
eigA$values
```

```
## [1] 10.7445626 -0.7445626
```

### Inverse 

Si A est une matrice régulière (carrée et inversible), l'inverse est trouvé à l'aide de la fonction `solve()`


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

### Somme des lignes ou somme des colonnes


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
sum(A)  # somme sur tous les éléments
```

```
## [1] 20
```

