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

Dans la plupart des cas, vos données ne sont pas prêtes à être utilisées immédiatement. La plupart du temps, elles comportent des valeurs manquantes, des noms de variables manquants, ou des variables dispersées dans plusieurs colonnes que vous devrez synthétiser. 

Jusqu'à présent, nous avons manipulé des vecteurs, des matrices et des data.frames en les réordonnant et en les prenant des sous-ensembles grâce à l'indexation et à l'opérateur `[ ]`. Mais lorsque nous commencerons des analyses plus avancées, nous voudrons manipuler avec des outils plus efficaces.

{{< figure library="true" src="OIP.jpg" >}}  

Pour manipuler les données, vous avez différentes options. Parmi elles, 3 environnements sont populaires :

+ Le Data.frame original
+ Le paquetage data.table
+ L'écosystème tidyverse

Dans cette section, nous décrivons les manipulations de base possibles avec l'écosystème `tidyverse`


## Le jeu de données

Dans ce chapitre, nous allons travailler avec le jeux de données mis à disposition par le paquet `gapminder`.
Vous devriez maintenant être capable d'installer le paquetage par vous-même (si ce n'est pas le cas, veuillez consulter les premières sections sur la façon d'installer des paquetages sur votre ordinateur).

Chargez le paquetage `gapminder` :
```r
library(gapminder)
```
Si tout s'est bien passé, vous devriez être capable d'accéder directement aux données de gapminder.

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

## Charger les paquetages de l'écosystème tidyverse

Chargeons maintenant les paquetage de l'écosystème `tidyverse`. Nous l'appelons "écosystème" car il est composé de plusieurs paquets. Cependant, vous installez ces différents paquets en utilisant une seule commande:

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

Le message indique les paquetages que vous venez de charger et les conflits potentiels. Notez encore une fois que la couleur rouge ne signifie pas nécessairement que cela n'a pas fonctionné, mais qu'il vous previens des conflits potentiels entre paquetages.

Dans le cas présenté, le paquetage `dplyr` définit une fonction `filter()` qui va masquer la fonction du même nom du paquet `stats`. Si vous voulez vraiment utiliser la fonction filter du paquet stats, il vous faudra mentionner expressément le paquet que vous voulez utiliser en appelant la function comme suit `stats::filter()`.

*Vous êtes maintenant prêt à travailler!*









