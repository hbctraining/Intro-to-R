## `ggplot2` code to make boxplot

```
ggplot(new_metadata) +
  geom_boxplot(aes(x = genotype, y=samplemeans, fill = celltype)) +
  ggtitle("Genotype differences in average gene expression") +
  xlab("Genotype") +
  ylab("Mean expression") +
  theme(axis.title=element_text(size=rel(1.5))) +
  theme(plot.title=element_text(size=rel(1.5)))

```
