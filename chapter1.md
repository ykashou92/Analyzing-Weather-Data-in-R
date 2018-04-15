---
title: Reading Data
description: >-
  Insert the chapter description here


---
## Importing the Data

```yaml
type: NormalExercise

xp: NaN

key: 3938a0ce13
```

We always begin the data science pipeline by asking a **question**. This question is what defines what we want to solve, or understand from the data.  However, we first need to take a see and explore our data.

`@instructions`
We have a single data file: 
- CR1000_OneHour.dat


Importing data in  **R** is very simple when using prepackaged libraries.

`@hint`


`@pre_exercise_code`
```{}
read_csv(url("https://assets.datacamp.com/production/repositories/2638/datasets/7a889124ca4aeb612a4067491b624d4797a16e50/CR1000_OneHour.dat"))
```
`@sample_code`
```{}
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













