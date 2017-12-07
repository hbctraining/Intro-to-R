---
title: "Data subsetting with base R: dataframes (matrices) and lists"
authors: Meeta Mistry, Mary Piper
date: "Friday, October 20th, 2017"
---
Approximate time: 45 min

## Learning Objectives

* Construct data structures to store external data in R.
* Inspect data structures in R.
* Demonstrate how to subset and create new datasets from existing data structures in R.
* Export data tables and plots for use outside of the R environment.

## Reading data into R
Regardless of the specific analysis in R we are performing, we usually need to bring data in for the analysis. The function in R we use will depend on the type of data file we are bringing in (e.g. text, Stata, SPSS, SAS, Excel, etc.) and how the data in that file are separated, or delimited. The table below lists functions that can be used to import data from common file formats.

| Data type               | Extension | Function          | Package            |
|:------------------------|:----------|:------------------|:-------------------|
| Comma separated values  | csv       | `read.csv()`      | utils (default)    |
|                         |           | `read_csv()`      | readr (tidyverse)  |
| Tab separated values    | tsv       | `read_tsv()`      | readr              |
| Other delimited formats | txt       | `read.table()`    | utils              |
|                         |           | `read_table()`    | readr              |
|                         |           | `read_delim()`    | readr              |
| Stata version 13-14     | dta       | `readdta()`       | haven              |
| Stata version 7-12      | dta       | `read.dta()`      | foreign            |
| SPSS                    | sav       | `read.spss()`     | foreign            |
| SAS                     | sas7bdat  | `read.sas7bdat()` | sas7bdat           |
| Excel                   | xlsx, xls | `read_excel()`    | readxl (tidyverse) |

For example, if we have text file separated by commas (comma-separated values), we could use the function `read.csv`. However, if the data are separated by a different delimiter in a text file, we could use the generic `read.table` function and specify the delimiter as an argument in the function. 

When working with genomic data, we often have a metadata file containing information on each sample in our dataset. Let's bring in the metadata file using the `read.csv` function. Check the arguments for the function to get an idea of the function options:

```r
?read.csv
```

The `read.csv` function has *one required argument* and several *options* that can be specified. The mandatory argument is a path to the file and filename, which in our case is `data/mouse_exp_design.csv`. We will put the function to the right of the assignment operator, meaning that **any output will be saved as the variable name provided on the left**.

```r
metadata <- read.csv(file="data/mouse_exp_design.csv")
```

> *Note: By default, `read.csv` converts (= coerces) columns that contain characters (i.e., text) into the `factor` data type. Depending on what you want to do with the data, you may want to keep these columns as `character`. To do so, `read.csv()` and `read.table()` have an argument called `stringsAsFactors` which can be set to `FALSE`.*
> 


## Inspecting data structures

There are a wide selection of base functions in R that are useful for inspecting your data and summarizing it. Let's use the `metadata` file that we created to test out data inspection functions. 

Take a look at the dataframe by typing out the variable name `metadata` and pressing return; the variable contains information describing the samples in our study. Each row holds information for a single sample, and the columns contain categorical information about the sample `genotype`(WT or KO),  `celltype` (typeA or typeB), and `replicate number` (1,2, or 3).


```r
metadata

          genotype celltype replicate
sample1        Wt    typeA		1
sample2        Wt    typeA		2
sample3        Wt    typeA		3
sample4        KO    typeA		1
sample5        KO    typeA		2
sample6        KO    typeA		3
sample7        Wt    typeB		1
sample8        Wt    typeB		2
sample9        Wt    typeB		3
sample10       KO    typeB		1
sample11       KO    typeB		2
sample12       KO    typeB		3

```

Suppose we had a larger file, we might not want to display all the contents in the console. Instead we could check the top (the first 6 lines) of this `data.frame` using the function `head()`:

```r
head(metadata)
```

Previously, we had mentioned that character values get converted to factors by default using `data.frame`. One way to assess this change would be to use the __`str`__ucture function. You will get specific details on each column:


```r
str(metadata)

'data.frame':	12 obs. of  3 variables:
 $ genotype : Factor w/ 2 levels "KO","Wt": 2 2 2 1 1 1 2 2 2 1 ...
 $ celltype : Factor w/ 2 levels "typeA","typeB": 1 1 1 1 1 1 2 2 2 2 ...
 $ replicate: num  1 2 3 1 2 3 1 2 3 1 ...
```

As you can see, the columns `genotype` and `celltype` are of the `factor` class, whereas the replicate column has been interpreted as integer data type.

__You can also get this information from the "Environment" tab in RStudio.__

### List of functions for data inspection

We already saw how the functions `head()` and `str()` can be useful to check the
content and the structure of a `data.frame`. Here is a non-exhaustive list of
functions to get a sense of the content/structure of data.

