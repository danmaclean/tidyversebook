# Loading data from files

## About this chapter

1. Questions:
- How do I get my data into R?
2. Objectives:
- Loading a '.csv' file
- Checking column contents
- Dealing with headers and column names
- Loading a specific sheet from a '.xlsx' file.
3. Keypoints:
- The `readr` and `readxl` packages contain functions for loading data from .csv and .xslx files. These functions help you to ensure that your data is loaded as you expect. 

## readr

readr is a tool for loading data into R. As part of the tidyverse it is loaded when you use `library(tidyverse)` but can be loaded on its own with `library(readr)`. We will use `readr` to load in data from a 'flat' `.csv` file. Most 


### read_csv()

The main function is `read_csv()` which can read a standard comma seperated values file from disk into an R dataframe. There are a few variants of `read_csv()` which may be appropriate for different sorts of `.csv` file, but they all work the same.

  * `read_csv2()` - reads semi-colon delimited files, which are commonly used where a comma is used as a decimal separator
  * `read_tsv()` - reads tab delimited files
  * `read_delim()` - reads files delimited by an arbitrary character
  
The first argument to `read_csv()` is the path to the file to read. Here I'll read a file on my Desktop that contains the diamonds data we've been using.




```r
read_csv("~/Desktop/diamonds.csv")
```

```
## Warning: Missing column names filled in: 'X1' [1]
```

```
## Parsed with column specification:
## cols(
##   X1 = col_integer(),
##   carat = col_double(),
##   cut = col_character(),
##   color = col_character(),
##   clarity = col_character(),
##   depth = col_double(),
##   table = col_double(),
##   price = col_integer(),
##   x = col_double(),
##   y = col_double(),
##   z = col_double()
## )
```

```
## # A tibble: 53,940 x 11
##       X1 carat cut       color clarity depth table price     x     y     z
##    <int> <dbl> <chr>     <chr> <chr>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1     1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
##  2     2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
##  3     3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
##  4     4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
##  5     5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
##  6     6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
##  7     7 0.24  Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
##  8     8 0.26  Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
##  9     9 0.22  Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
## 10    10 0.23  Very Good H     VS1      59.4    61   338  4     4.05  2.39
## # ... with 53,930 more rows
```

On loading we see a column specification, `read_csv()` has guessed at what the columns should be and made those types. Its fine for the most part, but some of those columns we'd prefer to be factors. We can set our own column specification to force the column types on loading. We only have to do the ones that `read_csv()` gets wrong. Specifically, lets fix `cut` and `color` to a `factor`. We can do that with the `col_types` argument.


```r
read_csv("~/Desktop/diamonds.csv",
    col_types = cols(
      cut = col_factor(NULL),
      color = col_factor(NULL)
    )
)
```

```
## Warning: Missing column names filled in: 'X1' [1]
```

```
## # A tibble: 53,940 x 11
##       X1 carat cut       color clarity depth table price     x     y     z
##    <int> <dbl> <fct>     <fct> <chr>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1     1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
##  2     2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
##  3     3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
##  4     4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
##  5     5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
##  6     6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
##  7     7 0.24  Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
##  8     8 0.26  Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
##  9     9 0.22  Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
## 10    10 0.23  Very Good H     VS1      59.4    61   338  4     4.05  2.39
## # ... with 53,930 more rows
```

### Parser functions

This works by assigning a parser function that returns a specific type to each column, here it's `col_factor()`.  There are parser functions for all types of data, and all of them can be used if `read_csv()` doesn't guess your data properly. We won't go into detail of all of them, just remember that if your numbers or dates or stuff won't load properly, there's a parser function that can help.


The parser functions all have their own arguments, so we can manipulate those. We can see the `NULL` argument being passed to `col_factor()` above, which means 'all values found should be used as levels of the factor'. This is a great default setting, but if we have a large file, it won't help us find unexpected values. 

Consider a situation where we are certain we should only have the values `Fair`, `Good` and `Very Good` for `cut` in our `diamonds` data. We can make the parser function check this for us and give a warning if it finds anything else. 


