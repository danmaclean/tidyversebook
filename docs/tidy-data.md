# Tidy Data

## About this chapter

1. Questions:
- What is tidy data?
2. Objectives:
- Understanding data type
- Understanding tidy data structures
- Explicitly describing and checking the data structure
3. Keypoints:
- Data needs to be in a particular format for tidy data principles to work

## Tidy data

There are many ways to structure data. Here are two quite common ones.


<table><th></th><th>treatment A</th><th>treatment B</th>
<tr><td>John Smith</td><td> 11 </td><td> 2</td></tr>
<tr><td>Jane Doe</td><td> 16 </td><td> 11</td></tr>
<tr><td>Mary Johnson</td><td> 3 </td><td> 1</td></tr>
</table>



<table><th></th><th>John Smith</th><th>Jane Doe</th><th>Mary Johnson</th>
<tr><td>treatment A</td><td>11</td><td>16</td><td>3</td></tr>
<tr><td>treatment B</td><td>2</td><td>11</td><td>1</td></tr>
</table>

_source:_ [Hadley Wickham ](http://vita.had.co.nz/papers/tidy-data.pdf) 	

Tables contain two things, variables and values for those variables. In these two tables there are only three variables.  `treatment` is one, with the values `a` and `b` . The second is 'name', with three values hidden in plain sight, and the third is `result` which is the value of the thing actually measured for each person and treatment.

For human reading purposes, we don't need to state the variables explicitly, we can see them by interpolating between the columns and rows and adding a bit of common sense. This mixing up of variables and values across tables like this has led some to call these tables 'messy'. A computer finds it hard to make sense of a messy table.

Working with R is made much less difficult if we get the data into a 'tidy' format. This format is distinct because each variable has its own column explicitly, like this  

<table><th>name</th><th>treatment</th><th>result</th>
<tr><td>John Smith</td><td>a</td><td>11</td></tr>
<tr><td>Jane Doe</td><td>a</td><td>16</td></tr>
<tr><td>Mary Johnson</td><td>a</td><td>3</td></tr>
<tr><td>John Smith</td><td>b</td><td>2</td></tr>
<tr><td>Jane Doe</td><td>b</td><td>11</td></tr>
<tr><td>Mary Johnson</td><td>b</td><td>1</td></tr>
</table>

Now each variable has a column, and each seperate observation of the data has its own row. It is _much_ more verbose for a human, but R can use this easily because we are now explicit about what is called what and how it relates to everything else.

More generally put, a tidy data set should look like this, schematically.

<div class="figure">
<img src="http://garrettgman.github.io/images/tidy-1.png" alt="from Garret Grolemund - http://garrettgman.github.io/tidying/"  />
<p class="caption">(\#fig:unnamed-chunk-1)from Garret Grolemund - http://garrettgman.github.io/tidying/</p>
</div>

  1. Each variable is in its own column
  2. Each observation is in its own row
  3. The value of a variable in an observation is in a single cell.
  
  
## A sample tidy data set

Let's use a tidy data set that comes with the tidyverse packages. The object `diamonds` is built in to `tidyr` and can be viewed by typing its name. We'll use the `head()` function to look at the top six rows only


```r
library(tidyverse)
head(diamonds)
```

```
## # A tibble: 6 x 10
##   carat cut       color clarity depth table price     x     y     z
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
## 2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
## 3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
## 4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
## 5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
## 6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
```

The output tells us that this is a thing called a `tibble` - this is just a table like object, more about these later. We can see the size of the tibble - 6 rows, 10 columns (this is truncated because of `head()` in reality its 53940 rows long). We can see the column headings and we can see the column type or, as this is called in R-speak, its class. 


### Class

Each of the columns has a particular type or class. Here class is either `<dbl>, <ord> or <int>`. This tells us what kind of data R is in that column. It's very important that you and R agree about what sort of data is in each column, otherwise the operations you run can go awry.

Thankfully there are only a few main classes to worry about

  * `num` or `int` or `dbl` - number types
  * `chr` - regular text
  * `fctr`  - A factor. A category or names for groups. Discrete values. 
  * `lgl` - `TRUE` or `FALSE` data. Can only have these two values.
  
Numeric, logical and character are pretty self explanatory. Factors need a bit more thinking about.

#### Factors

A factor is a variable that can only take pre-known values called levels. Often these will be experimental categories or groups. Usually you will know the values of the level before you even start an experiment. A treatment of a plant with different chemicals could be a factor. Its levels would be names for each treatment studied. E.G `GiberellicAcid`, `Jasmonate` or `Auxin`. Note a factor isn't restricted to describing inputs. In the same way, the sort of response of a plant to a treatment could be a factor, so `high`,`low`, `hypersensitive` could all be levels of an output factor variable in an infection assay. 

A factor can have numeric-looking levels. Treatment or response can often be labelled `1`, `2`, `3` etc, but they are used as categories, not actual measurements or numbers in factors. If the values can be replaced by e.g `A`, `B`, `C` without loss of sense, then the variable is a category and should be encoded as a factor.  

In our `diamonds` data set, the `cut`, `color` and `clarity` variables are factors - they just happen to be a particular sort of `ordered factor`. 

Factors are what we will group and split our data sets by. We will do statistics, plots and comparisons based on numbers within factor levels.

#### Checking Class Explicitly

The `tibble` table-like object of our `diamonds` data does a good job of summarising type. R has some commands for this too.

`class()` will give you the class(es) of a specific variable (we can use the `$` notation to get a single column out of a table-like object such as a `tibble`)


```r
class(diamonds$cut)
```

```
## [1] "ordered" "factor"
```

`levels()` will tell you all the levels of a factor


```r
levels(diamonds$cut)
```

```
## [1] "Fair"      "Good"      "Very Good" "Premium"   "Ideal"
```

`str()` will give you a summary of whole table-like objects


```r
str(diamonds)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	53940 obs. of  10 variables:
##  $ carat  : num  0.23 0.21 0.23 0.29 0.31 0.24 0.24 0.26 0.22 0.23 ...
##  $ cut    : Ord.factor w/ 5 levels "Fair"<"Good"<..: 5 4 2 4 2 3 3 3 1 3 ...
##  $ color  : Ord.factor w/ 7 levels "D"<"E"<"F"<"G"<..: 2 2 2 6 7 7 6 5 2 5 ...
##  $ clarity: Ord.factor w/ 8 levels "I1"<"SI2"<"SI1"<..: 2 3 5 4 2 6 7 3 4 5 ...
##  $ depth  : num  61.5 59.8 56.9 62.4 63.3 62.8 62.3 61.9 65.1 59.4 ...
##  $ table  : num  55 61 65 58 58 57 57 55 61 61 ...
##  $ price  : int  326 326 327 334 335 336 336 337 337 338 ...
##  $ x      : num  3.95 3.89 4.05 4.2 4.34 3.94 3.95 4.07 3.87 4 ...
##  $ y      : num  3.98 3.84 4.07 4.23 4.35 3.96 3.98 4.11 3.78 4.05 ...
##  $ z      : num  2.43 2.31 2.31 2.63 2.75 2.48 2.47 2.53 2.49 2.39 ...
```

## Quiz

1. How many levels in the factor `color` in the `diamonds` data?
2. Is the table below 'tidy'? 


country        year  type               count
------------  -----  -----------  -----------
Afghanistan    1999  cases                745
Afghanistan    1999  population      19987071
Afghanistan    2000  cases               2666
Afghanistan    2000  population      20595360
Brazil         1999  cases              37737
Brazil         1999  population     172006362
Brazil         2000  cases              80488
Brazil         2000  population     174504898
China          1999  cases             212258
China          1999  population    1272915272
China          2000  cases             213766
China          2000  population    1280428583

3. How many variables are contained in the table - how many columns should there be for it to be tidy?

