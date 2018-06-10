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

Visualizing our data is extremely important in data science. It can be one of the best tools to determine gaps or identify causes in your projects. Plots are a creative way of answering the questions we pose in our data pipeline. We will use the plots to gain intuition into the data. In this chapter, we will be plotting temperature bands to determine cloudiness.

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
- **dplyr** To easily select columns from the dataframe

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
library(dplyr)
```
`@sct`
```{r}
test_student_typed("library(scales)", not_typed_msg="Did you import `scales`?")
test_student_typed("library(ggplot2)", not_typed_msg="Did you import `ggplot2`?")
test_student_typed("library(reshape2)", not_typed_msg="Did you import `reshape2`?")
test_student_typed("library(lubridate)", not_typed_msg="Did you import `lubridate`?")
test_student_typed("library(dplyr)", not_typed_msg="Did you import `dplyr`?")

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

Our data is hosted on [https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat](https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat).

`@instructions`
You will be using two functions to perform this; `url(...)` and `read.delim(file = ..., sep = ..., skip = ...)`

### `url(...)`
- Use the `url(...)` function on the link above and assign it to a variable called `data`.

### `read.delim(...)`
- Use the `read.delim(...)` function to read from the variable you stored.
- Don't forget to declare the separator (our file is comma-separated)
- You'd also want to `skip` the _first_ line as it is obscure and unneeded.
- Lastly, assign the output to a variable called `df`.

`@hint`
Remember that you need to save the output of the `url(...)` function to a variable. And THEN pass it into the `file = ...` argument of `read.delim`.

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

# Fill in the blanks!
___ <- ___(___)
___ <- read.___(file = ___, sep = ___, skip = ___)
```
`@solution`
```{r}
# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)
```
`@sct`
```{r}
test_object("data", incorrect_msg = "Something is wrong with `data`. Make sure you included the quotation marks in the link.")
test_object("df", incorrect_msg = "Something is wrong with `df`. Make sure you've used the correct values to the arguments in the function.")

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

We would like to rename the columns into something short and smart. Use the console to take a look at the column names and how we will map them by calling `head(df, 2)`

Original | Mapping | Meaning | Unit
--- | --- | --- | ---
`TS` | `ts`  | Timestamp  | YYYY-MM-DD hh:mm:ss
`RN` |  `rec` | Record number | None
`meters.second` |  `ws` | wind speed | Meters per Second
`Deg`  | `wd` | Wind direction | Degree
`Deg.1` | `wsc`  | Wind direction | None (scaled out of 100)
`kW.m.2` | `srad` | Solar radiation | Kilowatts per Meter Squared
`Deg.C` | `temp` | Temperature | Celsius
`X.` | `rh` | Relative humidity | None (Measured as a Ratio) %
`mm` | `rain`  | Rainfall | Millimeters
`meters` | `vis` | Visibility | Meters (maximum is 75,000)
`mbar` | `bp` | Barometric Pressure | Millibar

`@instructions`
Assign the new column names  instead of the original ones using the `colnames()` function. You can copy and paste them.
`"ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp"`

`@hint`


`@pre_exercise_code`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)  

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

# Copy the following line into the console and press 'Enter'
head(df, 2)

# Write your code here:
# You need to concatenate the strings.
cols <- c(___, ___, ...)

# The function to change the colnames of a dataframe is called `colnames(...)`
___(df) = cols
```
`@solution`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)  

cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df)= cols
```
`@sct`
```{r}
#test_object("cols", incorrect_msg = "Something is wrong with `data`. Make sure you copied the column names correctly.")
test_object("df", incorrect_msg = "Something is wrong with `df`. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("`Nice! We are one step closer. Let's continue...")
```






---
## Choosing our Data

```yaml
type: NormalExercise

xp: 100

key: 400058a13c



```

So we have the data. Now we ask: What parts of these data do we need? Now we are going to visualize temperature over a period of time. Hence we need to select the columns we need and the number of rows we need as well so that we have an aesthetic graph that is easy to read and fast to run.

`@instructions`
Here's what you should do:

- Select the `ts` and `temp` columns from the dataframe and assign it to variable `df` (hence replacing the old one)
- Subset the last 10 days.

`@hint`
The `tail(...)` function can select the last N rows and we are dealing with hourly data. How many rows will represent that last 10-day period?

`@pre_exercise_code`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)  

# Assign those cool columns names
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) <- cols
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

# Assign those cool columns names
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) <- cols

# Use dplyr's select functions
___ <- ___(df, c(___, ___))

# Subset the last 10 days
___ <- tail(df, ___)
```
`@solution`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)  

# Assign those cool columns names
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) <- cols

# Use dplyr's select functions
df <- dplyr::select(df, c(ts, temp))

# Subset the last 10 days
df <- tail(df, 240)
```







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















---
## Finding the Average Temperature Per Day

```yaml
type: NormalExercise

xp: 100

key: a87cae653d



```















---
## Plotting: The axes

```yaml
type: NormalExercise

xp: 100

key: 3af49d5904



```















---
## Plotting: The data points

```yaml
type: NormalExercise

xp: 100

key: e578c9252b



```















---
## Plotting: The lines

```yaml
type: NormalExercise

xp: 100

key: af34272c37



```















---
## Plotting: The Pointrange

```yaml
type: NormalExercise

xp: 100

key: 1e7e2be896



```















---
## Plotting: Interactivity with ggplotly

```yaml
type: NormalExercise

xp: 100

key: 01f706ea05



```













