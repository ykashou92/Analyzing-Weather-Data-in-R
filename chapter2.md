---
  title: "Is it cloudy today?"
  description: "This is a template chapter."
  v2: true

---
## Overview: Daily Temperature Bands

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 33e93d2cb0



```

Visualizing our data is extremely important in data science. It can be one of the best tools to determine gaps or identify causes in your projects. Plots are a creative way of answering the questions we pose in our data pipeline.

`@instructions`
Take a look at the plot on the right and the corresponding code. Read through it and don't worry if you don't understand it completely. We will be constructing this plot in this chapter from scratch. When you are ready press submit to move to the first exercise.

`@hint`
Did you read through the code?

`@pre_exercise_code`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
df$day <- factor(df$ts)
t0 <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$day <- format(t0, "%Y-%m-%d")
df_agg <- aggregate(df$temp, by = list(df$day), function(x) {
  c(max = max(x), min = min(x)) })

xdf <- NULL
xdf$day <- df_agg$Group.1
xdf$max <- df_agg$x[, 1]
xdf$min <- df_agg$x[, 2]
xdf$avg <- (xdf$max + xdf$min) / 2
xdf <- data.frame(xdf)

# Visualize the cloud bands 
p <- ggplot(xdf, aes(x = day, y = avg, ymin = min, ymax = max)) +
  geom_line(aes(y = max), color = "firebrick", size = 1, group = 1) +
  geom_line(aes(y = min), color = "steelblue", size = 1, group = 1) +
  geom_pointrange(color = "black", size= 0.75) +
  geom_point(aes(y = max), color = "firebrick", size = 3.5) +
  geom_point(aes(y = min), color = "steelblue", size = 3.5) +
  ylim(0, 50) +
  xlab("Date") +
  ylab("Air Temperature") +
  ggtitle("The cloudier the day, the narrower the band") +
  theme_minimal() +
  theme(axis.text.x=element_text(angle=45)) +
  theme(axis.title.x=element_blank())
p

x <- 0
```
`@sample_code`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)  

# Rename the columns
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset the last 10 days of data (24 hours * 10)
df <- tail(df, 240)

# Convert the 'ts' (timestamp) column into a factor we can work with
df$day <- factor(df$ts)

# Reformat the 'ts' column
t0 <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$day <- format(t0, "%Y-%m-%d")

# Aggregate the produced column to find the minimum and maximum temperatures of each day
df_agg <- aggregate(df$temp, by = list(df$day), function(x) {
  c(max = max(x), min = min(x)) })

# Find the average temperature of each day
xdf <- NULL
xdf$day <- df_agg$Group.1
xdf$max <- df_agg$x[, 1]
xdf$min <- df_agg$x[, 2]
xdf$avg <- (xdf$max + xdf$min) / 2
xdf <- data.frame(xdf)

# Visualize the temperature bands
p <- ggplot(xdf, aes(x = day, y = avg, ymin = min, ymax = max)) +
  geom_line(aes(y = max), color = "firebrick", size = 1, group = 1) +
  geom_line(aes(y = min), color = "steelblue", size = 1, group = 1) +
  geom_pointrange(color = "black", size= 0.75) +
  geom_point(aes(y = max), color = "firebrick", size = 3.5) +
  geom_point(aes(y = min), color = "steelblue", size = 3.5) +
  ylim(0, 50) +
  xlab("Date") +
  ylab("Air Temperature") +
  ggtitle("The cloudier the day, the narrower the band") +
  theme_minimal() +
  theme(axis.text.x=element_text(angle=45)) +
  theme(axis.title.x=element_blank())

# Run the plot
p
```

`@sct`
```{r}
success_msg("Alright! Let's get to it!")
```






---
## Importing the Libraries

```yaml
type: NormalExercise

xp: 100

key: c7a40a5a73



