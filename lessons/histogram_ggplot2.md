## Histogram

To plot a histogram we require another type of geometric object called `geom_histogram`, which requires a statistical transformation. Some plot types (such as scatterplots) do not require transformations, each point is plotted at x and y coordinates equal to the original value. Other plots, such as boxplots, histograms, prediction lines etc. need to be transformed. Usually these objects have has a default statistic for the transformation, but that can be changed via the `stat_bin` argument. 

Let's plot a histogram of sample mean expression in our data:

```r
ggplot(new_metadata) +
  geom_histogram(aes(x = samplemeans))
```


 ![gghist1](../img/gghist-1.png) 
 

You will notice that even though the histogram is plotted, R gives a warning message ``stat_bin()` using `bins = 30`. Pick better value with `binwidth`.` These are the transformations we discussed. Apparently the default is not good enough. 

Let's change the binwidth values. How does the plot differ?

```r
ggplot(new_metadata) +
  geom_histogram(aes(x = samplemeans), stat = "bin", binwidth=0.8)
```

 ![gghist2](../img/gghist-2.png) 

***

**Exercise**

1. Add an appropriate title to this plot.
2. Make the theme consistent with the scatterplot.

***
