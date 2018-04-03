## The `apply` Function

To obtain mean values for all samples we can use `mean` on each column individually, but there is also an easier way to go about it. Programming languages typically have a way to allow the execution of a single line of code or several lines of code multiple times, or in a "loop". By default R is not very good at looping, hence the `apply()` family of functions are used for this purpose. This family includes several functions, each differing slightly on the inputs or outputs. For example, we can use `apply()` to execute some task on every element in a vector, every row/column in a dataframe, and so on. 

```r
base::apply             Apply Functions Over Array Margins
base::by                Apply a Function to a Data Frame Split by Factors
base::eapply            Apply a Function Over Values in an Environment
base::lapply            Apply a Function over a List or Vector (returns list)
base::sapply            Apply a Function over a List or Vector (returns vector)
base::mapply            Apply a Function to Multiple List or Vector Arguments
base::rapply            Recursively Apply a Function to a List
base::tapply            Apply a Function Over a Ragged Array
```

We will be using `apply` in our examples today, but do take a moment on your own to explore the many options that are available. The `apply` function returns a vector or array or list of values obtained by applying a function to margins of an array or matrix. We know about vectors/arrays and functions, but what are these “margins”? Margins are referring to either the rows (denoted by 1), the columns (denoted by 2) or both (1:2). By “both”, we mean  apply the function to each individual value.

The syntax for the apply function is: 

```r
## DO NOT RUN
apply(dataframe/matrix, margin, function_to_apply)
```

Let's try this to obtain mean expression values for each sample in our RPKM matrix:

```r
samplemeans <- apply(rpkm_ordered, 2, mean) 
```