```r
read_csv("~/Desktop/diamonds.csv",
    col_types = cols(
      cut = col_factor(levels = c( "Fair" ,"Good", "Very Good")),
      color = col_factor(NULL)
    )
)
```

```
## Warning: Missing column names filled in: 'X1' [1]
```

```
## Warning in rbind(names(probs), probs_f): number of columns of result is not
## a multiple of vector length (arg 1)
```

```
## Warning: 35342 parsing failures.
## row # A tibble: 5 x 5 col     row col   expected           actual  file                     expected   <int> <chr> <chr>              <chr>   <chr>                    actual 1     1 cut   value in level set Ideal   '~/Desktop/diamonds.csv' file 2     2 cut   value in level set Premium '~/Desktop/diamonds.csv' row 3     4 cut   value in level set Premium '~/Desktop/diamonds.csv' col 4    12 cut   value in level set Ideal   '~/Desktop/diamonds.csv' expected 5    13 cut   value in level set Premium '~/Desktop/diamonds.csv'
## ... ................. ... ................................................................. ........ ................................................................. ...... ................................................................. .... ................................................................. ... ................................................................. ... ................................................................. ........ .................................................................
## See problems(...) for more details.
```

```
## # A tibble: 53,940 x 11
##       X1 carat cut       color clarity depth table price     x     y     z
##    <int> <dbl> <fct>     <fct> <chr>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1     1 0.23  <NA>      E     SI2      61.5    55   326  3.95  3.98  2.43
##  2     2 0.21  <NA>      E     SI1      59.8    61   326  3.89  3.84  2.31
##  3     3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
##  4     4 0.290 <NA>      I     VS2      62.4    58   334  4.2   4.23  2.63
##  5     5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
##  6     6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
##  7     7 0.24  Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
##  8     8 0.26  Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
##  9     9 0.22  Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
## 10    10 0.23  Very Good H     VS1      59.4    61   338  4     4.05  2.39
## # ... with 53,930 more rows
```
This time, we get a large number of warnings. Though the output is quite cryptic at first glance, `read_csv()` is complaining that it found values for cut that were not in the list we passed to the parser function. 

Hence we can use parsers to ensure we are loading in the data we expect and generate errors if not.


### Headers and column names

By default `read_csv()` uses the first line of the file for column names. Consider this toy example.


```r
toy_csv <- 
"a,b,c
1,2,3
4,5,6"
read_csv(toy_csv)
```

```
## # A tibble: 2 x 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

The first line of the toy file becomes the column headings. This may not be appropriate, since there could be some metadata in the file


```r
toy_csv <- 
"some info about stuff
a,b,c
1,2,3
4,5,6"
read_csv(toy_csv)
```

```
## Warning in rbind(names(probs), probs_f): number of columns of result is not
## a multiple of vector length (arg 1)
```

```
## Warning: 3 parsing failures.
## row # A tibble: 3 x 5 col     row col   expected  actual    file         expected   <int> <chr> <chr>     <chr>     <chr>        actual 1     1 <NA>  1 columns 3 columns literal data file 2     2 <NA>  1 columns 3 columns literal data row 3     3 <NA>  1 columns 3 columns literal data
```

```
## # A tibble: 3 x 1
##   `some info about stuff`
##   <chr>                  
## 1 a                      
## 2 1                      
## 3 4
```

The loaded data gets really messed up, so we can skip a set number of lines if needed


```r
toy_csv <- 
"some info about stuff
a,b,c
1,2,3
4,5,6"
read_csv(toy_csv, skip = 1)
```

```
## # A tibble: 2 x 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

Alternatively, you might have comments that begin with a particular character. You can use the `comment` argument to skip those lines


```r
toy_csv <- 
"#some info about stuff
#some more info
#goodness, lots of INFO
a,b,c
1,2,3
4,5,6"
read_csv(toy_csv, comment = "#")
```

