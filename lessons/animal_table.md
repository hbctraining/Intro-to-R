**animals**

| | speed | color |
|:----:|:----:|:----:|
| Elephant | 40 | Gray |
| Cheetah | 120 | Tan |
| Tortoise | 0.1 | Green |
| Hare | 48 | Grey |
| Lion | 80 | Tan |
| Polar Bear | 30 | White |

*Source: http://www.speedofanimals.com*

Questions:

1. Extract the `speed` of 40 km/h.
2. Return the rows with `color` of Tan.
3. Return the rows with `speed` greater than 50 km/h and output only the `color` column. Keep the output as a data frame.  
4. Change the color of "Grey" to "Gray". 

[Answer Key](../results/refresher_answerKey.md)

To work with the `animals.csv` file, you can [right-click here](https://raw.githubusercontent.com/hbctraining/Intro-to-R/master/data/animals.csv) and download it **to the `data` folder**. 

Next, read it in using the following: ~`animals <- read.csv("data/animals.csv")`~

```r
animals <- read.csv("data/animals.csv", row.names=1)
```

