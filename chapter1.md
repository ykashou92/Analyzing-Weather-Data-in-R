---
title: 'Is it cloudy today?'
description: 'We can figure out if it is cloudy or not by visualizing our data.'
---

## Overview: Daily Temperature Bands

```yaml
type: NormalExercise
key: 33e93d2cb0
lang: r
xp: 100
skills: 1
```

Visualizing data is extremely important in data science. It can be one of the best tools to determine gaps or identify causes in your projects. Plots are a creative way of answering the questions you pose in our data pipeline. You will use the plots to gain intuition into the data. In this chapter, you will be plotting temperature bands to determine cloudiness.

The cloudier the day, the narrower the temperature range. This is because clouds prevent heat from entering the atmosphere and also cause heat to be trapped on Earth, thus decreasing the amount of energy Earth receives from the Sun and the amount of energy  that leaves Earth. 

And vice versa...

`@instructions`
Take a look at the plot on the right and the corresponding code. Read through it and don't worry if you don't understand it completely. You will be constructing this plot step-by-step from scratch. When you are ready, press submit to move to the first exercise.

`@hint`
Did you read through the code?

`@pre_exercise_code`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Reformat the 'ts' column
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols

# Visualize the cloud bands
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
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
  theme(axis.title.x=element_blank()) + 
	# This simply centers the title.
	theme(plot.title = element_text(hjust = 0.5))
p
```

`@sample_code`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Reformat the 'ts' column
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols

# Visualize the cloud bands
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
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
  theme(axis.title.x=element_blank()) +
  # This simply centers the title.
  theme(plot.title = element_text(hjust = 0.5))
p
```

`@solution`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Reformat the 'ts' column
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols

# Visualize the cloud bands
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
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
  theme(axis.title.x=element_blank()) +
  # This simply centers the title.
  theme(plot.title = element_text(hjust = 0.5))
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
key: c7a40a5a73
lang: r
xp: 100
skills: 1
```

To build our plot, you first need to import a few libraries:

- **scales** To change and modify the scaling of the plot  
- **ggplot2** To actually build the plot using a "grammar of graphics"  
- **reshape2** To reshape our data  
- **lubridate** To manipulate data concerning date and time  
- **dplyr** To easily select columns from the dataframe

`@instructions`
You can import a library (for instance: the library **rvest**) like so:  
`library(rvest)`

Import `scales`,  `ggplot2`, `reshape2`, `lubridate` and `dplyr` like the example above.

`@hint`
You can write each import on a separate line and in any order you wish.

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
# This is how you import libraries in R
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
key: 768d0c5d2f
lang: r
xp: 100
skills: 1
```

