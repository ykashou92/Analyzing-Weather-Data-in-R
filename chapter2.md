---
  title: "Is it cloudy today?"
  description: "This is a template chapter."
  v2: true

---
## Ex 1.1

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 33e93d2cb0



```

We will begin visualizing the cloud bands.

`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
library(magrittr)
library(scales)
library(ggplot2)
library(reshape2)
library(lubridate)

data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
df <- read.delim(file = data, sep = ",", skip=1)   
cols <- c("ts", "rec", "ws", "wd", "wsc", "srad", "temp", "rh", "rain", "vis", "bp")
colnames(df)= cols

subtract_month <- tail(df$ts, 1) %m-% months(1)
df <- subset(df, ts > subtract_month)
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

ggplot(xdf, aes(x = day, y = avg, ymin = min, ymax = max)) +
  geom_line(aes(y = max), color = "steelblue", size = 1, group = 1) +
  geom_line(aes(y = min), color = "steelblue", size = 1, group = 1) +
  geom_pointrange(color = "black", size= 0.75) +
  geom_point(aes(y = max), color = "steelblue", size = 3.5) +
  geom_point(aes(y = min), color = "steelblue", size = 3.5) +
  theme_minimal() +
  ylab("Air Temperature") +
  theme(axis.title.x=element_blank())

x <- 0
```

`@solution`
```{r}
print(x)
```
`@sct`
```{r}
success_msg("Nice work! Success messages are a great way to keep users engaged even after the exercise is over. Try to encourage them to play around in the console to really grasp the concepts you're trying to teach!")
```




