---
title: Reading Data
description: >-
  This chapter covers reading in the data and learning more about the data frame that contains it.


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

xp: 100

key: 6e00a1acb2
```



`@instructions`


`@hint`











---
## Quiz Question: Taking a look!

```yaml
type: MultipleChoiceExercise

xp: 50

key: 1548cfd0ea
```

How can I view the **first 7 rows** of a dataframe called `df`?

`@instructions`
1. tail(df, 3)
2. view(df, 7)
3. [head(df, 7)]
4. list(df, 7)

`@hint`
Remember our previous lessons. We learned two functions: `head()` and `tail()`.




`@sct`
```{r}
msg1 <- "Incorrect. Remember that the `tail()` function retrieves the last N rows."
msg2 <- "Incorrect. Remember that the `tail()` function retrieves the last N rows."
msg3 <- "Correct! "
msg4 <- "Incorrect. Have we even covered this yet?"


test_mc(correct=3,  feedback_msgs = c(msg1, msg2, msg3, msg4))
```





---
## Chapter 1 Quiz!

```yaml
type: TabExercise

xp: 100

key: 3c239feb00
```

Alright, so far, we have covered the following:

- Importing a file into the R workspace as a Data Frame
- Viewing the first and last rows
- Displaying a summary of the data frame
- Changing the column names into a more human-readable form

Well done! Now it is time to test your skills with a few multiple choice questions!

**Questions:**
1. How can I view the **first 7 rows** of a dataframe called `df`?
2. How can I read a tab-separated file?

3. What is a data frame?











***



```yaml
type: MultipleChoiceExercise

xp: 50

key: 60ab93e5b8
```



`@instructions`
1. tail(df, 3)
2. view(df, 7)
3. [head(df, 7)]
4. list(df, 7)

`@hint`





`@sct`
```{undefined}
msg1 <- "Incorrect. Remember that the `tail()` function retrieves the last N rows."
msg2 <- "Incorrect. Remember that the `tail()` function retrieves the last N rows."
msg3 <- "Correct! "
msg4 <- "Incorrect. Have we even covered this yet?"

test_mc(correct=3,  feedback_msgs = c(msg1, msg2, msg3, msg4))
```






***



```yaml
type: MultipleChoiceExercise

xp: 50

key: 7d90552b7c
```



`@instructions`


`@hint`












---
## Quiz Question: To the documentation we go!

```yaml
type: MultipleChoiceExercise

xp: 50

key: 2d492ae581
```

Open the link `https://stat.ethz.ch/R-manual/R-devel/library/utils/html/read.table.html` and read through the **Usage** heading.

Which of the following functions can import a comma-separated text file?
1. `read.table(...)`
2. `read.csv(...)`
3. `read.csv2(...)`
4. `read.delim(...)`
5. `read.delim2(...)`

`@instructions`
1. 1 and 4
2. 2 and 3
3. 4 and 5
4. [1, 2, 3, 4 and 5]
5. 2 and 4

`@hint`
Remember that what `sep = ","` does!

`@pre_exercise_code`
```{r}
 
```


`@sct`
```{r}
msg1 <- "Correct! They all work!"
msg2 <- "Incorrect! Read through the documentation.."
test_mc(correct=4,  feedback_msgs = c(msg2, msg2, msg2, msg1, msg2))
```



