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

We have opted for the third one as it provides a consistent framework to work with and is relatively easy to learn.






