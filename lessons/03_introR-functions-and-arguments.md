---
layout: topic
title: Functions in R
author: Data Carpentry contributors
date: "Tuesday, May 9, 2017"
---
Approximate time: 50 min

## Learning Objectives

* Describe and utilize functions in R. * Modify default behavior of functions using arguments in R.* Identify R-specific sources of help to get more information about functions.

## Functions and their arguments

### What are functions?

A key feature of R is functions. Functions are **"self contained" modules of code that accomplish a specific task**. Functions usually take in some sort of data structure (value, vector, dataframe etc.), process it, and return a result.

The general usage for a function is the name of the function followed by parentheses:

```r
function_name(input)
```
The input(s) are called **arguments**, which can include:

1. the physical object (any data structure) on which the function carries out a task 
2. specifications that alter the way the function operates (e.g. options)

Not all functions take arguments, for example:

```r
getwd()
```

However, most functions can take several arguments. If you don't specify a required argument when calling the function, you will either receive an error or the function will fall back on using a *default*. 

The **defaults** represent standard values that the author of the function specified as being "good enough in standard cases". An example would be what symbol to use in a plot. However, if you want something specific, simply change the argument yourself with a value of your choice.

### Basic functions

We have already used a few examples of basic functions in the previous lessons i.e `getwd()`, `c()`, and  `factor()`. These functions are available as part of R's built in capabilities, and we will explore a few more of these base functions below. 

You can also get functions from external [*packages or libraries*](https://github.com/hbc/NGS_Data_Analysis_Course/blob/master/sessionII/lessons/07_introR-functions-and-arguments.md#packages-and-libraries) (which we'll talk about in a bit), or [even write your own](https://campus.datacamp.com/courses/writing-functions-in-r/a-quick-refresher?ex=1). 

Let's revisit a function that we have used previously to combine data `c()` into vectors. The *arguments* it takes is a collection of numbers, characters or strings (separated by a comma). The `c()` function performs the task of combining the numbers or characters into a single vector. You can also use the function to add elements to an existing vector:

```r
glengths <- c(glengths, 90) # adding at the end	
glengths <- c(30, glengths) # adding at the beginning
```

What happens here is that we take the original vector `glengths` (containing three elements), and we are adding another item to either end. We can do this over and over again to build a vector or a dataset.

Since R is used for statistical computing, many of the base functions involve mathematical operations. One example would be the function `sqrt()`. The input/argument must be a number, and the output is the square root of that number. Let's try finding the square root of 81:

```r
sqrt(81)
```

Now what would happen if we **called the function** (e.g. ran the function), on a *vector of values* instead of a single value? 

```r
sqrt(glengths)
```

In this case the task was performed on each individual value of the vector `glengths` and the respective results were displayed.


Let's try another function, this time using one that we can change some of the *options* (arguments that change the behavior of the function), for example `round`:

```r
round(3.14159)
```

We can see that we get `3`. That's because the default is to round to the nearest whole number. **What if we want a different number of significant digits?**


#### Seeking help & Arguments

The best way of finding out this information is to use the `?` followed by the name of the function. Doing this will open up the help manual in the bottom right panel of RStudio that will provide a description of the function, usage, arguments, details, and examples: 

```r
?round
```	
Alternatively, if you are familiar with the function but just need to remind yourself of the names of the arguments, you can use:

```r
args(round)
```

Even more useful is the `example()` function. This will allow you to run the examples section from the Online Help to see exactly how it works when executing the commands. Let's try that for `round()`:

```r
example("round")
```

In our example, we can change the number of digits returned by **adding an argument**. We can type `digits=2` or however many we may want:


```r
round(3.14159, digits=2)
```

> *NOTE:* If you provide the arguments in the exact same order as they are defined (in the help manual) you don't have to name them:
>
	round(3.14159, 2)
>
>However, it's usually not recommended practice because it involves a lot of memorization. In addition, it makes your code difficult to read for your future self and others, especially if your code includes functions that are not commonly used. (It's however OK to not include the names of the arguments for basic functions like `mean`, `min`, etc...). Another advantage of naming arguments, is that the order doesn't matter. This is useful when a function has many arguments. 

**Example of an argument**

```r
df <- data.frame(species, glengths, stringsAsFactors = F)
```

***
**Exercise** 

1. Another commonly used base function is `mean()`. Use this function to calculate an average for the `glengths` vector.

---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson is adapted from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*

