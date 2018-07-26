---
    title: Where is the wind with the highest power coming from?
    description: Finally some math and functional programming in R.
    v2: true



---
## Overview: The Wind Rose

```yaml
type: NormalExercise
key: b0437f91cf
lang: r
xp: 100
skills: 1
```
Meteorologists usually create a **pollution rose** or a **wind rose** in order to show the distribution of pollutants and particles in the air. These are beautiful plots that take into account the wind speed and direction, but you want to find the direction from which the wind with the highest _power_ is coming from. And this demands some functional program and a bit of math. Oh and Wikipedia!

While faster winds have faster speeds, air density plays an important role.


`@instructions`
- Read through [this](https://en.wikipedia.org/wiki/Density_of_air#Humidity_(water_vapor)) Wikipedia section on the density of humid air.
- Read through [this](https://en.wikipedia.org/wiki/Wind_power#Wind_energy) Wikipedia section on calculating wind energy and power.
- Click submit when you are done.


`@sct`
```{r}
success_msg("Alright! Let's get to it!")
```




---
## R Functions - Example

```yaml
type: NormalExercise
key: ae3e36862b
lang: r
xp: 100
skills: 1
```
In case you are unfamiliar with functions, you will create a basic function of __two inputs__ and a __single output__.
Functions are created using `function(inputs)` like so:
```
example_function <- function(input) {
    return(input * 2)
}
```
This function returns double the value of the input. Notice that the code to calculate is nested inside the parenthesis `{}`

`@instructions`

- Create a function called `add_xyz`, that takes inputs `x`, `y` and `z` and returns the sum of all three when called.  
- Call the function using any values of `x`, `y` and `z`.  



`@sample_code`
```{r}
___ <- ___(x, ___, ___) {
    ___(___ + ___ + ___ )
}

add_xyz(___, ___, ___)
```

`@solution`
```{r}
add_xyz <- function(x, y, z) {
    return(x + y + z )
}
```


`@sct`
```{r}
test_object("add_xyz", incorrect_msg = "Something is wrong with `add_xyz`. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("`Nice! Onwards to more relevant functions") 
```


---
## Calculating Air Density: Part 1

```yaml
type: NormalExercise
xp: 100
key: 5e9efdfa1f
lang: r
skills: 1
```
Calculating air density is a bit of a hassle, but no worries! To find the wind power, you must already know the air density.


`@instructions`
Create a function called `calculate_air_density` that takes an input `df` and returns the variable `air_density` (you do not have to define it yet).  
Inside the function you may specify the constants you encountered on Wikipedia earlier:  
- $ M_d = 0.028964 $ as `M_d`
- $ M_v = 0.018016 $ as `M_v`
- $ R_d = 287.058 $ as `R_d`
- $ R_v = 461.495 $ as `R_v`



`@hint`



`@pre_exercise_code`

```{r}

```
`@sample_code`

```{r}
___ <- ___(df) {
    
    # ----------Part 1----------
    # Molar mass of dry air (kg/mol)
    M_d <- ___
    # Molar mass of water vapor (kg/mol)
    ___ <- ___
    
    # Spcific gas constant for dry air (J/(kg.K))
    ___ <- ___
    # Specific Gas constant for water vapor (J/(kg.K))
    ___ <- ___
    
    return(air_density)
}
```
`@solution`
```{r}
calculate_air_density <- function(df){
    
    # ----------Part 1----------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    return(air_density)
  }
```
`@sct`
```{r}
test_object("calculate_air_density", incorrect_msg = "Something is wrong with your function. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("`Nice! Time for the second part.") 
```




---
## Calculating Air Density: Part 2

```yaml
type: NormalExercise
key: 795c332365
lang: r
xp: 100
skills: 1
```
To continue, there are some variable you can calculate from data that you already have! Scroll down to Part 2.

`@instructions`
1. Assign the `rh` column of the data frame to the variable `phi`. (line 17)
2. Calculate the __absolute__ temperature by adding `273.15` to the `temp` column of the data frame. Assign it to `abs_temp`. (line 20)
3. Find the absolute pressure `p` by multiply the `bp` column by `100` to obtain the pressure in __Pascals__. (line 23)

`@hint`

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    ___ <- df$___
    
    # Temperature (K)
    abs_temp <- df$___ + ___
    
    # Absolute pressure (Pa)
    p <- df$___ * 100
    
    
    
    return(air_density)
    
  }
```

`@solution`
```{r}
calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    phi <- df$rh
    
    # Temperature (K)
    abs_temp <- df$temp + 273.15
    
    # Absolute pressure (Pa)
    p <- df$bp * 100
    
    return(air_density)
}
```

`@sct`
```{r}
test_object("calculate_air_density", incorrect_msg = "Something is wrong with your function. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("`Cool! You are half-way there!") 
```




---
## Calculating Air Density: Part 3

```yaml
type: NormalExercise
key: 58ef687a35
lang: r
xp: 100
skills: 1
```
Three equations before the final one.
Scroll down to Part 3.

`@instructions`
We have written two of these equations as they are very trivial. You are left with the task to implement the following saturation vapor pressure equation (line 28): 

$ p_{sat} = 6.1078 \times 10^{\frac{7.5T}{T+237.3}} * 100$

Assign it to the variable `p_sat`.

Note: it is multiplied by 100 to convert it to Pascals, also T is in Celsius not in Kelvin.


`@hint`

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    phi <- df$rh
    
    # Temperature (K)
    abs_temp <- df$temp + 273.15
    
    # Absolute pressure (Pa)
    p <- df$bp * 100
    
    
    # Part 3 -----------------------------------------
    # Saturation Vapor Pressure (Pa)
    p_sat <- ___ * ___ ^ ((___ * df$temp)/(df$___ + 237.3)) * 100

    # Vapor pressure of water (Pa)
    p_v <- phi*p_sat

    # Partial Pressure of dry air (Pa)
    p_d <- p - p_v
    
    return(air_density)
    
  }
```

`@solution`
```{r}
calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    phi <- df$rh
    
    # Absolute Temperature (K)
    abs_temp <- df$temp + 273.15
    
    # Absolute pressure (Pa)
    p <- df$bp * 100
    
    
    # Part 3 -----------------------------------------
    # Saturation Vapor Pressure (Pa)
    p_sat <- 6.1078 * 10^((7.5*df$temp)/(df$temp+237.3)) * 100

    # Vapor pressure of water (Pa)
    p_v <- phi*p_sat

    # Partial Pressure of dry air (Pa)
    p_d <- p - p_v
    
    return(air_density)
    
  }
