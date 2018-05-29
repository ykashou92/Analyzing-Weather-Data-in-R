---
title: Reading Data
description: >-
  Insert the chapter description here


---
## Let's Begin by Importing the Data into a DataFrame

```yaml
type: NormalExercise

xp: 100

key: 3938a0ce13
```

Alright! Let's begin!  
We always begin the data science pipeline by asking a **question**. This question is what defines what we want to solve, or understand from the data. However, we first need to take a look and explore our data...

`@instructions`
We have a single data file (comma-separated): 
- CR1000_OneHour.dat

**R** has a function called `read.delim()` which can read any *delimited text file*, whether it is tab-delimited, comma-delimited, etc.. We would only need to specify the file, and the separator.

Assuming you have a file named `sample.tsv` that is *tab-separated*, you can import the file into a variable named *sample_data* like so:    
``sample_data <- read.delim("sample.tsv", sep = "\t")``.

With comma-separated variables, can simply set `sep = ","`. The "\" is unnecessary in this case.

We preloaded the dataset into a variable called `data`
This way, you can refer to the file as `data` instead of `"CR1000_OneHour.dat"` when assigning it to a variable `df`!

`@hint`


`@pre_exercise_code`
```{r}
#data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")
x = 4 + 5
```
`@sample_code`
```{r}
# We preloaded the dataset into a variable called data
# This way, you can refer to the file as `data` instead of `CR1000_OneHour.dat`
data <- url("https://assets.datacamp.com/production/repositories/2638/datasets/e73949a03c41fd2cbe1de7691ff7adfc624bd22b/CR1000_OneHour.dat")

# Change the following code such that it reads the 'data' variable into a variable calle 'df'
___ <- read.delim(file = ___, sep = ___)

# The head() function displays the first 5 rows of the data frame.
head(___)
```
`@solution`
```{r}
#df <- read.delim("CR1000_OneHour.dat", sep = ",")
print(x)
```
`@sct`
```{r}
test_output_contains(9, incorrect_msg = "")
success_msg("Awesome! See how the console shows the result of the R code you submitted?")
```





---
## How do we view the last 10 Rows?

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 98b8ed2d89
```



`@instructions`


`@hint`











---
## Let's do something about the column names!

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 56a2ee92da
```



`@instructions`


`@hint`











---
## Insert exercise title here

```yaml
type: MultipleChoiceExercise

xp: 50

key: 6ea3ae7859
```

How do we view the first 7 rows of a Data Frame variable `df`?

`@instructions`
- view(df, 7)
- list(df, 7)
- head(df)
- head(df, 7)   
- tail(df, 3)

`@hint`
We have not covered the list() function. Maybe it's another one?

`@pre_exercise_code`
```{r}
head(df, 7)
```


`@sct`
```{r}
head(df, 7)
```