* All data structures - content display:
	- **`str()`:** compact display of data contents (env.)
	- **`class()`:** data type (e.g. character, numeric, etc.) of vectors and data structure of dataframes, matrices, and lists.
	- **`summary()`:** detailed display, including descriptive statistics, frequencies
	- **`head()`:** will print the beginning entries for the variable
	- **`tail()`:** will print the end entries for the variable
* Vector and factor variables: 
	- **`length()`:** returns the number of elements in the vector or factor
* Dataframe and matrix variables:
	- **`dim()`:** returns dimensions of the dataset
	- **`nrow()`:** returns the number of rows in the dataset
	- **`ncol()`:** returns the number of columns in the dataset
	- **`rownames()`:** returns the row names in the dataset  
	- **`colnames()`:** returns the column names in the dataset

## Selecting data using indices and sequences

When analyzing data, we often want to **partition the data so that we are only working with selected columns or rows.** A data frame or data matrix is simply a collection of vectors combined together. So let's begin with vectors and how to access different elements, and then extend those concepts to dataframes.

### Vectors

#### Selecting using indices

If we want to extract one or several values from a vector, we must provide one or several indices using square brackets `[ ]` syntax. The **index represents the element number within a vector** (or the compartment number, if you think of the bucket analogy). R indices start at 1. Programming languages like Fortran, MATLAB, and R start counting at 1, because that's what human beings typically do. Languages in the C family (including C++, Java, Perl, and Python) count from 0 because that's simpler for computers to do.

Let's start by creating a vector called age:

```r
age <- c(15, 22, 45, 52, 73, 81)
```

![vector indices](../img/vector-index.png)

Suppose we only wanted the fifth value of this vector, we would use the following syntax:

```r
age[5]
```

If we wanted all values except the fifth value of this vector, we would use the following:

```r
age[-5]
```

If we wanted to select more than one element we would still use the square bracket syntax, but rather than using a single value we would pass in a *vector of several index values*:

```r
idx <- c(3,5,6) # create vector of the elements of interest
age[idx]
```

To select a sequence of continuous values from a vector, we would use `:` which is a special function that creates numeric vectors of integer in increasing or decreasing order. Let's select the *first four values* from age:

```r
age[1:4]
```

Alternatively, if you wanted the reverse could try `4:1` for instance, and see what is returned. 

***
**Exercises** 

1. Create a vector called alphabets with the following alphabets, C, D, X, L, F.
2. Use the associated indices along with `[ ]` to do the following:
	* only display C, D and F
	* display all except X
	* display the alphabets in the opposite order (F, L, X, D, C)

***

#### Selecting using indices with logical operators

We can also use indices with logical operators. Logical operators include greater than (>), less than (<), and equal to (==). A full list of logical operators in R is displayed below:

| Operator | Description |
| :-----------:|:----------------|
| > | greater than |
| >= | greater than or equal to|
| < | less than |
| <= | less than or equal to |
| == | equal to |
| != | not equal to |
| & | and |
| \| |or |

We can use logical expressions to determine whether a particular condition is true or false. For example, let's use our age vector: 
	
```r
age
```

If we wanted to know if each element in our age vector is greater than 50, we could write the following expression:	

```r
age > 50
```

Returned is a vector of logical values the same length as age with TRUE and FALSE values indicating whether each element in the vector is greater than 50.

```r
[1] FALSE FALSE FALSE  TRUE  TRUE  TRUE
```

We can use these logical vectors to select only the elements in a vector with TRUE values at the same position or index as in the logical vector.

Create an index with logical operators to select all values in the `age` vector over 50 **or** `age` less than 18:

```r
idx <- age > 50 | age < 18
	
idx
	
age

age[idx]
```

##### Indexing with logical operators using the `which()` function

While logical expressions will return a vector of TRUE and FALSE  values of the same length, we could use the `which()` function to output the indices where the values are TRUE. Indexing with either method generates the same results, and personal preference determines which method you choose to use. For example:

```r
idx <- which(age > 50 | age < 18)

idx

age[idx]
```

Notice that we get the same results regardless of whether or not we use the `which()`. Also note that while `which()` works the same as the logical expressions for indexing, it can be used for multiple other operations, where it is not interchangeable with logical expressions.

> **Note about *Nesting* functions**:
>
> Instead of creating the `idx` object in the above sections, we could have just place the logical operations and/or functions within the brackets. 
>
> `age[which(age > 50 | age < 18)]` **is identical to** `age[idx]` above.


### Factors

Since factors are special vectors, the same rules for selecting values using indices apply. The elements of the expression factor created previously had the following categories or levels: low, medium, and high. 