Now it's time to import your data.
The libraries have already been imported in the background, but we kept the code invisible to you so you can keep focus on the current task.

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
library(dplyr)
```

`@sample_code`
```{r}
# Fill in the blanks!
___ <- ___("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
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
key: 290e1b01ca
lang: r
xp: 100
skills: 1
```

We would like to rename the columns into something short and smart. Use the console to take a look at the column names and how you will map them by calling `head(df, 2)`

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
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)
```

`@sample_code`
```{r}
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
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df)= cols
```

`@sct`
```{r}
#test_object("cols", incorrect_msg = "Something is wrong with `data`. Make sure you copied the column names correctly.")
test_object("df", incorrect_msg = "Something is wrong with `df`. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("`Nice! You are one step closer. Let's continue...")
```

---

## Choosing our Data

```yaml
type: NormalExercise
key: 400058a13c
lang: r
xp: 100
skills: 1
```

So you have the data. Now you can ask: 'What parts of these data do I need?'. In this case you are going to visualize temperature over a period of time. Hence you should only select the columns you need and the number of rows you wish to represent such that you create an aesthetically pleasing graph that is easy to read and fast to run.

`@instructions`
You can use functions from a specific library, especially if another function with the same name exists in another library (in this case, they both will work.)
`dplyr::select()` instead of `select()`.

Here's what you should do:

- Subset the last 10 days (not rows) and assign it back to the same variable `df` (hence replacing the old one).
- Select the `ts` and `temp` columns from the dataframe and assign it to variable `df` (hence replacing the old one).
- Verify that it worked with by calling the `str()` function on our dataframe.

`@hint`
The `tail(...)` function can select the last N rows and you are dealing with hourly data. How many rows will represent that last 10-day period?

`@pre_exercise_code`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)  

# Assign those cool columns names
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) <- cols
```

`@sample_code`
```{r}
# Subset the last 10 days
___ <- tail(df, ___)

# Use dplyr's select function
___ <- ___::___(df, c(___, ___))

# Verify with str()
str(___)
```

`@solution`
```{r}
# Subset the last 10 days
df <- tail(df, 240)

# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Verify with str()
str(df)
```

`@sct`
```{r}
test_object("df", incorrect_msg = "Something is wrong with `df`. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("`Nice! You have been following through. Let's continue...")
```

---

## Formatting the 'Timestamp' Column

```yaml
type: NormalExercise
key: 88f9897b09
lang: r
xp: 100
skills: 1
```

Time data can take on various forms, you are concerned specifically with the **POSIXct** data type, which is _**the number of seconds since the start of January 1st, 1970**_.   
  
You will format our `ts` column in two steps:

1. Convert it from a **factor** (category) object to a **list** of *date* objects, specifically POSIXct.
2. Convert it to a **character** object by formatting the column to represent only year-month-day values without hours, minutes or seconds.

You can find a list of the date and time abbreviation symbols [here](https://stat.ethz.ch/R-manual/R-devel/library/base/html/strptime.html):

`@instructions`
- Use the `strptime(...)` function to convert the `ts` column from a **factor** to a **date (POSIXct)**.  You need to specify the format of the date `Year-Month-Day Hour:Minute:Second`
- Use the `format(...)` function to convert the column from a `Year-Month-Day Hour:Minute:Second` **list** object to a `Year-Month-Day` **character** object.

You have written `str(df$ts)` to run on every step such that you may keep track of the changes of the date's format before and after each conversion and gain better intuition.

`@hint`
Both the `strptime()` and `format()` functions require two arguments: __a character vector to be converted__ and __a character sting specifying the format__. Format is a generic function that can reformat any string while strptime works specifically with dates and times.

`@pre_exercise_code`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)  

# Assign those cool columns names
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) <- cols

# Subset the last 10 days
df <- tail(df, 240)

# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))
```

`@sample_code`
```{r}
# You can select a specific column from the dataframe by using $ sign. 
# df$ts selects the ts column, in each step will with replace the column with its result

# Verify
str(df$ts)

# Step 1: Use strptime here
df$ts <- ___(___, "%Y-%m-%d %H:%M:%S")

# Verify
str(df$ts)

# Step 2: Use the format function correctly
df$ts <- format(df$ts, ___)

# Verify
str(df$ts)
```

`@solution`
```{r}
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")
```

`@sct`
```{r}
test_object("df", incorrect_msg = "Something is wrong with the timestamp column `ts`. Make sure you've used the correct values to the arguments during conversion.")

test_error()
success_msg("`Good work! Let's proceed...")
```

---

## Finding the Minimum, Maximum and Average Temperature Per Day

```yaml
type: NormalExercise
key: fba56b2895
lang: r
xp: 100
skills: 1
```

So that's all interesting, but wouldn't it be cooler to find the minimum, maximum and average temperature in a single line of code?

 Type `?aggregate` in the console and read through the documentation, observe the very first example:
`aggregate(state.x77, list(Region = state.region), mean)`

Basically, in the example, the aggregate function takes **an R object**, a **list of grouping elements** and a **function to compute**. 
In this particular case they calculate the **`mean()`** of the **`state.x77`** data frame, grouped by (**`state.region`**)

`@instructions`
Use the aggregate function in the format `aggregate(x, by, func, ...) ` to calculate the `min()`, `max()`, and `mean()` values of the `temp` column of the data frame `df` grouped by the `ts` column of the same data frame and store it in a variable called `xdf`.

Afterwards, take a look at the data frame when it is printed out in the console.

`@hint`
You can select the columns using the `$` symbol.

`@pre_exercise_code`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)  

# Assign those cool columns names
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) <- cols

# Subset the last 10 days
df <- tail(df, 240)

# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")
```

`@sample_code`
```{r}
# Don't fret! It's simpler than you might think.
# The three calculations are just concatenated together.
xdf <- ___(df$___, by = list(df$___), function(x) {
  c(max = ___(x), min = ___(x), avg = ___(x))})

print(xdf)
```

`@solution`
```{r}
xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x))})
```

`@sct`
```{r}
test_object("xdf", incorrect_msg = "Hmm, might want to recheck your code...")

test_error()
success_msg("`Awesome! It's nearly plotting time!")
```

---

## Reformatting the aggregated data with `cbind`

```yaml
type: NormalExercise
key: a87cae653d
lang: r
xp: 100
skills: 1
```

This is our final step before visualizing our data. The aggregated data frame `xdf` actually needs to be reshaped. See it appears like a normal data frame, but it the mean, minimum and maximum columns are actually nested within the timestamp, instead of alongside it. The data frame you now have is a __multi-index__ data frame.

Try the commands in the coding section to better understand the data.

`@instructions`
Use `cbind()` to bind the first column of `xdf`, to the three sub-columns contained in the second column of `xdf`:
- Run the first four `print()` commands and try to understand the structure of the data frame.
- `cbind()` the correct columns and assign it to the same variable `xdf`.

`@hint`
Remember that you can subset using square bracket notation. `df[5]` selects the **5th** column of the data frame.

`@pre_exercise_code`
```{r}
# Import Libraries
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)  