```
## # A tibble: 2 x 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

The data might not have any column names at all, the first row may data. This situation is handled with `col_names` argument


```r
toy_csv <- 
"1,2,3
4,5,6"
read_csv(toy_csv, col_names = FALSE)
```

```
## # A tibble: 2 x 3
##      X1    X2    X3
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

So, `read_csv()` sets up arbitrary column names. We can specify column names if we wish


```r
toy_csv <- 
"a,b,c
1,2,3
4,5,6"
read_csv(toy_csv, col_names = c("x", "y", "z"))
```

```
## # A tibble: 3 x 3
##   x     y     z    
##   <chr> <chr> <chr>
## 1 a     b     c    
## 2 1     2     3    
## 3 4     5     6
```

### Missing values

There are many different ways of encoding missing values, you can tell `read_csv()` which character represents a missing value explicitly with the `na` argument. These values will all be loaded as proper `NA`. 


```r
toy_csv <- 
"a,b,c
1,_,3
4,5,_"
read_csv(toy_csv, na = "_")
```

```
## # A tibble: 2 x 3
##       a     b     c
##   <int> <int> <int>
## 1     1    NA     3
## 2     4     5    NA
```

## Writing Files

A complementary functionto `read_csv()` `write_csv()` allows you to write a dataframe out to a '.csv' file. The convention is straightforward, you need the name of the dataframe and the name of the file and path to write to.


```r
write_csv(diamonds, "~/Desktop/my_data.csv")
```


## readxl

The `readxl` package is installed as part of the tidyvers `install.packages()` command, but it is not part of the core, so `library(tidyverse)` does not load it. You must do it explicitly with `library(readxl)`. 

### read_excel()

The main function is `read_excel()`, it's similar to `read_csv()`. 


```r
library(readxl)
read_excel("~/Desktop/datasets.xls")
```

```
## # A tibble: 150 x 5
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>  
##  1          5.1         3.5          1.4         0.2 setosa 
##  2          4.9         3            1.4         0.2 setosa 
##  3          4.7         3.2          1.3         0.2 setosa 
##  4          4.6         3.1          1.5         0.2 setosa 
##  5          5           3.6          1.4         0.2 setosa 
##  6          5.4         3.9          1.7         0.4 setosa 
##  7          4.6         3.4          1.4         0.3 setosa 
##  8          5           3.4          1.5         0.2 setosa 
##  9          4.4         2.9          1.4         0.2 setosa 
## 10          4.9         3.1          1.5         0.1 setosa 
## # ... with 140 more rows
```

By default it loads the first worksheet, you can examine the sheets available with `excel_sheets()`


```r
excel_sheets("~/Desktop/datasets.xls")
```

```
## [1] "iris"     "chickwts" "mtcars"   "quakes"
```

Then load in the one you want.


```r
read_excel("~/Desktop/datasets.xls", sheet = "chickwts")
```

```
## # A tibble: 71 x 2
##    weight feed     
##     <dbl> <chr>    
##  1    179 horsebean
##  2    160 horsebean
##  3    136 horsebean
##  4    227 horsebean
##  5    217 horsebean
##  6    168 horsebean
##  7    108 horsebean
##  8    124 horsebean
##  9    143 horsebean
## 10    140 horsebean
## # ... with 61 more rows
```

Loading then follows the same pattern as for `read_csv()`, with a difference in the column specifications - in this package its much simpler. You can only specify type columnwise and the specification can only be one of "skip", "guess", "logical", "numeric", "date", "text" or "list" - meaning you can't do the advanced parsing as for `read_csv()`.

A sample spec might look like


```r
read_excel(
  "~/Desktop/datasets.xls", 
  sheet = "chickwts",
  col_types = c("numeric", "text")
  )
```

```
## # A tibble: 71 x 2
##    weight feed     
##     <dbl> <chr>    
##  1    179 horsebean
##  2    160 horsebean
##  3    136 horsebean
##  4    227 horsebean
##  5    217 horsebean
##  6    168 horsebean
##  7    108 horsebean
##  8    124 horsebean
##  9    143 horsebean
## 10    140 horsebean
## # ... with 61 more rows
```