Let's extract the values of the factor with high expression, and let's using nesting here:

```r
expression[expression == "high"]    ## This will only return those elements in the factor equal to "high"
```

> **Nesting note**: 
>
> The piece of code above was more efficient with nesting; we used a single step instead of two steps as shown below:
> 
> Step1 (no nesting): `idx <- expression == "high"`
>
> Step2 (no nesting): `expression[idx]`


***
**Exercise**

Extract only those elements in `samplegroup` that are not KO (*nesting the logical operation is optional*).

***

#### Releveling factors

We have briefly talked about factors, but this data type only becomes more intuitive once you've had a chance to work with it.  Let's take a slight detour and learn about how to **order and/or relevel categories within a factor**. 

As we learned earlier, the categories in the `expression` factor were assigned integers alphabetically, with high=1, low=2, medium=3. To view the integer assignments under the hood you can use str:

```r
str(expression)
Factor w/ 3 levels "high","low","medium": 2 1 3 1 2 3 1
```
The unique elements are referred to as "factor levels".

```r
expression[expression > "low"]
```

In the example above, the logical expression to obtain all levels greater than "low" does not work because the factor is unordered, i.e. there is no notation to say that high is greater than medium etc. In fact, the high category is the middle category because of the alphabetical order of the factor names. 

To order factor levels, you can add an argument to the `factor()` function, `ordered=TRUE`:

```r
expression <- factor(expression, ordered=TRUE)    ## Note that the `factor()` function is used to create a factor, & to modify the characteristics of an existing factor

str(expression)
Ord.factor w/ 3 levels "high"<"low"<"medium": 2 1 3 1 2 3 1

expression[expression > "low"]     ## Now that this factor is ordered what do you expect the output of this line of code to be?
```

Now the output of the `str()` function states that this is an `Ord.factor`, and there are "<" signs to denote that low is the lowest category. 

However, the order of categories is still incorrect and this is even more obvious now with the "<" notation. This is because R is still ordering the levels alphabetically. R does not consider the meaning of the words here, so we have to coerce the **releveling** such that `"low" < "medium" < "high"` using the `levels` option within the `factor()` function.

```r
expression <- factor(expression, levels=c("low", "medium", "high"))      ## note the nested combine function
	
str(expression)
Ord.factor w/ 3 levels "low"<"medium"<..: 1 3 2 3 1 2 3

expression[expression > "low"]
```

> **Note**: 
>
> The `ordered` and `levels` options within `factor()` are *independent of each other*, and can be used separately or together. 


***
**Exercise**

Use the `samplegroup` vector we created in a previous lesson, and change that to an ordered factor such that KO < CTL < OE. 

***

### Dataframes

Subsetting data frames is very similar to subsetting vectors, except that data frames (and matrices) have 2 dimensions (rows and columns). Therefore, if we want to select specific values from it we need to specify the corresponding "coordinates". We use the same square bracket notation but rather than providing a single index, there are *two indices required*. Within the square bracket, **row numbers come first followed by column numbers (and the two are separated by a comma)**. Let's explore the `metadata` dataframe, shown below are the first six samples:

![metadata](../img/metadata.png)

For example:

```r
metadata[1, 1]   # element from the first row in the first column of the data frame
metadata[1, 3]   # element from the first row in the 3rd column
```

Now if you only wanted to select based on rows, you would provide the index for the rows and leave the columns index blank. The key here is to include the comma, to let R know that you are accessing a 2-dimensional data structure:

```r
metadata[3, ]    # vector containing all elements in the 3rd row
```

If you were selecting specific columns from the data frame - the rows are left blank:

```r
metadata[ , 3]    # vector containing all elements in the 3rd column
```

Just like with vectors, you can select multiple rows and columns at a time. Within the square brackets, you need to provide a vector of the desired values:	

```r
metadata[ , 1:2] # dataframe containing first two columns
metadata[c(1,3,6), ] # dataframe containing first, third and sixth rows
```

For larger datasets, it can be tricky to remember the column number that corresponds to a particular variable. (Is celltype in column 1
or 2? oh, right... they are in column 1). In some cases, the column number for a variable can change if the script you are using adds or removes columns. It's therefore often better to use column names to refer to a particular variable, and it makes your code easier to read and your intentions clearer.

You can subset using both column names and row names. You can use `colnames(metadata)` or `names(metadata)` to remind yourself of the column names and `rownames(metadata)` for the row names. 

>**NOTE:** If selecting more than a single row or column, the names need to be combined using the `c()` function.

```r
metadata[c("sample1", "sample4", "sample5") , "celltype"] # elements of the celltype column corresponding to the first, fourth, and fifth samples
```