```

`@sct`
```{r}
test_object("calculate_air_density", incorrect_msg = "Something is wrong with your function. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("`Nice! One more part to go!") 
```





---
## Calculating Air Density: Part 4

```yaml
type: NormalExercise
key: d3ba462131
lang: r
xp: 100
skills: 1
```
Finally, the equation for air density:

$ \rho_{\,\mathrm{humid~air}} = \frac{p\_{d}}{R\_dT} + \frac{p\_{v}}{R\_vT}$

`@instructions`

- Implement the equation in code and assign it to the variable `air_density`. (line 39)
- Run the function on the dataframe `df`. (line 46)

Note: T is in Kelvin not Celsius. Remember, you assigned this to a certain variable.

`@hint`

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
calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    phi <- df$rh
    
    # Temperature (K)
    abs_temp <- df$temp + 273.15
    
    # Absolute pressure (Pa)
    p <- df$bp * 100
    
    
    # Part 3 -----------------------------------------
    # Saturation Vapor Pressure (Pa)
    p_sat <- 6.1078 * 10^((7.5*df$temp)/(df$temp+237.3)) * 100

    # Vapor pressure of water (Pa)
    p_v <- phi*p_sat

    # Partial Pressure of dry air (Pa)
    p_d <- p - p_v


    # Part 4 -----------------------------------------
    # Air Density (kg/m^3)
    air_density <- ((___ / (___ * ___)) + (___ / (___ * ___)))
    
    return(air_density)
    
  }
  
# Run the function 
___(df)
```

`@solution`
```{r}
calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    phi <- df$rh
    
    # Temperature (K)
    abs_temp <- df$temp + 273.15
    
    # Absolute pressure (Pa)
    p <- df$bp * 100
    
    
    # Part 3 -----------------------------------------
    # Saturation Vapor Pressure (Pa)
    p_sat <- 6.1078 * 10^((7.5*df$temp)/(df$temp+237.3)) * 100

    # Vapor pressure of water (Pa)
    p_v <- phi*p_sat

    # Partial Pressure of dry air (Pa)
    p_d <- p - p_v


    # Part 4 -----------------------------------------
    # Air Density (kg/m^3)
    air_density <- ((p_d / (R_d * abs_temp)) + (p_v / (R_v * abs_temp)))
    
    return(air_density)
    
  }
  
