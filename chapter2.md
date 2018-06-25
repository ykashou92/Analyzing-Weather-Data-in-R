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

The cloudier the day, the narrower the temperature range. This is because clouds prevent heat from entering the atmosphere and also cause heat to be trapped on Earth, thus decreasing the amount of energy Earth receives from the Sun and the amount of energy  that leaves Earth. 

And vice versa...

`@instructions`
Take a look at the plot on the right and the corresponding code. Read through it and don't worry if you don't understand it completely. We will be constructing this plot step-by-step from scratch. When you are ready, press submit to move to the first exercise.

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
  c(max = max(x), min = min(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])

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
  theme(axis.title.x=element_blank())
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
  c(max = max(x), min = min(x)) })

xdf <- cbind(xdf[-ncol(xdf)], xdf[[ncol(xdf)]])

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
  theme(axis.title.x=element_blank())
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

To build our plot, we first need to import a few libraries:

- **scales** To change and modify the scaling of the plot  
- **ggplot2** To actually build the plot using a "grammar of graphics"  
- **reshape2** To reshape our data  
- **lubridate** To manipulate data concerning date and time  
- **dplyr** To easily select columns from the dataframe

`@instructions`
We can import a library (for instance: the library **rvest**) like so:  
`library(rvest)`

Import `scales`,  `ggplot2`, `reshape2`, `lubridate` and `dplyr` like the example above.

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
success_msg("`Nice! We are one step closer. Let's continue...")
```






---
## Choosing our Data

```yaml
type: NormalExercise

xp: 100

key: 400058a13c



```

So we have the data. Now we ask: What parts of these data do we need?  We are going to visualize temperature over a period of time. Hence we only select the columns we need and the number of rows we wish to represent such that we create an aesthetically pleasing graph that is easy to read and fast to run.

`@instructions`
We can use functions from a specific library, especially if another function with the same name exists in another library (in this case, they both will work.)
`dplyr::select()` instead of `select()`.

Here's what you should do:

- Subset the last 10 days.
- Select the `ts` and `temp` columns from the dataframe and assign it to variable `df` (hence replacing the old one)
- Verify that it worked with by calling the `str()` function on our dataframe.

`@hint`
The `tail(...)` function can select the last N rows and we are dealing with hourly data. How many rows will represent that last 10-day period?

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

xp: 100

key: 88f9897b09



```

Time data can take on various forms, we are concerned specifically with the **POSIXct** data type, which is _**the number of seconds since the start of January 1, 1970**_.   
  
We will format our `ts` column in two steps:

1. Convert it from a **factor** (category) object to a **date** object, specifically POSIXct.
2. Format the column to represent only year-month-day values without hours, minutes or seconds.

`@instructions`
- Use the `strptime(...)` function to convert the `ts` column from a **factor** to a **date (POSIXct)**.  You need to specify the format of the date `"%Y-%m-%d %H:%M:%S"`
- Use the `format(...)` function to convert the column from a `Year-Month-Day Hour:Minute:Second` format to a `Year-Month-Day`.

We have written `str(df$ts)` to run on every step such that you may keep track of the changes of the date's format before and after each conversion and gain better intuition.

`@hint`


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
# We can select a specific column from the dataframe by using $ sign. 
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

xp: 100

key: fba56b2895



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

xp: 100

key: a87cae653d



```

This is our final step before visualizing our data. The aggregated dataframe `xdf` actually needs to be reshaped. See it appears like a normal dataframe, but it the mean, minimum and maximum columns are actually nested within the timestamp, instead of alongside it. The dataframe we have now is a multi-index dataframe, or multiple-layered.

`@instructions`
Look up `?cbind`...

`@hint`



`@sample_code`
```{r}
cols <- c("ts", "max", "min","avg")
colnames(xdf) = cols
```








---
## Plotting: The Aesthetics

```yaml
type: NormalExercise

xp: 100

key: 3af49d5904



```

Now that we have a nice, clean data frame `xdf` ready...Let's begin by plotting the axes. In the `ggplot()` function, we first specify the data source then the aesthetics of the plot, i.e. the data points involved that would be on the **x** and **y** axes.

Note that you do not have to use the column selector `$` in the `aes` argument of `ggplot()`. Instead you can simply type `ts` to reference the `ts` column of the data frame `df` if `df` is given as the data argument.

`ggplot(df, aes(x = xcolumn, y = ycolumn)` as opposed to `ggplot(df, aes(x = df$xcolumn, y = df$ycolumn)`

`@instructions`
Use `ggplot()` to plot the data, where `x`, `y`, `ymin`, and `ymax` will correspond to the `ts`, `avg`, `min` and `max` columns of the data frame `xdf`, here's an example:  
`ggplot(df, aes(x = time, y = temperature)`

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
# Use the ggplot()
# Select the data
# Define the aes() argument
___(___, ___(x = ts, y = ___, ymin = ___, ymax = max))
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