There is a shortcut for extracting a single column, which is the `$` sign notation (`df_name$column_name`). Extracting a single column results in a vector. For instance, to extract all the genotypes from our dataset, we can use: 

```r
metadata$genotype 
```

We can then supply index values to select specific values from that vector. For example, if we wanted the genotype information for the first five samples in `metadata`:

```r
metadata$genotype[1:5]
```

#### Selecting using indices with logical operators

With dataframes, similar to vectors, we can use logical vectors for specific columns in the dataframe to select only the rows in a dataframe with TRUE values at the same position or index as in the logical vector. We can then use the logical vector to return all of the rows in a dataframe where those values are TRUE.

```r
idx <- metadata$celltype == "typeA"
	
metadata[metadata$celltype == "typeA", ]
```

##### Selecting indices with logical operators using the `which()` function
As you might have guessed, we can also use the `which()` function to return the indices for which the logical expression is TRUE. For example, we can find the indices where the `celltype` is `typeA` within the `metadata` dataframe:

```r
idx <- which(metadata$celltype == "typeA")
	
metadata[idx, ]
```

Or we could find the indices for the metadata replicates 2 and 3:

```r
idx <- which(metadata$replicate > 1)
	
metadata[idx, ]
```

***

**Exercise**  

Subset the `metadata` dataframe to return only the rows of data with a genotype of `KO`.
	
***


### Lists

Selecting components from a list requires a slightly different notation, even though in theory a list is a vector (that contains multiple data structures). To select a specific component of a list, you need to use double bracket notation `[[]]`. Let's use the `list1` that we created previously, and index the second component:

```r
list1[[2]]
```

What do you see printed to the console? Using the double bracket notation is useful for **accessing the individual components whilst preserving the original data structure.** When creating this list we know we had originally stored a dataframe in the second component. With the `class` function we can check if that is what we retrieve:

```r
comp2 <- list1[[2]]
class(comp2)
```

You can also reference what is inside the component by adding and additional bracket. For example, in the first component we have a vector stored. 

```r
list1[[1]]
	
[1] "ecoli" "human" "corn" 
```

It is important to note that when selecting components we can only **access one at a time**. To access multiple components of a list, see the note below. 

> Note: Using the single bracket notation also works wth lists. The difference is the class of the information that is retrieved. Using single bracket notation i.e. `list1[1]` will return the contents in a list form and *not the original data structure*. The benefit of this notation is that it allows indexing by vectors so you can access multiple components of the list at once.

#### Naming list components
Assigning names to the components in a list can help identify what each list component contains, as well as, facilitating the extraction of values from list components. 

Adding names to components of a list uses the same function as adding names to the columns of a dataframe, `names()`.
	
Let's check and see if the `list1` has names for the components:

```r
names(list1) 
```

When we created the list we had combined the `species` vector with  a dataframe `df` and the `number` variable. Let's assign the original names to the components:

```r
names(list1) <- c("species", "df", "number")
	
names(list1)
```

Now that we have named our list components, we can extract components using the `$` similar to extracting columns from a dataframe. To obtain a component of a list using the component name, use `list_name$component_name`:

To extract the `df` dataframe from the `list1` list:

```r
list1$df
```

Now we have three ways that we could extract a component from a list. Let's extract the `species` vector from `list1`:

```r
list1[[1]]
list1[["species"]]
list1$species
```

***

**Exercise**

Let's practice combining ways to extract data from lists:

1. Create a list named `random` with the following components: `metadata`, `age`, `list1`, `samplegroup`, and `number`.
2. Print out the values stored in the `samplegroup` component.
3. Set names for the `random` list you created in the last exercise.

***

### Writing to file 

Everything we have done so far has only modified the data in R; the files have remained unchanged. Whenever we want to save our datasets to file, we need to use a `write` function in R. 

To write our matrix to file in comma separated format (.csv), we can use the `write.csv` function. There are two required arguments: the variable name of the data structure you are exporting, and the path and filename that you are exporting to. By default the delimiter is set, and columns will be separated by a comma:

```r
write.csv(sub_meta, file="data/subset_meta.csv")
```

Similar to reading in data, there are a wide variety of functions available allowing you to export data in specific formats. Another commonly used function is `write.table`, which allows you to specify the delimiter you wish to use. This function is commonly used to create tab-delimited files.

> **NOTE:**
>  
> Sometimes when writing a dataframe with row names to file, the column names will align starting with the row names column. To avoid this, you can include the argument `col.names = NA` when writing to file to ensure all of the column names line up with the correct column values.

> Writing a vector of values to file requires a different function than the functions available for writing dataframes. You can use `write()` to save a vector of values to file. For example:
>
>
>```r
>write(glengths, file="data/genome_lengths.txt", ncolumns=1)
>```
>

---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson is adapted from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
