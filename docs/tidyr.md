# Tidying data

## About this chapter

1. Questions:
- How do I go from non-tidy to tidy data structures?
2. Objectives:
- Manipulating dataframes with the `tidyr` verbs
3. Keypoints:
- The `tidyr` package contains functions that affect the layout and structure of dataframes.

## tidyr

tidyr is a tool for manipulating layout of datasets. As part of the tidyverse it is loaded when you use `library(tidyverse)` but can be loaded on its own with `library(tidyr)`. tidyr has two main functions - `spread()` and `gather()`.


### Sample tidy datasets

Let's look at five sample tables that show the same data in different ways, only one of which counts as tidy.






```r
table1
```

```
## # A tibble: 6 x 4
##       country  year  cases population
##         <chr> <int>  <int>      <int>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3      Brazil  1999  37737  172006362
## 4      Brazil  2000  80488  174504898
## 5       China  1999 212258 1272915272
## 6       China  2000 213766 1280428583
```

```r
table2
```

```
## # A tibble: 12 x 4
##        country  year       type      count
##          <chr> <int>      <chr>      <int>
##  1 Afghanistan  1999      cases        745
##  2 Afghanistan  1999 population   19987071
##  3 Afghanistan  2000      cases       2666
##  4 Afghanistan  2000 population   20595360
##  5      Brazil  1999      cases      37737
##  6      Brazil  1999 population  172006362
##  7      Brazil  2000      cases      80488
##  8      Brazil  2000 population  174504898
##  9       China  1999      cases     212258
## 10       China  1999 population 1272915272
## 11       China  2000      cases     213766
## 12       China  2000 population 1280428583
```

```r
table3
```

```
## # A tibble: 6 x 3
##       country  year              rate
## *       <chr> <int>             <chr>
## 1 Afghanistan  1999      745/19987071
## 2 Afghanistan  2000     2666/20595360
## 3      Brazil  1999   37737/172006362
## 4      Brazil  2000   80488/174504898
## 5       China  1999 212258/1272915272
## 6       China  2000 213766/1280428583
```

```r
table4a
```

```
## # A tibble: 3 x 3
##       country `1999` `2000`
## *       <chr>  <int>  <int>
## 1 Afghanistan    745   2666
## 2      Brazil  37737  80488
## 3       China 212258 213766
```

```r
table4b
```

```
## # A tibble: 3 x 3
##       country     `1999`     `2000`
## *       <chr>      <int>      <int>
## 1 Afghanistan   19987071   20595360
## 2      Brazil  172006362  174504898
## 3       China 1272915272 1280428583
```

The tidy data is in `table1`. 

  * `table2` is not tidy because not every variable has its own column. The `count` column has values for `cases` and `population` and these are two different things. The `type` column _could_ be a variable on its own, but as used here its a way to mix up the count variable unneccesarily.
  * `table3` is not tidy because `rate` is currently a function of two variables - literally its printed as `cases/population`. The column `rate` should contain the result of `cases/population` and if we wanted to retain the `case` and `population` information it should be in its own column, like in `table1`
  * `table4a` and `table4b` aren't tidy, because the data are split over two tables and in each table the values of the `year` variable are split over multiple columns.
  
Let's work with each of these non-tidy datasets in turn to get them tidy.

## gather()

The most common problem is that in `table4a`, where the values of a variable are split over multiple columns. 


```r
table4a
```

```
## # A tibble: 3 x 3
##       country `1999` `2000`
## *       <chr>  <int>  <int>
## 1 Afghanistan    745   2666
## 2      Brazil  37737  80488
## 3       China 212258 213766
```

To tidy this, we need to use `gather()`, which needs three bits of information

  1. The names of the columns that represent values, i.e. the year value columns
  2. The name of the new variable to create, this is known as the `key`
  3. The name of the variable to put all the values from the table cells into. This is known as the `value`.
  
The function call goes like this
  

```r
table4a %>% 
  gather('1999', '2000', key = "year", value = "cases")
```

```
## # A tibble: 6 x 3
##       country  year  cases
##         <chr> <chr>  <int>
## 1 Afghanistan  1999    745
## 2      Brazil  1999  37737
## 3       China  1999 212258
## 4 Afghanistan  2000   2666
## 5      Brazil  2000  80488
## 6       China  2000 213766
```
Note how the columns we 'gathered' (`1999` and `2000`) have been dropped from the table completely. This little table is now tidy. 
We can do the same with `table4b` but this one has the value `population`


```r
table4b %>% 
  gather('1999', '2000', key = "year", value = "population")
```

```
## # A tibble: 6 x 3
##       country  year population
##         <chr> <chr>      <int>
## 1 Afghanistan  1999   19987071
## 2      Brazil  1999  172006362
## 3       China  1999 1272915272
## 4 Afghanistan  2000   20595360
## 5      Brazil  2000  174504898
## 6       China  2000 1280428583
```

To combine these together we can use `left_join()`.

```r
t4a <- table4a %>% 
  gather('1999', '2000', key = "year", value = "cases")
t4b <- table4b %>% 
  gather('1999', '2000', key = "year", value = "population")

left_join(t4a, t4b)
```

```
## Joining, by = c("country", "year")
```

```
## # A tibble: 6 x 4
##       country  year  cases population
##         <chr> <chr>  <int>      <int>
## 1 Afghanistan  1999    745   19987071
## 2      Brazil  1999  37737  172006362
## 3       China  1999 212258 1272915272
## 4 Afghanistan  2000   2666   20595360
## 5      Brazil  2000  80488  174504898
## 6       China  2000 213766 1280428583
```

