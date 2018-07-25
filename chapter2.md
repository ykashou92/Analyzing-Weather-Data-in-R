---
  title: "Is it more humid than normal?"
  description: "Insert the chapter description here"
  v2: true

---
## Overview: Typical = Median, not Average
```yaml
type: NormalExercise

xp: 100

key: fe1baa991d



```
When we want to arrive at a _typical_ or _usual_ value for any variable, we want the __median__, not the __mean__.  
So here's what you are going to do, in this chapter, you will:  
1. Select and subset the humidity column to obtain the last two weeks of data
2. Calculate the mean humidity per day (like you calculated the min, max and mean temperature in the last chapter). 
3. Find the median of the mean values over the two-week period. (Typical humidity over past two-weeks)
4. Compare it to the median of the last day. 


`@instructions`
You do not need to do anything other than giving this a thought or two. Then just click submit and the data will be loaded.

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

`@hint`

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

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


`@instructions`

`@hint`

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

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

`@hint`

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

```


---
## Conclusion: Is it more humid than normal (True/False)

```yaml
type: NormalExercise
key: 0e53467f7e
lang: r
xp: 100
skills: 1
```


`@instructions`

`@hint`

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

```
