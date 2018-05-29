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

**R** has a function called `read.delim()` which can read any *delimited text file*, whether it is tab-delimited, comma-delimited, etc.. We would only need to specify the file, and the separator.

Assuming you have a file named `sample.tsv` that is *tab-separated*, you can import the file into a variable named *sample_data* like so:    
``sample_data <- read.delim("sample.tsv", sep = "\t")``.

With comma-separated variables, can simply set `sep = ","`. The "\" is unnecessary in this case.

Modify the code on the right to work on the file mentioned above!

`@hint`


`@pre_exercise_code`
```{r}
x <- read_table(url("https://assets.datacamp.com/production/repositories/2638/datasets/7a889124ca4aeb612a4067491b624d4797a16e50/CR1000_OneHour.dat"))
```
`@sample_code`
```{r}
sample_data <- read.delim("sample.tsv", sep = "\t")
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












