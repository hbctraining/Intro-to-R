```# Refresher Exercise

# 0. create the data frame called animals
animals <- read.csv("data/animals.csv")

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
