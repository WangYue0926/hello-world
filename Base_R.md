---
title: "Base R"
author: "Yue Wang"
date: '2018-11-17'
output:
  html_document:
    keep_md: yes
    number_sections: yes
    toc: yes
---

```r
library(MASS)
library(robustbase)
library(insuranceData)
library(car)
```

```
## Loading required package: carData
```

```r
library(corrplot)
```

```
## corrplot 0.84 loaded
```

```r
library(rpart)
library(wordcloud)
```

```
## Loading required package: RColorBrewer
```

```r
library(insuranceData)
library(lattice)
#library(aplpack)
```


# Base R

## Adding details


```r
plot(Cars93$Price,Cars93$Max.Price,pch = 17, col ="red")
```

![](Base_R_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

### `points()` 
- `points()` allows for adding a new set of points to existing scatterplot.

```r
plot(Cars93$Price,Cars93$Max.Price,pch = 17, col ="red")
points(Cars93$Price,Cars93$Min.Price,pch = 16, col ="blue")
```

![](Base_R_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

### `abline()`
- `abline()` allows for adding reference lines. 

```r
plot(Cars93$Price,Cars93$Max.Price,pch = 17, col ="red")
points(Cars93$Price,Cars93$Min.Price,pch = 16, col ="blue")
abline(a = 0, b = 1, lty = 2)
```

![](Base_R_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
linear_model = lm(Gas ~ Temp, data =whiteside)
plot(whiteside$Temp,whiteside$Gas)
abline(linear_model,lty = 2)
```

![](Base_R_files/figure-html/unnamed-chunk-4-2.png)<!-- -->

### `text()`
- `text()` function is to add informative labels to a data plot.

```r
plot(Cars93$Horsepower,Cars93$MPG.city,pch = 15)
index3 = which(Cars93$Cylinders == 3)
text(x = Cars93$Horsepower[index3], 
     y = Cars93$MPG.city[index3],
     labels = Cars93$Make[index3], adj = 0)
```

![](Base_R_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

#### `adj`;`cex`;`font`;`srt`

```r
plot(Cars93$Horsepower,Cars93$MPG.city)
index3 = which(Cars93$Cylinders==3)
points(Cars93$Horsepower[index3],Cars93$MPG.city[index3],pch=16)
text(Cars93$Horsepower[index3],Cars93$MPG.city[index3],label = Cars93$Make[index3],adj = -0.2, cex = 1.2,font=4)
```

![](Base_R_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

```r
plot(whiteside$Temp,whiteside$Gas,pch = 17)
indexB = which(whiteside$Insul =="Before")
indexA = which(whiteside$Insul == "After")
text(x = whiteside$Temp[indexB], y = whiteside$Gas[indexB],
     labels = "Before", col = "blue", srt = 30, cex = 0.8)
text(x = whiteside$Temp[indexA], y = whiteside$Gas[indexA],
     labels = "After", col = "red", srt = -20, cex = 0.8)
```

![](Base_R_files/figure-html/unnamed-chunk-6-2.png)<!-- -->

### legend

```r
plot(whiteside$Temp, whiteside$Gas,
     type = "n", xlab = "Outside temperature",
     ylab = "Heating gas consumption")
indexB <- which(whiteside$Insul == "Before")
indexA <- which(whiteside$Insul == "After")
points(whiteside$Temp[indexB], whiteside$Gas[indexB],pch=17)
points(whiteside$Temp[indexA], whiteside$Gas[indexA])
legend("topright", pch = c(17,1), 
       legend = c("Before", "After"))
```

![](Base_R_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

### custom axes

```r
boxplot(sugars ~ shelf, data = UScereal,
        axes = FALSE)
axis(side = 2)
axis(side = 1, at = c(1, 2, 3))
axis(side = 3, at = c(1, 2, 3),
     labels = c("floor", "middle", "top"))
```

![](Base_R_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

### supsum
- `supsum()` adds a curved trend line that highlights this behavior of the data.
- `bass` argument controls the degree of smoothness in the resulting trend curve. The default value is 0, but specifying larger values (up to a maximum of 10) results in a smoother curve.

```r
plot(Cars93$Horsepower,Cars93$MPG.city)
trend1 = supsmu(Cars93$Horsepower, Cars93$MPG.city)
lines(trend1)
trend2 = supsmu(Cars93$Horsepower,Cars93$MPG.city,bass = 10)
lines(trend2,lty=3,lwd=2)
```

![](Base_R_files/figure-html/unnamed-chunk-9-1.png)<!-- -->



### par
#### Creating multiple plot arrays `mfrow`
- `par(mfrow = c(1,2))` creates a plot array with 1 row and 2 columns.

```r
par(mfrow=c(1,2))
plot(Animals2$brain,Animals2$body)
title("Original representation")
plot(Animals2$brain,Animals2$body, log="xy")
title("Log-log plot")
```

![](Base_R_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

### type
#### Specifying how the plot is drawn `type`
- `"p"` for "points"; `"l"` for "lines"; `"o"` for "overlaid"; `"s"` for "steps".

```r
par(mfrow = c(2,2))
plot(Animals2$brain,type = "p")
title("points")
plot(Animals2$brain,type = "l")
title("lines")
plot(Animals2$brain,type = "o")
title("overlaid")
plot(Animals2$brain,type = "s")
title("steps")
```

![](Base_R_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

## plot type

### Histograms

#### `hist()`
- `hist()` is part of base R and its default option yields a histogram based on the number of times a record falls into each of the bins on which the histogram is based.
- `truehist()` is from the MASS package and scales these counts to give an estimate of the probability density.

```r
par(mfrow = c(1,2))
hist(Cars93$Horsepower, main = "hist() plot")
truehist(Cars93$Horsepower, main = "truehist() plot")
```

![](Base_R_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

### Density plots

#### `lines()` and `density()`
- `lines(density())` adds density on top.

```r
index16 <- which(ChickWeight$Time == 16)
weights <- ChickWeight$weight[index16]
truehist(weights)
lines(density(weights))
```

![](Base_R_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

#### line types
- line types are set by the `lty` argument, with the default value lty = 1 specifying solid lines, lty = 2 specifying dashed lines, and lty = 3 specifying dotted lines.
- the `lwd` argument specifies the relative width

```r
x <- seq(0, 10, length = 200)
gauss1 <- dnorm(x, mean = 2, sd = 0.2)
gauss2 <- dnorm(x, mean = 4, sd = 0.5)
plot(x,gauss1,type = "l",ylab ="Gaussian probability density")
lines(x,gauss2,lty = 2,lwd=3)
```

![](Base_R_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


### QQ-plot
- QQ plot indentifies whether the data is subject to Gaussian distribution.

```r
qqPlot(weights)
```

![](Base_R_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

```
## [1] 32 18
```

```r
qqPlot(Boston$tax)
```

![](Base_R_files/figure-html/unnamed-chunk-15-2.png)<!-- -->

```
## [1] 489 490
```

### sunflowerplot
- In Sunflowerplots, each petal represent each repeat of point.

```r
par(mfrow = c(1,2))
plot(rad ~ zn, data = Boston)
title("Standard scatterplot")
sunflowerplot(rad ~ zn, data = Boston)
title("Sunflower plot")
```

![](Base_R_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

### boxplot
- The `boxplot()` function shows how the distribution of a numerical variable `y` differs across the unique levels of a second variable, `x`. 

#### parameters
- `varwidth` allows for variable-width boxplots that show the different sizes of the data subsets.
- `log` allows for log-transformed y-values.
- `las` allows for more readable axis labels.

```r
boxplot(crim ~ rad, data = Boston, varwidth = T, log = 'y', las =1)
title("Crime rate vs. radial highway index")
```

![](Base_R_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

### mosaicplot
- A mosaic plot may be viewed as a scatterplot between categorical variables and it is supported in R with the `mosaicplot()` function.

```r
mosaicplot(carb ~ cyl, data = mtcars)
```

![](Base_R_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

### bagplot
- A single box plot gives a graphical representation of the range of variation in a numerical variable, based on five numbers:The minimum and maximum values;The median (or "middle") value;Two intermediate values called the lower and upper quartiles


```r
boxplot(Cars93$Min.Price, Cars93$Max.Price)
#bagplot(Cars93$Min.Price, Cars93$Max.Price, cex = 1.2)
abline(a = 0, b = 1, lty = 2)
```

![](Base_R_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

### corrplot

```r
numericalVars=subset(UScereal,select = 2:10)
corrplot(cor(numericalVars),method = "ellipse")
```

![](Base_R_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

### rpart
- `rpart()` function is used to build a decision tree model. 

```r
tree_model = rpart(medv ~. , data = Boston)
plot(tree_model)
text(tree_model,cex = 0.7)
```

![](Base_R_files/figure-html/unnamed-chunk-21-1.png)<!-- -->


## control plot complexity 

### how many plots is too many

```r
keep_vars <- c("calories", "protein", "fat",
               "fibre", "carbo", "sugars")
df <- UScereal[, keep_vars]
par(mfrow=c(2,2))
matplot(UScereal$calories,df[,2:3],ylab='',xlab = "calories")
title("Two scatterplots")
matplot(UScereal$calories,df[,2:4],ylab='',xlab = "calories")
title("Three scatterplots")
matplot(UScereal$calories,df[,2:5],ylab='',xlab = "calories")
title("Four scatterplots")
matplot(UScereal$calories,df[,2:6],ylab='',xlab = "calories")
title("Five scatterplots")
```

![](Base_R_files/figure-html/unnamed-chunk-22-1.png)<!-- -->


### how many words is too many
- `wordcloud()` function generates wordclouds
- The `scale` argument is a two-component numerical vector giving the relative size of the largest word in the display and that of the smallest word. 
- The wordcloud only includes those words that occur at least `min.freq` times in the collection and the default value for this argument is 3


```r
# Create mfr_table of manufacturer frequencies
mfr_table <- table(Cars93$Manufacturer)

# Create the default wordcloud from this table
wordcloud(words = names(mfr_table), 
          freq = as.numeric(mfr_table), 
          scale = c(2,0.25))
```

![](Base_R_files/figure-html/unnamed-chunk-23-1.png)<!-- -->

```r
# Change the minimum word frequency
wordcloud(words = names(mfr_table), 
          freq = as.numeric(mfr_table), 
          scale = c(2,0.25), 
          min.freq = min(mfr_table))
```

![](Base_R_files/figure-html/unnamed-chunk-23-2.png)<!-- -->

```r
# Create model_table of model frequencies
model_table <- table(Cars93$Model)

# Create the wordcloud of all model names with smaller scaling
wordcloud(words = names(model_table), 
          freq = as.numeric(model_table), 
          scale = c(0.75, 0.25), 
          min.freq = min(mfr_table))
```

![](Base_R_files/figure-html/unnamed-chunk-23-3.png)<!-- -->

### The utility of common scaling and individual titles

```r
# Define common x and y limits for the four plots
xmin <- min(anscombe$x1,anscombe$x2,anscombe$x3,anscombe$x4)
xmax <- max(anscombe$x1,anscombe$x2,anscombe$x3,anscombe$x4)
ymin <- min(anscombe$y1,anscombe$y2,anscombe$y3,anscombe$y4)
ymax <- max(anscombe$y1,anscombe$y2,anscombe$y3,anscombe$y4)

# Set up a two-by-two plot array
par(mfrow= c(2,2))

# Plot y1 vs. x1 with common x and y limits, labels & title
plot(anscombe$x1, anscombe$y1,
     xlim = c(xmin, xmax),
     ylim = c(ymin, ymax),
     xlab = "x value", ylab = "y value",
     main = "First dataset")

# Do the same for the y2 vs. x2 plot
plot(anscombe$x2, anscombe$y2,
     xlim = c(xmin, xmax),
     ylim = c(ymin, ymax),
     xlab = "x value", ylab = "y value",
     main = "Second dataset")

# Do the same for the y3 vs. x3 plot
plot(anscombe$x3, anscombe$y3,
     xlim = c(xmin, xmax),
     ylim = c(ymin, ymax),
     xlab = "x value", ylab = "y value",
     main = "Third dataset")

# Do the same for the y4 vs. x4 plot
plot(anscombe$x4, anscombe$y4,
     xlim = c(xmin, xmax),
     ylim = c(ymin, ymax),
     xlab = "x value", ylab = "y value",
     main = "Fourth dataset")
```

![](Base_R_files/figure-html/unnamed-chunk-24-1.png)<!-- -->

### Using multiple plots to give multiple views of a dataset

```r
# Set up a two-by-two plot array
par(mfrow = c(2,2))

# Plot the raw duration data
plot(geyser$duration,main ="Raw data")

# Plot the normalized histogram of the duration data
truehist(geyser$duration,main="Histogram")

# Plot the density of the duration data
plot(density(geyser$duration),main ="Density")

# Construct the normal QQ-plot of the duration data
qqPlot(geyser$duration,main= "QQ-plot")
```

![](Base_R_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

```
## [1] 149  12
```

### layout

```r
# Use the matrix function to create a matrix with three rows and two columns
layoutMatrix <- matrix(
  c(
    0, 1,
    2, 0,
    0, 3
  ), 
  byrow = TRUE, 
  nrow = 3
)
# Call the layout() function to set up the plot array
layout(layoutMatrix)

# Show where the three plots will go 
layout.show(3)
```

![](Base_R_files/figure-html/unnamed-chunk-26-1.png)<!-- -->


```r
# Set up the plot array
layoutMatrix <- matrix( c(
    0, 1,
    2, 0,
    0, 3
  ), byrow = TRUE, 
  nrow = 3)
layout(layoutMatrix)
# Construct vectors indexB and indexA
indexB <- which(whiteside$Insul=="Before")
indexA <- which(whiteside$Insul=="After")
# Create plot 1 and add title
plot(whiteside$Temp[indexB], whiteside$Gas[indexB],
     ylim = c(0,8))
title("Before data only")
# Create plot 2 and add title
plot(whiteside$Temp, whiteside$Gas,
     ylim = c(0,8))
title("Complete dataset")
# Create plot 3 and add title
plot(whiteside$Temp[indexA], whiteside$Gas[indexA],
     ylim = c(0,8))
title("After data only")
```

![](Base_R_files/figure-html/unnamed-chunk-27-1.png)<!-- -->


```r
# Create row1, row2, and layoutVector
row1 <- c(1,0,0)
row2 <- c(0,2,2)
layoutVector <- c(row1,row2,row2)
# Convert layoutVector into layoutMatrix
layoutMatrix <- matrix(layoutVector, byrow = T, nrow = 3)
# Set up the plot array
layout(layoutMatrix)
# Plot scatterplot
plot(Boston$rad,Boston$zn)
# Plot sunflower plot
sunflowerplot(Boston$rad,Boston$zn)
```

![](Base_R_files/figure-html/unnamed-chunk-28-1.png)<!-- -->

### Some plot functions also return useful information
- capture the return values from `barplot()` and use them as the `y` parameter in a subsequent call to the `text()` function to place the text at whatever `x` position but overlaid in the middle of each horizontal bar.

```r
# Create a table of Cylinders frequencies
tbl <- table(Cars93$Cylinders)

# Generate a horizontal barplot of these frequencies
mids <- barplot(tbl, horiz =T,
                col = "transparent",
                names.arg = "")

# Add names labels with text()
text(20, mids, names(tbl))

# Add count labels with text()
text(35, mids, as.numeric(tbl))
```

![](Base_R_files/figure-html/unnamed-chunk-29-1.png)<!-- -->

### Using the `symbols()` function to display relations between more than two variables
-circles argument to create a bubbleplot where each data point is represented by a circle whose radius depends on the third variable specified by the value of this argument.

```r
# Call symbols() to create the default bubbleplot
symbols(Cars93$Horsepower, Cars93$MPG.city,
        circles = sqrt(Cars93$Price))
```

![](Base_R_files/figure-html/unnamed-chunk-30-1.png)<!-- -->

```r
# Repeat, with the inches argument specified
symbols(Cars93$Horsepower, Cars93$MPG.city,
        circles = sqrt(Cars93$Price),
        inches = 0.1)
```

![](Base_R_files/figure-html/unnamed-chunk-30-2.png)<!-- -->

### Saving files

```r
# Call png() with the name of the file we want to create
png("bubbleplot.png")
# Re-create the plot from the last exercise
symbols(Cars93$Horsepower, Cars93$MPG.city,
        circles = sqrt(Cars93$Price),
        inches = 0.1)
# Save our file and return to our interactive session
dev.off()
```

```
## quartz_off_screen 
##                 2
```

```r
# Verify that we have created the file
list.files(pattern = "png")
```

```
## [1] "bubbleplot.png"
```

### Using color to enhance a bubbleplot

```r
# Iliinsky and Steele color name vector
IScolors <- c("red", "green", "yellow", "blue",
              "black", "white", "pink", "cyan",
              "gray", "orange", "brown", "purple")

# Create the data for the barplot
barWidths <- c(rep(2, 6), rep(1, 6))

# Recreate the horizontal barplot with colored bars
barplot(rev(barWidths), horiz = T,
        col = rev(IScolors), axes = F,
        names.arg = rev(IScolors), las = 1)
```

![](Base_R_files/figure-html/unnamed-chunk-32-1.png)<!-- -->


```r
# Iliinsky and Steele color name vector
IScolors <- c("red", "green", "yellow", "blue",
              "black", "white", "pink", "cyan",
              "gray", "orange", "brown", "purple")
# Create the `cylinderLevels` variable
cylinderLevels <- as.numeric(Cars93$Cylinders)
# Create the colored bubbleplot
symbols(Cars93$Horsepower, Cars93$MPG.city, 
        circles = cylinderLevels, inches = 0.2, 
        bg = IScolors[cylinderLevels])
```

![](Base_R_files/figure-html/unnamed-chunk-33-1.png)<!-- -->


```r
# Create a table of Cylinders by Origin
tbl <- table(Cars93$Cylinders, Cars93$Origin)

# Create the default stacked barplot
barplot(tbl)
```

![](Base_R_files/figure-html/unnamed-chunk-34-1.png)<!-- -->

```r
# Enhance this plot with color
barplot(tbl, col = IScolors)
```

![](Base_R_files/figure-html/unnamed-chunk-34-2.png)<!-- -->

### Other graphic systems in R

#### The grid graphics system
- The main function in `tabplot` is `tableplot()`, developed to visualize data distributions and relationships between variables in large datasets.
- Specifically, the `tableplot()` function constructs a set of side-by-side horizontal barplots, one for each variable.

```r
# Load the insuranceData package
library(insuranceData)

# Use the data() function to load the dataCar data frame
data(dataCar)

# Load the tabplot package
suppressPackageStartupMessages(library(tabplot))

# Generate the default tableplot() display
tableplot(dataCar)
```

![](Base_R_files/figure-html/unnamed-chunk-35-1.png)<!-- -->

#### The lattice graphics system
- `lattice` is a general-purpose graphics package that provides alternative implementations of many of the plotting functions available in base graphics. 
- Specific examples include scatterplots with the `xyplot()` function, bar charts with the `barchart()` function, and boxplots with the `bwplot()` function.

```r
# Load the lattice package
library(lattice)
# Construct the formula
calories_vs_sugars_by_shelf <- calories ~ sugars|shelf
# Use xyplot() to draw the conditional scatterplot
xyplot(calories_vs_sugars_by_shelf, data=UScereal)
```

![](Base_R_files/figure-html/unnamed-chunk-36-1.png)<!-- -->

#### The ggplot2 graphic package

```r
# Load the ggplot2 package
library(ggplot2)

# Create the basic plot (not displayed): basePlot
basePlot <- ggplot(Cars93, aes(x = Horsepower, y = MPG.city))

# Display the basic scatterplot
basePlot + 
  geom_point()
```

![](Base_R_files/figure-html/unnamed-chunk-37-1.png)<!-- -->

```r
# Color the points by Cylinders value
basePlot + 
  geom_point(color = IScolors[Cars93$Cylinders])
```

![](Base_R_files/figure-html/unnamed-chunk-37-2.png)<!-- -->

```r
# Make the point sizes also vary with Cylinders value
basePlot + 
  geom_point(color = IScolors[Cars93$Cylinders], 
             size = as.numeric(Cars93$Cylinders))
```

![](Base_R_files/figure-html/unnamed-chunk-37-3.png)<!-- -->

