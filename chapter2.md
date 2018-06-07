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
## Insert exercise title here

```yaml
type: NormalExercise

xp: 100

key: c7a40a5a73



```



`@instructions`


`@hint`










