#### Practice with the `map()` family of functions

The syntax for the `map()` family of functions is: 

```r
## DO NOT RUN
map(object, function_to_apply)
```

For example, let's practice with creating a list called `list_purrr`:

```r
library(purrr)

list_purrr <- list(c(0:10), c(20:30), c(40:50))

list_purrr
```

Now if we wanted to take the median value for each of the components using the `map()` function:

```r
map(list_purrr, median)
```

This will return a **list**. However, if we wanted the output to be returned as a **numeric vector**, we could use the `map_dbl()` function:

```r
map_dbl(list_purrr, median)

```

Or we could return a **character vector**:

```r
map_chr(list_purrr, median)

```

This flexibility of the `map()` family of functions can be really useful. 