Note we don't need to specify the `by.x` and `by.y`, since the two tables have column names in common - `left_join()` works that out and does the join automatically.


## spread()

The complementary function to `gather()` is `spread()` which takes variables squashed into one column and puts them into more as appropriate. This is useful for dealing with the `table2` case. `spread()` needs two bits of information

  * The column with the variable name, again the `key` column
  * The column that contains the values, again the `value` column
  

```r
table2 %>%
  spread(key = type, value = count)
```

```
## # A tibble: 6 x 4
##       country  year  cases population
## *       <chr> <int>  <int>      <int>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3      Brazil  1999  37737  172006362
## 4      Brazil  2000  80488  174504898
## 5       China  1999 212258 1272915272
## 6       China  2000 213766 1280428583
```

## separate()

The `seperate()` function turns one column into many by splitting the value whenever a particular character appears. Remember `table3`


```r
table3
```

```
## # A tibble: 6 x 3
##       country  year              rate
## *       <chr> <int>             <chr>
## 1 Afghanistan  1999      745/19987071
## 2 Afghanistan  2000     2666/20595360
## 3      Brazil  1999   37737/172006362
## 4      Brazil  2000   80488/174504898
## 5       China  1999 212258/1272915272
## 6       China  2000 213766/1280428583
```

We can separate that `rate` column into two - `cases` and `population`


```r
table3 %>%
  separate(rate, into = c("cases", "population"))
```

```
## # A tibble: 6 x 4
##       country  year  cases population
## *       <chr> <int>  <chr>      <chr>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3      Brazil  1999  37737  172006362
## 4      Brazil  2000  80488  174504898
## 5       China  1999 212258 1272915272
## 6       China  2000 213766 1280428583
```

By default `separate()` splits things on any non-numeric character. But we can be explicit with the `sep` argument.


```r
table3 %>%
  separate(rate, into = c("cases", "population"), sep = "/")
```

```
## # A tibble: 6 x 4
##       country  year  cases population
## *       <chr> <int>  <chr>      <chr>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3      Brazil  1999  37737  172006362
## 4      Brazil  2000  80488  174504898
## 5       China  1999 212258 1272915272
## 6       China  2000 213766 1280428583
```

This works just as well, but is useful if the computer makes a bad guess.

Note the column types of `cases` and `population`, they're down as `chr`. By default the type of the parent column is retained, but you can make `separate()` guess what type the new column is with the `convert` argument.


```r
table3 %>%
  separate(rate, into = c("cases", "population"), sep = "/", convert = TRUE)
```

```
## # A tibble: 6 x 4
##       country  year  cases population
## *       <chr> <int>  <int>      <int>
## 1 Afghanistan  1999    745   19987071
## 2 Afghanistan  2000   2666   20595360
## 3      Brazil  1999  37737  172006362
## 4      Brazil  2000  80488  174504898
## 5       China  1999 212258 1272915272
## 6       China  2000 213766 1280428583
```
And now, we're back to tidy data.



## unite()

The `unite()` function is the inverse of `separate()`, and combines multiple columns into a single one.

To demonstrate `unite()` we can make a new table, `table5` by using the `separate()` function on the `year` column in `table3`. Passing `sep` a number, tells it just to split that many characters into the string  

Here's `table5`

```r
table5 <- table3 %>%
  separate(year, into = c("century", "year"), sep = 2, convert = TRUE)
table5
```

```
## # A tibble: 6 x 4
##       country century  year              rate
## *       <chr>   <int> <int>             <chr>
## 1 Afghanistan      19    99      745/19987071
## 2 Afghanistan      20     0     2666/20595360
## 3      Brazil      19    99   37737/172006362
## 4      Brazil      20     0   80488/174504898
## 5       China      19    99 212258/1272915272
## 6       China      20     0 213766/1280428583
```

We can now re- `unite()` `table5`. The arguments for this function are just the name of the new column, and the columns to join


```r
table5 %>%
  unite(new, century, year)
```

```
## # A tibble: 6 x 3
##       country   new              rate
## *       <chr> <chr>             <chr>
## 1 Afghanistan 19_99      745/19987071
## 2 Afghanistan  20_0     2666/20595360
## 3      Brazil 19_99   37737/172006362
## 4      Brazil  20_0   80488/174504898
## 5       China 19_99 212258/1272915272
## 6       China  20_0 213766/1280428583
```

Here the default is to use an underscore `_` to join the values, but we can be explicit and use nothing with the `sep` argument


```r
table5 %>%
  unite(new, century, year, sep="")
```

```
## # A tibble: 6 x 3
##       country   new              rate
## *       <chr> <chr>             <chr>
## 1 Afghanistan  1999      745/19987071
## 2 Afghanistan   200     2666/20595360
## 3      Brazil  1999   37737/172006362
## 4      Brazil   200   80488/174504898
## 5       China  1999 212258/1272915272
## 6       China   200 213766/1280428583
```

## Quiz

Tidying data is hard! And it needs you to know your data quite well, which naturally takes time. Rather than quiz questions here, a worked example will give good benefit, so let's try one of those. Do the Case Study on page 163 of R for Data Science. If you don't have the print edition it is available online here [Case Study for Tidy Data](http://r4ds.had.co.nz/tidy-data.html#case-study).


