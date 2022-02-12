---
# Data Structures
linktitle: "Chapter 2: Data structures"
summary: Understand the data objects that you can manipulate with R
weight: 2
icon: book
icon_pack: fas

# Page metadata.
title: "Data types and structures"
date: "2022-02-12T00:00:00Z"
type: book  
---

We have seen that R works with objects which are characterized by their *name* and their *content*. However, these objects have more attributes which specify the kind of data stored in that object. 

In order to understand the usefulness of these attributes, consider a variable that takes the value 1, 2, or 3: such a variable
could be an integer variable (for instance, the number of kids in a family), or the coding of a categorical variable (for instance, the gender of the respondent: male, female, other, did not declare). 

R will not process these variables in the same way: with R, the attributes of the object give the necessary information for the functions to use the correct process. 

All objects have two intrinsic attributes: *mode* and *length*. 

The mode is the basic type of the elements of the object; there are four main modes:

+ numeric
+ character
+ logical (FALSE or TRUE). 
+ complex

During this course, we will not work with complex numbers, so we haven't included any information about this mode.
Besides, other modes exist but they do not represent data, for instance function or expression, and we are not looking at these modes here.

In this section, you will learn more about 

1. What are the {{% staticref "get_ready/data_structures/basic-data-types/" %}}basic modes{{% /staticref %}} in R

2. How to combine data into: 
  + {{% staticref "get_ready/data_structures/vectors/" %}}Vectors{{% /staticref %}} or {{% staticref "get_ready/data_structures/matrices/" %}}Matrices{{% /staticref %}} when data are of the same types
  + {{% staticref "get_ready/data_structures/lists/" %}}Lists{{% /staticref %}} or {{% staticref "get_ready/data_structures/r-data-frames/" %}}Data frames{{% /staticref %}} when data are of different types

3. How to [import data]({{<relref path="2019-02-15-import-data">}} "Page Import Data")  you have stored in other formats, especially text or excel formats into such structures

You can work on the following tutorial for hands on exercises related to this section:
<a href=" https://ecoo.shinyapps.io/Work_with_R_structures/" target="_blank">Proceed to the tutorial</a>
  
{{< figure src="datastructures.png" >}}  
<a href="https://hackernoon.com/50-data-structure-and-algorithms-interview-questions-for-programmers-b4b1ac61f5b0" target="_blank">Image source</a>