# Assign those cool columns names
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) <- cols

# Subset the last 10 days
df <- tail(df, 240)

# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })
```

`@sample_code`
```{r}
# Number of 'actual' columns in data frame (what you want is 4 - ts, max, min, avg)
print(paste("Number of columns in xdf:",ncol(xdf)))

# Since ncol is 2, you cannot choose any columns other than 1 and 2.
# Let's subset column 1 and view it
print(paste("The 1st column of xdf:", xdf[1]))  

# Let's subset column 2 and view it
print(paste("The 2nd column of xdf:", xdf[2]))  

# But column 2 actually consists of 3 columns. 
# Subsetting will take one extra bracket.
print(paste("Looking deeper into the 2nd column of xdf:", xdf[[2]])

# So you will `cbind` or column bind the first column, with the other three.
xdf <- cbind(xdf[___], xdf[[___]])

# Let's call ncol again
ncol(xdf)

# Finally let's reassign the column names
cols <- c("ts", "max", "min", "avg")
colnames(xdf) <- cols
```

`@solution`
```{r}
# So you will `cbind` or column bind the first column, with the other three.
xdf <- cbind(xdf[1], xdf[[2]])

# Finally let's reassign the column names
cols <- c("ts", "max", "min", "avg")
colnames(xdf) <- cols
```

`@sct`
```{r}
test_object("xdf", incorrect_msg = "Hmm, might want to recheck your code...")

test_error()
success_msg("`Awesome! It's nearly plotting time!")
```

---

## Plotting: The Aesthetics

```yaml
type: NormalExercise
key: 3af49d5904
lang: r
xp: 100
skills: 1
```

Now that you have a nice, clean data frame `xdf` ready...Let's begin by plotting the axes. In the `ggplot()` function, you first specify the data source then the aesthetics of the plot, i.e. the data points involved that would be on the **x** and **y** axes.

Note that you do not have to use the column selector `$` in the `aes` argument of `ggplot()`. Instead you can simply type `ts` to reference the `ts` column of the data frame `df` if `df` is given as the data argument.

`ggplot(df, aes(x = xcolumn, y = ycolumn)` as opposed to `ggplot(df, aes(x = df$xcolumn, y = df$ycolumn)`

`@instructions`
Use `ggplot()` to plot the data, where `x`, `y`, `ymin`, and `ymax` will correspond to the `ts`, `avg`, `min` and `max` columns of the data frame `xdf` and save it to a variable `p`, here's an example:  
`p <- ggplot(df, aes(x = time, y = temperature)`

`@hint`
The best hint is the documentation. Try  the `?ggplot` command in the console!

`@pre_exercise_code`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Reformat the 'ts' column
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols
```

`@sample_code`
```{r}
# Use the ggplot() function
# Select the data
# Define the aes() argument
# Assign it to variable `p`
p <- ___(___, ___(x = ts, y = ___, ymin = min, ymax = ___))

# Writing p by itself will view the plot when the code is run
p
```

`@solution`
```{r}
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max))
```

`@sct`
```{r}
test_student_typed("p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max))", not_typed_msg = "Something is wrong with your expression. Take another look at the instructions.")

test_error()
success_msg("Our plot is in the making! Keep going!")
```

---

## Plotting: The data points

```yaml
type: NormalExercise
key: e578c9252b
lang: r
xp: 100
skills: 1
```

When you save a plot to a variable, it gives you power to add `layers` on the plot. This is simply given by the addition symbol `+`. Check the sample code.
The `geom_point()` function is how you plot **points**, the `geom_line()` function is how you plot **lines**. They both can take a large number of arguments that change how they appear on the graph.

`@instructions`
Use `geom_point()` to:  

- Plot the points of the maximum occurring temperature, with a color of  `firebrick` and a size of `3.5`.
- Plot the points of the minimum occurring temperature, with a color of  `steelblue` and a size of `3.5`.

`@hint`
You've gone this far, maybe the documentation should help for now on :-) 
`?geom_point`

`@pre_exercise_code`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Reformat the 'ts' column
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols

p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max))
```

`@sample_code`
```{r}
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
	___(aes(y = ___), color = '___', size = ___) +
	___(aes(y = min), color = '___', size = ___)

p
```

`@solution`
```{r}
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
  geom_point(aes(y = max), color = "firebrick", size = 3.5) +
  geom_point(aes(y = min), color = "steelblue", size = 3.5)
```

`@sct`
```{r}
test_ggplot(data_fail_msg = "Did you use the `filtered_6_countries` as the `data` argument to your `ggplot()` call?",
            aes_fail_msg = "Something's wrong in the `aes()` layer of your `ggplot()` call. Did you plot `year` on the x-axis and `percent_yes` on the y-axis?",
            geom_fail_msg = "Did you add a `geom_line` layer with `geom_line()` to your call to `ggplot()`?",
            facet_fail_msg = "Did you add a facet layer with `facet_wrap()`? Make sure to use the correct syntax: `~ country`.")
test_error()
success_msg("Our plot is in the making! Keep going!")
```

---

## Plotting: The lines

```yaml
type: NormalExercise
key: af34272c37
lang: r
xp: 100
skills: 1
```

Time to plot the lines with `geom_line()`!

You will do the same as in the previous lesson, except with one difference you will add an extra argument `group = 1` to each of them to connect the lines together.

`@instructions`
Use `geom_line()` to:
- Plot the points of the maximum occurring temperature, with a color of  `firebrick`, a size of `1` and a group of `1`.
- Plot the points of the minimum occurring temperature, with a color of  `steelblue`, a size of `1`and a group of `1`.

`@hint`
Use `?geom_line`!

`@pre_exercise_code`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Reformat the 'ts' column
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols
```

`@sample_code`
```{r}
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
 	# The points
	geom_point(aes(y = max), color = "firebrick", size = 3.5) +
	geom_point(aes(y = min), color = "steelblue", size = 3.5) + 
	# The lines
  	___(aes(y = ___), color = "firebrick", size = ___, group = ___) + 
  	___(___(y = max), color = "___", size = 1, group = ___)
      
p
```

`@solution`
```{r}
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
 	geom_point(aes(y = max), color = "firebrick", size = 3.5) +
 	geom_point(aes(y = min), color = "steelblue", size = 3.5) + 
 	geom_line(aes(y = max), color = "firebrick", size = 1, group = 1) + 
 	geom_line(aes(y = min), color = "steelblue", size = 1, group = 1)
```

`@sct`
```{r}
test_ggplot(data_fail_msg = "Did you use the `filtered_6_countries` as the `data` argument to your `ggplot()` call?",
            aes_fail_msg = "Something's wrong in the `aes()` layer of your `ggplot()` call. Did you plot `year` on the x-axis and `percent_yes` on the y-axis?",
            geom_fail_msg = "Did you add a `geom_line` layer with `geom_line()` to your call to `ggplot()`?",
            facet_fail_msg = "Did you add a facet layer with `facet_wrap()`? Make sure to use the correct syntax: `~ country`.")
test_error()
success_msg("Cool! You're killing it! A couple more things and you are done!")
```

---

## Plotting: The Pointrange

```yaml
type: NormalExercise
key: 1e7e2be896
lang: r
xp: 100
skills: 1
```

What about creating a range? A line that can show the bandwidth of the day's minimum, maximum. This can be a direct indicator of _**cloudiness**_!

`@instructions`
Add a `geom_pointrange()` phrase to the plot. You do not need to specify any data, just set the _color_ to **black** and _size_ to **.75**.

`@hint`


`@pre_exercise_code`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Reformat the 'ts' column
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols
```

`@sample_code`
```{r}
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
 	# The points
	geom_point(aes(y = max), color = "firebrick", size = 3.5) +
	geom_point(aes(y = min), color = "steelblue", size = 3.5) + 
	# The lines
  	geom_line(aes(y = max), color = "firebrick", size = 1, group = 1) + 
  	geom_line(aes(y = min), color = "steelblue", size = 1, group = 1) +
	# The point range
    ___(___ = "___", ___ = 0.75)
p
```

`@solution`
```{r}
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
 	# The points
	geom_point(aes(y = max), color = "firebrick", size = 3.5) +
	geom_point(aes(y = min), color = "steelblue", size = 3.5) + 
	# The lines
  	geom_line(aes(y = max), color = "firebrick", size = 1, group = 1) + 
  	geom_line(aes(y = min), color = "steelblue", size = 1, group = 1) +
	# The point range
    geom_pointrange(color = "black", size = 0.75)
p
```

`@sct`
```{r}
test_ggplot(data_fail_msg = "Did you use the `filtered_6_countries` as the `data` argument to your `ggplot()` call?",
            aes_fail_msg = "Something's wrong in the `aes()` layer of your `ggplot()` call. Did you plot `year` on the x-axis and `percent_yes` on the y-axis?",
            geom_fail_msg = "Did you add a `geom_line` layer with `geom_line()` to your call to `ggplot()`?",
            facet_fail_msg = "Did you add a facet layer with `facet_wrap()`? Make sure to use the correct syntax: `~ country`.")
test_error()
success_msg("Cool! You're killing it! A couple more things and you are done!")
```

---

## Plotting: Axes and Theming

```yaml
type: NormalExercise
key: 01f706ea05
lang: r
xp: 100
skills: 1
```

Hmm, the last plot did not look that great, did it? Here's what you are going to do:

1. You will give the plot a title.
2. You will label the `x` and `y` axes so you know what you are looking at.
3. You will change the scale of `y` axis, so the plot does not appear stretched from top to bottom.
4. You will change the theme of the plot.
5. Finally, you will tilt the dates on the `x` axis so they become readable.

`@instructions`
Alright! Let's test your intuition:

1. Use` ggtitle()`, and set the title to "The cloudier the day, the narrower the band".
2. Use `xlab()` to specify the label of the x-axis to "Date".
3. Use `ylab()` to specify the label of the y-axis to " Air Temperature" .
4. Use `ylim()` to specify the y-axis limits from 0 to 50 degrees Celsius.
5. Use `theme_minimal()` with no arguments to change the theme! Don't forget how to end the line so that it accepts the next phrase.
6. Use `theme()` to tilt the dates by 45 degrees.

`@hint`


`@pre_exercise_code`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Reformat the 'ts' column
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols
```

`@sample_code`
```{r}
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
    # The points
    geom_point(aes(y = max), color = "firebrick", size = 3.5) +
    geom_point(aes(y = min), color = "steelblue", size = 3.5) + 
    # The lines
    geom_line(aes(y = max), color = "firebrick", size = 1, group = 1) + 
    geom_line(aes(y = min), color = "steelblue", size = 1, group = 1) +
    # The point range
    geom_pointrange(color = "black", size = 0.75) +
    # The Title
    ___("___") +
    # The x and y labels
    ___("Date") +
    ___("___") +
    # The y limit
    ___(0, 50) +
    # The theme
    ___ ___
    # Further theming
    ___(axis.text.x=element_text(angle=___)) +
    theme(axis.title.x=element_blank())
p
```

`@solution`
```{r}
p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
 	# The points
	geom_point(aes(y = max), color = "firebrick", size = 3.5) +
	geom_point(aes(y = min), color = "steelblue", size = 3.5) + 
	# The lines
  	geom_line(aes(y = max), color = "firebrick", size = 1, group = 1) + 
  	geom_line(aes(y = min), color = "steelblue", size = 1, group = 1) +
	# The point range
    geom_pointrange(color = "black", size = 0.75) +
	# The Title
	ggtitle("The cloudier the day, the narrower the band") +
	# The x and y labels
  	xlab("Date") +
  	ylab("Air Temperature") +
	# The y limit
	ylim(0, 50) +
	# The theme
  	theme_minimal() +
	# Further theming
  	theme(axis.text.x=element_text(angle=45)) +
  	theme(axis.title.x=element_blank()) + 
	# This simply centers the title.
	theme(plot.title = element_text(hjust = 0.5))
p
```

`@sct`
```{r}
test_ggplot(data_fail_msg = "Did you use the `filtered_6_countries` as the `data` argument to your `ggplot()` call?",
            aes_fail_msg = "Something's wrong in the `aes()` layer of your `ggplot()` call. Did you plot `year` on the x-axis and `percent_yes` on the y-axis?",
            geom_fail_msg = "Did you add a `geom_line` layer with `geom_line()` to your call to `ggplot()`?",
            facet_fail_msg = "Did you add a facet layer with `facet_wrap()`? Make sure to use the correct syntax: `~ country`.")
test_error()
success_msg("Cool! You're killing it! A couple more things and you are done!")
```

---

## Conclusion

```yaml
type: MultipleChoiceExercise
key: 548a3b5e63
lang: r
xp: 50
skills: 1
```

Run the code. Out of the 10 days you see in front of you:

Which day was the cloudiest? 

Which day was the least cloudy?

`@possible_answers`
1. The 19th, 20th, and the 21st were the cloudiest, while the 29th was the least cloudy.
2. The 24th was the cloudiest, while the 29th was least cloudy.
3. The 23th was the cloudiest, while the 25th was least cloudy.
4. [The 29th was the cloudiest, while the 24th was least cloudy.]

`@hint`
The title says it all!

`@pre_exercise_code`
```{r}
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

# Subset and prepare the data
df <- tail(df, 240)
# Use dplyr's select function
df <- dplyr::select(df, c(ts, temp))

# Reformat the 'ts' column
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$temp, by = list(df$ts), function(x) {
  c(max = max(x), min = min(x), avg = mean(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols

p <- ggplot(xdf, aes(x = ts, y = avg, ymin = min, ymax = max)) +
 	# The points
	geom_point(aes(y = max), color = "firebrick", size = 3.5) +
	geom_point(aes(y = min), color = "steelblue", size = 3.5) + 
	# The lines
  	geom_line(aes(y = max), color = "firebrick", size = 1, group = 1) + 
  	geom_line(aes(y = min), color = "steelblue", size = 1, group = 1) +
	# The point range
    geom_pointrange(color = "black", size = 0.75) +
	# The Title
	ggtitle("The cloudier the day, the narrower the band") +
	# The x and y labels
  	xlab("Date") +
  	ylab("Air Temperature") +
	# The y limit
	ylim(0, 50) +
	# The theme
  	theme_minimal() +
	# Further theming
  	theme(axis.text.x=element_text(angle=45)) +
  	theme(axis.title.x=element_blank())
p
```

`@sct`
```{r}
msg1 <- "Incorrect."
msg2 <- "Incorrect."
msg3 <- "Incorrect."
msg4 <- "Correct!"



test_mc(correct=4,  feedback_msgs = c(msg1, msg2, msg3, msg4))
```