```

To build our plot, we first need to import a few libraries.

One can import a library (for instance: the library **rvest**) like so:  
`library(rvest)`

`@instructions`
The libraries we need are:

- **scales** To change and modify the scaling of the plot  
- **ggplot2** To actually build the plot using a "grammar of graphics"  
- **reshape2** To reshape our data  
- **lubridate** To manipulate data concerning date and time  

Import them like in the example.

`@hint`
You can write each import on a separate line and in any order you wish.


`@sample_code`
```{r}
# This is how we import libraries in R
# library(rvest)

```
`@solution`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
```
`@sct`
```{r}
test_student_typed("library(scales)", not_typed_msg="Did you import `scales`?")
test_student_typed("library(ggplot2)", not_typed_msg="Did you import `ggplot2`?")
test_student_typed("library(reshape2)", not_typed_msg="Did you import `reshape2`?")
test_student_typed("library(lubridate)", not_typed_msg="Did you import `lubridate`?")

#test_function("library", "Did you call the library() function?")
test_error()
success_msg("Awesome! Let's proceed.")
```






---
## Reading the Data

```yaml
type: NormalExercise

xp: 100

key: 768d0c5d2f



```

Now it's time to import our data like we learned in the first chapter.
The libraries have already been imported in the background, but we kept the code visible to you so you can keep track of the whole process.

Our data is hosted on "https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat"

`@instructions`
You will be using two functions to perform this; `url(...)` and `read.delim(file = ..., sep = ..., skip = ...)`

### `url(...)`
1. Use the `url(...)` function on the link above and assign it to a variable called `data`.

### `read.delim(...)`
2. Use the `read.delim(...)` function to read from the variable you stored in the previous step.
3. Don't forget to declare what separates the columns of our data (our file is comma-separated)
4. And lastly, you'd want to skip the first line as it is obscure and unneeded.
5. Assign to a variable called `df`.

`@hint`
Remember than you need to save the output of the `url(...)` function to a variable. And THEN pass it into the `file = ...` argument of `read.delim`.

`@pre_exercise_code`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
```
`@sample_code`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)

# Type your answer on the next line
```
`@solution`
```{r}
# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
```
`@sct`
```{r}
test_object("data", incorrect_msg = "Something is wrong with `data`. Make sure you've assigned the correct value to the variable.")
test_object("df", incorrect_msg = "Something is wrong with `data`. Make sure you've assigned the correct value to the variable.")


test_error()
success_msg("`Nice! You have been following through. Let's continue...")
```






---
## Renaming the Columns

```yaml
type: NormalExercise

xp: 100

key: 290e1b01ca



```



`@instructions`


`@hint`












---
## Choosing our Data

```yaml
type: NormalExercise

xp: 100

key: 400058a13c



```



`@instructions`


`@hint`












---
## Formatting the 'Timestamp' Column

```yaml
type: NormalExercise

xp: 100

key: 88f9897b09



```



`@instructions`


`@hint`












---
## Finding the Minimum and Maximum Temperatures Per Day

```yaml
type: NormalExercise

xp: 100

key: fba56b2895



```



`@instructions`


`@hint`












---
## Finding the Average Temperature Per Day

```yaml
type: NormalExercise

xp: 100

key: a87cae653d



```



`@instructions`


`@hint`












---
## Plotting: The axes

```yaml
type: NormalExercise

xp: 100

key: 3af49d5904



```



`@instructions`


`@hint`












---
## Plotting: The data points

```yaml
type: NormalExercise

xp: 100

key: e578c9252b



```



`@instructions`


`@hint`












---
## Plotting: The lines

```yaml
type: NormalExercise

xp: 100

key: af34272c37



```



`@instructions`


`@hint`












---
## Plotting: The Pointrange

```yaml
type: NormalExercise

xp: 100

key: 1e7e2be896



```



`@instructions`


`@hint`












---
## Plotting: Interactivity with ggplotly

```yaml
type: NormalExercise

xp: 100

key: 01f706ea05



```



`@instructions`


`@hint`










