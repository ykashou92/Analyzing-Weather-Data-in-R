---
title: 'Is it more humid than normal?'
description: 'Can we find a `typical` temperature?'
---

## Overview: Typical = Median, not Average

```yaml
type: NormalExercise
key: fe1baa991d
xp: 100
```

When you want to arrive at a _typical_ or _usual_ value for any variable, you want the __median__, not the __mean__.  
So in this chapter, you will:  
1. Select and subset the humidity column to obtain the last two weeks of data
2. Calculate the mean humidity per day (like you calculated the min, max and mean temperature in the last chapter). 
3. Find the median of the mean values over the two-week period. (Typical humidity over past two-weeks)
4. Compare it to the median of the last day.

`@instructions`
You do not need to do anything other than giving this a thought or two. Then just click submit and the data will be loaded.

`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
library(scales)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
# Rename Columns
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols
```

`@solution`
```{r}

```

`@sct`
```{r}
success_msg("Alright! Let's get to it!")
```

---

## Selecting and Subsetting

```yaml
type: NormalExercise
key: 3fd8194c91
lang: r
xp: 100
skills: 1
```



`@instructions`
- Subset the last two weeks from the data.
- Select the relative humidity column `rh` using the `select()` function from the `dplyr` package.
- Save both to variable `df`.

`@hint`
Use the documentation! Type `?tail` and `?dplyr::select` in the console

`@pre_exercise_code`
```{r}
library(scales)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
# Rename Columns
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols
```

`@sample_code`
```{r}
# Subset the last 336 hours (two weeks)
___ <- tail(df, ___)

# Select the humidity column (timestamp is already selected)
___ <- ___::select(df, c(ts, ___))

# Verify with str()
str(df)
```

`@solution`
```{r}
df <- tail(df, 336)
df <- dplyr::select(df, c(ts, rh))
str(df)
```

`@sct`
```{r}
test_object("df", incorrect_msg = "Something is wrong with `df`. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("`Nice! You have been following through. Let's continue...")
```

---

## Mean Humidity per Day

```yaml
type: NormalExercise
key: 2638260fa6
lang: r
xp: 100
skills: 1
```

In the previous chapter you formatted the `ts` (timestamp) column and you aggregated the data to find the minimum, maximum and average temperatures per day. Then you reshaped the data with `cbind()` to make it usable.

You will do all these steps in one go but instead you will apply them to the `rh` column instead of `temp` and find only the mean.

You can find a list of the date and time abbreviation symbols [here](https://stat.ethz.ch/R-manual/R-devel/library/base/html/strptime.html):

`@instructions`
1. Convert the `ts` column from a factor to POSIXlt date using `strptime`
2. Format the `ts` column as a character date, making sure it displays year, month and day and does not display any hours, minutes or seconds.
3. Aggregate the `rh` column by the day `ts`. Unlike in the last chapter where you specified multiple functions to aggregate by, this time you only need the `mean` and it is given as argument to the parameters `FUN`.

`@hint`
Use `?` with the function that you need or refer back to the first chapter.

`@pre_exercise_code`
```{r}
library(scales)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
# Rename Columns
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

df <- tail(df, 336)
df <- dplyr::select(df, c(ts, rh))
```

`@sample_code`
```{r}
# Convert the `ts` columnt to POSIXlt
df$ts <- strptime(df$ts, "%___-%___-%___ %___:%___:%___")

# Format as character
df$ts <- format(df$ts, "%___-%___-%___")

# Aggregate and assign to xdf
xdf <- aggregate(df$___, by = list(df$___), FUN=mean)

# Finally let's reassign the column names
cols <- c("ts", "avg")
colnames(xdf) <- ___
```

`@solution`
```{r}
df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$rh, by = list(df$ts), FUN=mean)
cols <- c("ts", "avg")
colnames(xdf) <- cols

```

`@sct`
```{r}
test_object("xdf", incorrect_msg = "Hmm, might want to recheck your code...")

test_error()
success_msg("If you got through that, then you can get through anything, good work!")
```

---

## Typical Humidity over the past Two Weeks

```yaml
type: NormalExercise
key: 62e8d86bc2
lang: r
xp: 100
skills: 1
```



`@instructions`
Calculate the median humidity over the past two weeks using the `median()` function.  
Remember, your dataframe is assigned to the variable `xdf`.

Save the value to a variable called `median_over_two_weeks` and print it out.

`@hint`
`?median` will always help.

`@pre_exercise_code`
```{r}
library(scales)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
# Rename Columns
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

df <- tail(df, 336)
df <- dplyr::select(df, c(ts, rh))

df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$rh, by = list(df$ts), FUN=mean)
cols <- c("ts", "avg")
colnames(xdf) <- cols


```

`@sample_code`
```{r}
# Write your code below here:
___ <- ___(___)

print(___)
```

`@solution`
```{r}
median_over_two_weeks <- median(xdf$avg)

print(median_over_two_weeks)
```

`@sct`
```{r}
test_object("median_over_two_weeks", incorrect_msg = "Hmm, might want to recheck your code...")

test_error()
success_msg("`Awesome! Simple, no?")
```

---

## Conclusion

```yaml
type: MultipleChoiceExercise
key: 9c564dac47
lang: r
xp: 50
skills: 1
```

Time to put things completely in your hand...here's what you will do, just like you found aggregated the humidity to find the average for each day, now use the console on the right to:
- Aggregate the `rh` column from the `df` dataframe by the `ts` column to find the median of each day.
- Subset the last entry in the column and assign it to a variable `median_of_last_day`.
- Compare `median_of_last_day` and `median_over_two_weeks` and answer the statement.

`@possible_answers`
Calculate the median humidity of the last day and select the correct statement.

1. [The last day was less humid than your typical day.]
2. The last day was more humid than normal.
3. Your typical day was more humid than the last day.
4. There was no difference in humidity between the last day and your ordinary day.

`@hint`
Use every resource you have, refer to old exercises, go at it step-by-step, and remember to use the documentation! `?aggregate`

`@pre_exercise_code`
```{r}
library(scales)
library(reshape2)
library(lubridate)
library(dplyr)

# Read the data
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
# Rename Columns

cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df) = cols

df <- tail(df, 336)
df <- dplyr::select(df, c(ts, rh))

df$ts <- strptime(df$ts, "%Y-%m-%d %H:%M:%S")
df$ts <- format(df$ts, "%Y-%m-%d")

xdf <- aggregate(df$rh, by = list(df$ts), FUN=mean)
cols <- c("ts", "avg")
colnames(xdf) <- cols

median_over_two_weeks <- median(xdf$avg)

ydf <- aggregate(df$rh, by = list(df$ts), FUN=median)
cols <- c("ts", "med")
colnames(ydf) <- cols
ydf <- tail(ydf, 1)

median_of_last_day <- ydf$med
```

`@sct`
```{r}
msg1 <- "Correct!"
msg2 <- "Incorrect."
msg3 <- "Incorrect."
msg4 <- "Incorrect."


test_mc(correct=1,  feedback_msgs = c(msg1, msg2, msg3, msg4))
```