calculate_air_density(df)
```

`@sct`
```{r}
test_object("calculate_air_density", incorrect_msg = "Something is wrong with your function. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("That's some good work! Next you will calculate the wind power!") 
```

---
## Calculating Wind Power

```yaml
type: NormalExercise
key: d5124d9a8f
lang: r
xp: 100
skills: 1
```


`@instructions`
- Run `calculate_air_density` on `df` and assign it to the variable name `air_density`.
- Write a function called `calculate_wind_power`, with inputs  `area`, `air_density` `wind_speed` and output `wind_power`. Use the equation below as your guide:

$$ P = \frac{1}{2}A \rho v^3 $$

- Call the function and save the result to a variable called `wind_power`. 
Note: Use `A = 1` when calling the function at the end as you do not have a collecting area (wind turbine). So this will measure the estimated wind power incident on 1 meter squared of area. 

`@hint`
Don't forget the `return()` statement!

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

calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    phi <- df$rh
    
    # Temperature (K)
    abs_temp <- df$temp + 273.15
    
    # Absolute pressure (Pa)
    p <- df$bp * 100
    
    
    # Part 3 -----------------------------------------
    # Saturation Vapor Pressure (Pa)
    p_sat <- 6.1078 * 10^((7.5*df$temp)/(df$temp+237.3)) * 100

    # Vapor pressure of water (Pa)
    p_v <- phi*p_sat

    # Partial Pressure of dry air (Pa)
    p_d <- p - p_v


    # Part 4 -----------------------------------------
    # Air Density (kg/m^3)
    air_density <- ((p_d / (R_d * abs_temp)) + (p_v / (R_v * abs_temp)))
    
    return(air_density)
    
  }
```

`@sample_code`
```{r}
# Calculate the air density
___ <- ___(___)

# Create your function here:
___ <- ___(area, air_density, ___) {
    wind_power = 0.5 * ___ * ___ * (___ ^ ___)
    ___(___)
}

# Run your function on air_density and windspeed
wind_power <- calculate_wind_power(1, ___, df$ws)
```

`@solution`
```{r}
air_density <- calculate_air_density(df)
calculate_wind_power <- function(area, air_density, wind_speed) {
    wind_power = 0.5 * area * air_density * (wind_speed ^ 3)
    return(wind_power)
}

wind_power <- calculate_wind_power(1, air_density, df$ws)

```

`@sct`
```{r}
test_object("calculate_wind_power", incorrect_msg = "Something is wrong with your function. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("Time for plotting!") 
```



---
## Plotting the Windrose: Part 1

```yaml
type: NormalExercise
key: 618f66c1f8
lang: r
xp: 100
skills: 1
```
You will now prepare the data for the windrose.

`@instructions`
- Import the library `openair`
- Define a variable `z`, which consists of a string of colors in this order, `"dodgerblue4", "gray", "firebrick"`
- Create a dataframe `wind_df` that contains only two columns `wind_power` (you calculated this in the last exercise) and `wind_direction` (already present in the dataframe `df` as `wd`)

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

calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    phi <- df$rh
    
    # Temperature (K)
    abs_temp <- df$temp + 273.15
    
    # Absolute pressure (Pa)
    p <- df$bp * 100
    
    
    # Part 3 -----------------------------------------
    # Saturation Vapor Pressure (Pa)
    p_sat <- 6.1078 * 10^((7.5*df$temp)/(df$temp+237.3)) * 100

    # Vapor pressure of water (Pa)
    p_v <- phi*p_sat

    # Partial Pressure of dry air (Pa)
    p_d <- p - p_v


    # Part 4 -----------------------------------------
    # Air Density (kg/m^3)
    air_density <- ((p_d / (R_d * abs_temp)) + (p_v / (R_v * abs_temp)))
    
    return(air_density)
    
  }
  
  
air_density <- calculate_air_density(df)
calculate_wind_power <- function(area, air_density, wind_speed) {
    wind_power = 0.5 * area * air_density * (wind_speed ^ 3)
    return(wind_power)
}

wind_power <- calculate_wind_power(1, air_density, df$ws)
```

`@sample_code`
```{r}
# Import openair
library(openair)

# Define the color list
___ <- c(___, ___, ___)

# Create the data frame wind_df
wind_df <- NULL
wind_df$___ <- wind_power
___$___ <- df$___


```

`@solution`
```{r}
# Import openair
library(openair)

# Define the color list
z <- c("dodgerblue", "gray", "firebrick")

# Create the data frame wind_df
wind_df <- NULL
wind_df$wind_power <- wind_power
wind_df$wind_direction <- df$wd


