---
title: Reading Data
description: >-
  Insert the chapter description here


---
## Importing the Data

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

**R** has a function called `read.delim()` which can read any delimited text file, whether it is tab-delimited, comma-delimited, etc.. We would only need to specify the file, and the separator.

You can read a file `sample.dat` like so: `read.delim('sample.dat', sep = ',')`.
And assign it to a variable like so: `s <- read.delim('sample.dat', sep = ',')`

`@hint`


`@pre_exercise_code`
```{r}
read_table(url("https://assets.datacamp.com/production/repositories/2638/datasets/7a889124ca4aeb612a4067491b624d4797a16e50/CR1000_OneHour.dat"))
```
`@sample_code`
```{r}
df <- read_csv("CR1000_OneHour.dat", sep=",")
```







---
## Viewing the last 10 Rows

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 98b8ed2d89
```














---
## General Information

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 56a2ee92da
```












