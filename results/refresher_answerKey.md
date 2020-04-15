### Refresher Answer Key

To work with the `animals.csv` file, you can [right-click here](https://raw.githubusercontent.com/hbctraining/Intro-to-R/master/data/animals.csv) and download it to the `data` folder. 

Next, read it in using the following: `animals <- read.csv("data/animals.csv")`

```r
# 1. Extract the speed of 40 km/h. (any of the following lines of code will work)

animals$speed[1]
animals$speed[animals$speed == 40]
animals[1,1]
animals[1,"speed"]
animals["Elephant", "speed"]


# 2. Return the rows with color of Tan. (any of the following lines of code will work)

animals[c(2,5), ]
animals[which(animals$color == "Tan"), ]
animals[animals$color == "Tan",]


# 3. Return the rows with speed greater than 50 km/h and output only the color column. Keep the output as a data frame. (any of the following lines of code will work)

animals[animals$speed > 50, "color", drop =F]
animals[animals$speed > 50, 2, drop=F]
animals[which(animals$speed > 50), "color", drop =F]


# 4. Change the color of “Grey” to “Gray”. (any of the following lines of code will work)

animals[4,2] <- "Gray"
animals[animals$color == "Grey", 2] <- "Gray"
animals[animals$color == "Grey", "color"] <- "Gray"
```