```

`@sct`
```{r}
test_object("wind_df", incorrect_msg = "Something is wrong with your function. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("Time for plotting!")
```


---
## Plotting the Windrose: Part 2

```yaml
type: NormalExercise
key: 392b80a9db
lang: r
xp: 100
skills: 1
```

Traditionally, using the `windRose()` function demands `ws` and a `wd` arguments, short for wind speed and wind direction respectively. But keep in mind that you want to plot the wind power!


`@instructions`
- Use the `windRose()` function appropriately. Pass `wind_power` into the `ws` argument and `wind_direction` into `wd`. 

`@hint`
By now you should know that your hint comes from your own research `?windRose()`.

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

calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    phi <- df$rh
    
    # Temperature (K)
    abs_temp <- df$temp + 273.15
    
    # Absolute pressure (Pa)
    p <- df$bp * 100
    
    
    # Part 3 -----------------------------------------
    # Saturation Vapor Pressure (Pa)
    p_sat <- 6.1078 * 10^((7.5*df$temp)/(df$temp+237.3)) * 100

    # Vapor pressure of water (Pa)
    p_v <- phi*p_sat

    # Partial Pressure of dry air (Pa)
    p_d <- p - p_v


    # Part 4 -----------------------------------------
    # Air Density (kg/m^3)
    air_density <- ((p_d / (R_d * abs_temp)) + (p_v / (R_v * abs_temp)))
    
    return(air_density)
    
  }
  
  
air_density <- calculate_air_density(df)
calculate_wind_power <- function(area, air_density, wind_speed) {
    wind_power = 0.5 * area * air_density * (wind_speed ^ 3)
    return(wind_power)
}

wind_power <- calculate_wind_power(1, air_density, df$ws)

# Import openair
library(openair)

# Define the color list
z <- c("dodgerblue", "gray", "firebrick")

# Create the data frame wind_df
wind_df <- NULL
wind_df$wind_power <- wind_power
wind_df$wind_direction <- df$wd
wind_df <- data.frame(wind_df)
```

`@sample_code`
```{r}
# Plot the rose
___(mydata = ___, ws = "___", wd = "___", cols = ___, annotate = FALSE)
```

`@solution`
```{r}
# Plot the rose
windRose(mydata = wind_df, ws = "wind_power", wd = "wind_direction", cols = z, annotate = FALSE)
```

`@sct`
```{r}
test_object("windRose", incorrect_msg = "Something is wrong with your function. Make sure you've used the correct values to the arguments in the function.")

test_error()
success_msg("Time for plotting!")
```

---
## Conclusion

```yaml
type: MultipleChoiceExercise
key: 35d7f4b05d
lang: r
xp: 50
skills: 1
```

Where is the wind with the highest power coming from?

`@instructions`
Calculate the median humidity of the last day and select the correct statement.

1. North
2. South-East
3. [South-West]
4. North-West

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

calculate_air_density <- function(df){

    # Part 1 -----------------------------------------
    # Molar mass of dry air (kg/mol)
    M_d <- 0.028964
    # Molar mass of water vapor (kg/mol)
    M_v <- 0.018016

    # Spcific gas constant for dry air (J/(kg.K))
    R_d <- 287.058
    # Specific Gas constant for water vapor (J/(kg.K))
    R_v <- 461.495
    
    
    # Part 2 -----------------------------------------
    # Relative humidity (%)
    phi <- df$rh
    
    # Temperature (K)
    abs_temp <- df$temp + 273.15
    
    # Absolute pressure (Pa)
    p <- df$bp * 100
    
    
    # Part 3 -----------------------------------------
    # Saturation Vapor Pressure (Pa)
    p_sat <- 6.1078 * 10^((7.5*df$temp)/(df$temp+237.3)) * 100

    # Vapor pressure of water (Pa)
    p_v <- phi*p_sat

    # Partial Pressure of dry air (Pa)
    p_d <- p - p_v


    # Part 4 -----------------------------------------
    # Air Density (kg/m^3)
    air_density <- ((p_d / (R_d * abs_temp)) + (p_v / (R_v * abs_temp)))
    
    return(air_density)
    
  }
  
  
air_density <- calculate_air_density(df)
calculate_wind_power <- function(area, air_density, wind_speed) {
    wind_power = 0.5 * area * air_density * (wind_speed ^ 3)
    return(wind_power)
}

wind_power <- calculate_wind_power(1, air_density, df$ws)

# Import openair
library(openair)

# Define the color list
z <- c("dodgerblue", "gray", "firebrick")

# Create the data frame wind_df
wind_df <- NULL
wind_df$wind_power <- wind_power
wind_df$wind_direction <- df$wd
wind_df <- data.frame(wind_df)

# Plot the rose
windRose(mydata = wind_df, ws = "wind_power", wd = "wind_direction", cols = z, annotate = FALSE, key=FALSE)
```

`@sct`
```{r}
msg1 <- "Incorrect."
msg2 <- "Incorrect."
msg3 <- "Correct!"
msg4 <- "Incorrect."


test_mc(correct=3,  feedback_msgs = c(msg1, msg2, msg3, msg4))
```
