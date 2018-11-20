# dplyr Verbs

## About this chapter

1. Questions:
  - How do I manipulate tidy data?
2. Objectives:
  - Understanding the pipe syntax
  - Working with the 6 main functions
  - Overview of helper functions
3. Keypoints:
  - Tidy data can be operated on with six main functions that can quickly split, apply summaries and combine sub-groups of data

## dplyr

dplyr (data plier) is a tool for manipulating datasets. As part of the tidyverse it is loaded when you use `library(tidyverse)` but can be loaded on it's own with `library(dplyr)`. dplyr is set up as a small grammar, it has five main verbs that help you form small 'sentences' to get to your result.

The verbs are: 

  * `select()` picks variables based on their names.
  * `filter()` picks cases based on their values.
  * `mutate()` adds new variables that are functions of existing variables
  * `summarise()` reduces multiple values down to a single summary.
  * `arrange()` changes the ordering of the rows.
  
  A sixth function `group_by()` allows you to operate on subsets of the data.

## Pipe Syntax

The first argument to all these functions is the tidy table-like object (we'll start calling these data frames from here), so for our diamonds data set, then we use


```r
filter(diamonds, ... ) # ... stands in for other arguments
```

If we want to perform more than one step in series, we end up in the situation of having to save our result with a new name and working from there. This gets cumbersome quickly...


```r
diamonds2 <- filter(diamonds, ...)
diamonds3 <- select(diamonds2, ...)
diamonds4 <- mutate(diamonds3, ...)
```

To avoid this, there is a pipe operator - `%>%`. The purpose of the pipe is to take the thing on its left, and use it as the first argument of the thing on its right. So we can change that mess to


```r
diamonds %>% filter( ... ) %>% select( ... ) %>% mutate( ... )
```

Which is much more readable. Further we can put the right hand side of the pipe on a new line, such that we can get a very easy to read pattern.


```r
diamonds %>%
  filter( ... ) %>%
  select( ... ) %>% 
  mutate( ... )
```

If you want to save the result, you'll just put the usual assign at the top 


```r
filtered_diamonds <- 
diamonds %>%
  filter( ... ) %>%
  select( ... ) %>% 
  mutate( ... )
```

## select()

`select()` is probably the simplest verb. It lets you select whole columns from the dataframe, discarding others. This is most useful for working with huge datasets of many columns, or for extracting bits for ease of printing or presentation.



```r
diamonds %>% 
  select(carat, cut) %>%
  head()
```

```
## # A tibble: 6 x 2
##   carat cut      
##   <dbl> <ord>    
## 1 0.23  Ideal    
## 2 0.21  Premium  
## 3 0.23  Good     
## 4 0.290 Premium  
## 5 0.31  Good     
## 6 0.24  Very Good
```

Shorthands include the `:` which lets you choose a range and `-` which can be read as `except` so leaves out the columns you state


```r
diamonds %>%
  select(depth:price) %>%
  head()
```

```
## # A tibble: 6 x 3
##   depth table price
##   <dbl> <dbl> <int>
## 1  61.5    55   326
## 2  59.8    61   326
## 3  56.9    65   327
## 4  62.4    58   334
## 5  63.3    58   335
## 6  62.8    57   336
```


```r
diamonds %>%
  select( -x, -y, -z)  %>%
  head()
```

```
## # A tibble: 6 x 7
##   carat cut       color clarity depth table price
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int>
## 1 0.23  Ideal     E     SI2      61.5    55   326
## 2 0.21  Premium   E     SI1      59.8    61   326
## 3 0.23  Good      E     VS1      56.9    65   327
## 4 0.290 Premium   I     VS2      62.4    58   334
## 5 0.31  Good      J     SI2      63.3    58   335
## 6 0.24  Very Good J     VVS2     62.8    57   336
```

You can select columns with helpers, 

  * `starts_with()`
  * `ends_with()`
  * `contains()`
  * `num_range()`
  
Here are examples.


```r
diamonds %>%
  select( starts_with("c")) %>%
  head()
```

```
## # A tibble: 6 x 4
##   carat cut       color clarity
##   <dbl> <ord>     <ord> <ord>  
## 1 0.23  Ideal     E     SI2    
## 2 0.21  Premium   E     SI1    
## 3 0.23  Good      E     VS1    
## 4 0.290 Premium   I     VS2    
## 5 0.31  Good      J     SI2    
## 6 0.24  Very Good J     VVS2
```

```r
diamonds %>%
  select( ends_with("e")) %>%
  head()
```

```
## # A tibble: 6 x 2
##   table price
##   <dbl> <int>
## 1    55   326
## 2    61   326
## 3    65   327
## 4    58   334
## 5    58   335
## 6    57   336
```

```r
diamonds %>%
  select( contains("l")) %>%
  head()
```

```
## # A tibble: 6 x 3
##   color clarity table
##   <ord> <ord>   <dbl>
## 1 E     SI2        55
## 2 E     SI1        61
## 3 E     VS1        65
## 4 I     VS2        58
## 5 J     SI2        58
## 6 J     VVS2       57
```
### rename()

Often when you're selecting columns to work on, you'll need to fix the names - `rename()` is useful for this. Let's fix that mis-spelled column


```r
diamonds %>% 
  rename( colour = color) %>%
  head()
```

```
## # A tibble: 6 x 10
##   carat cut       colour clarity depth table price     x     y     z
##   <dbl> <ord>     <ord>  <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1 0.23  Ideal     E      SI2      61.5    55   326  3.95  3.98  2.43
## 2 0.21  Premium   E      SI1      59.8    61   326  3.89  3.84  2.31
## 3 0.23  Good      E      VS1      56.9    65   327  4.05  4.07  2.31
## 4 0.290 Premium   I      VS2      62.4    58   334  4.2   4.23  2.63
## 5 0.31  Good      J      SI2      63.3    58   335  4.34  4.35  2.75
## 6 0.24  Very Good J      VVS2     62.8    57   336  3.94  3.96  2.48
```
## filter()

The `filter()` function lets you select rows (observations) from your data frame based on criteria you specify. Here I'll look for all rows with a value of `G` for the `color` variable (I'll also pipe the output to the `head()` function to view just the top of the output.)


```r
diamonds %>%
  filter( color == "G") %>%
  head()
```

```
## # A tibble: 6 x 10
##   carat cut       color clarity depth table price     x     y     z
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23 Very Good G     VVS2     60.4    58   354  3.97  4.01  2.41
## 2  0.23 Ideal     G     VS1      61.9    54   404  3.93  3.95  2.44
## 3  0.28 Ideal     G     VVS2     61.4    56   553  4.19  4.22  2.58
## 4  0.31 Very Good G     SI1      63.3    57   553  4.33  4.3   2.73
## 5  0.31 Premium   G     SI1      61.8    58   553  4.35  4.32  2.68
## 6  0.24 Premium   G     VVS1     62.3    59   554  3.95  3.92  2.45
```

The syntax is fairly clear, just pass the column you want to think about and the condition to keep the rows. Multiple conditions can be used and all must be true to keep a row.  


```r
diamonds %>%
  filter( color == "G", 
          cut == "Ideal" ) %>%
  head()
```

```
## # A tibble: 6 x 10
##   carat cut   color clarity depth table price     x     y     z
##   <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23 Ideal G     VS1      61.9    54   404  3.93  3.95  2.44
## 2  0.28 Ideal G     VVS2     61.4    56   553  4.19  4.22  2.58
## 3  0.7  Ideal G     VS2      61.6    56  2757  5.7   5.67  3.5 
## 4  0.74 Ideal G     SI1      61.6    55  2760  5.8   5.85  3.59
## 5  0.75 Ideal G     SI1      62.2    55  2760  5.87  5.8   3.63
## 6  0.71 Ideal G     VS2      62.4    54  2762  5.72  5.76  3.58
```

To make more complex queries, you'll need to combine comparisons and logical operators.

### Comparisons

R provides the following comparison operators 

 * `==` - strictly equal to
 * `!=` - not equal to
 * `>`, `<`, `>=`, `<=` - greater than, less than, greater or equal to, less or equal to

These all work as you might expect. Except for `==`.  Trying to use `==` on numbers with a decimal point is tricky because of rounding errors in the computer. See this: 


```r
(1 / 49) * 49 == 1
```

```
## [1] FALSE
```

This statement is asking 'is 1 divided by 49, multiplied by 49, equal to 1'. The computer says `FALSE` because the computer can't store infinite numbers of decimal places. The rounding error is extremely small (down to the last 16th decimal place) but it is there. To deal with this rounding error we use the `near()` function, which checks numbers are the same to about the 8th decimal place.  


```r
near( (1 / 49) * 49, 1)
```

```
## [1] TRUE
```

### Logical operators 

`filter()` uses combinations of R logical operators, these are `&` for `and`, `|` for `or` and `!` for `not`. You can build filters with these. Let's modify our query to find `color` `G` or `cut` `ideal`.


```r
diamonds %>%
  filter( color == "G" | cut == "Ideal" ) %>%
  head()
```

```
## # A tibble: 6 x 10
##   carat cut       color clarity depth table price     x     y     z
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23 Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
## 2  0.23 Ideal     J     VS1      62.8    56   340  3.93  3.9   2.46
## 3  0.31 Ideal     J     SI2      62.2    54   344  4.35  4.37  2.71
## 4  0.3  Ideal     I     SI2      62      54   348  4.31  4.34  2.68
## 5  0.23 Very Good G     VVS2     60.4    58   354  3.97  4.01  2.41
## 6  0.33 Ideal     I     SI2      61.8    55   403  4.49  4.51  2.78
```

Note that the computer doesn't read this like it's English. Consider this


```r
diamonds %>%
  filter( color == "G" | "F") %>%
  head()
```

You might consider this to read `filter rows with color column equal to `G` or `F`. The computer doesn't read it like this. It needs more explicit statements 


```r
diamonds %>%
  filter( color == "G" | color == "F") %>%
  head()
```

```
## # A tibble: 6 x 10
##   carat cut       color clarity depth table price     x     y     z
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.22 Premium   F     SI1      60.4    61   342  3.88  3.84  2.33
## 2  0.23 Very Good G     VVS2     60.4    58   354  3.97  4.01  2.41
## 3  0.23 Very Good F     VS1      60.9    57   357  3.96  3.99  2.42
## 4  0.23 Very Good F     VS1      60      57   402  4     4.03  2.41
## 5  0.23 Very Good F     VS1      59.8    57   402  4.04  4.06  2.42
## 6  0.23 Good      F     VS1      58.2    59   402  4.06  4.08  2.37
```

Which can be cumbersome if you want to filter on one of many possible values. For that reason we have `%in%` , which works like


```r
diamonds %>%
  filter( color  %in% c("G", "F") ) %>%
  head()
```

```
## # A tibble: 6 x 10
##   carat cut       color clarity depth table price     x     y     z
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.22 Premium   F     SI1      60.4    61   342  3.88  3.84  2.33
## 2  0.23 Very Good G     VVS2     60.4    58   354  3.97  4.01  2.41
## 3  0.23 Very Good F     VS1      60.9    57   357  3.96  3.99  2.42
## 4  0.23 Very Good F     VS1      60      57   402  4     4.03  2.41
## 5  0.23 Very Good F     VS1      59.8    57   402  4.04  4.06  2.42
## 6  0.23 Good      F     VS1      58.2    59   402  4.06  4.08  2.37
```

You can select anything not in a list given to `%in%` with a judicious `!` (not), again this is a bit weird if you translate directly from English, as the not goes first. 

```r
diamonds %>%
  filter( ! color %in% c("G", "F") ) %>%
  head()
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

## mutate()

The `mutate()` function lets you add new columns based on values in other columns. Note that doing so to this data set makes it too big to print, so I'll `select()` the appropriate columns. 


```r
diamonds %>%
  mutate(price_per_carat = price / carat) %>%
  select(price, carat, price_per_carat) %>%
  head()
```

```
## # A tibble: 6 x 3
##   price carat price_per_carat
##   <int> <dbl>           <dbl>
## 1   326 0.23            1417.
## 2   326 0.21            1552.
## 3   327 0.23            1422.
## 4   334 0.290           1152.
## 5   335 0.31            1081.
## 6   336 0.24            1400
```

You can refer to columns straight after creating them, so you can minimise `mutate()`s


```r
diamonds %>%
  mutate(price_per_carat = price / carat) %>%
  mutate(depth_per_ppc = depth / price_per_carat) %>%
  select(depth_per_ppc, price_per_carat) %>%
  head()
```

```
## # A tibble: 6 x 2
##   depth_per_ppc price_per_carat
##           <dbl>           <dbl>
## 1        0.0434           1417.
## 2        0.0385           1552.
## 3        0.0400           1422.
## 4        0.0542           1152.
## 5        0.0586           1081.
## 6        0.0449           1400
```

### Functions in mutate()

You can create a new column with `mutate()` using pretty much any (vectorized) R function. It's a bit out of scope to explain what I mean by 'vectorized' but here's some examples.


```r
diamonds %>% 
  mutate(log_price = log(price)) %>%
  select(-x, -y, -z) %>% 
  head()
```

```
## # A tibble: 6 x 8
##   carat cut       color clarity depth table price log_price
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int>     <dbl>
## 1 0.23  Ideal     E     SI2      61.5    55   326      5.79
## 2 0.21  Premium   E     SI1      59.8    61   326      5.79
## 3 0.23  Good      E     VS1      56.9    65   327      5.79
## 4 0.290 Premium   I     VS2      62.4    58   334      5.81
## 5 0.31  Good      J     SI2      63.3    58   335      5.81
## 6 0.24  Very Good J     VVS2     62.8    57   336      5.82
```


```r
diamonds %>%
  mutate( total_price = sum(price)) %>%
  select( -x, -y, -z) %>%
  head()
```

```
## # A tibble: 6 x 8
##   carat cut       color clarity depth table price total_price
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int>       <int>
## 1 0.23  Ideal     E     SI2      61.5    55   326   212135217
## 2 0.21  Premium   E     SI1      59.8    61   326   212135217
## 3 0.23  Good      E     VS1      56.9    65   327   212135217
## 4 0.290 Premium   I     VS2      62.4    58   334   212135217
## 5 0.31  Good      J     SI2      63.3    58   335   212135217
## 6 0.24  Very Good J     VVS2     62.8    57   336   212135217
```

Observe how the same number is in all the rows in the last example, this highlights how this 'vectorized' function idea works. 

Briefly, vectorized functions work on whole columns at a time, not just single rows. So if it makes sense to treat each element of the column individually, the function will do that. Consider this column (here printed on its side) with the `log()` function.


```r
log(c(1,2,3))
```

```
## [1] 0.0000000 0.6931472 1.0986123
```

You get back a column of numbers the same length as you put in, each item logged. With the `sum()` function it makes sense to return the sum of all the numbers in the column.


```r
sum(c(1,2,3))
```

```
## [1] 6
```

So you get back a single number. The behaviour of `mutate()` then is akin to taking the whole column or columns you specify, apply whatever function you ask for and putting the resulting column in the dataframe. If the resulting column is too short, it just gets repeated until it fits. That is why we get repeats of the same number in the `sum()` example and why that number is the sum of all the prices.


### if_else()

One final vectorized function is `if_else()`, this is useful when you want to add a column that annotates the data with an arbitrary value based on values. Let's add a column called `cost` that can be `high` or `low` depending on the `price`. 


```r
diamonds %>%
  mutate( cost = if_else( price > 335, "high", "low")) %>%
  select( -x, -y, -z)
```

```
## # A tibble: 53,940 x 8
##    carat cut       color clarity depth table price cost 
##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <chr>
##  1 0.23  Ideal     E     SI2      61.5    55   326 low  
##  2 0.21  Premium   E     SI1      59.8    61   326 low  
##  3 0.23  Good      E     VS1      56.9    65   327 low  
##  4 0.290 Premium   I     VS2      62.4    58   334 low  
##  5 0.31  Good      J     SI2      63.3    58   335 low  
##  6 0.24  Very Good J     VVS2     62.8    57   336 high 
##  7 0.24  Very Good I     VVS1     62.3    57   336 high 
##  8 0.26  Very Good H     SI1      61.9    55   337 high 
##  9 0.22  Fair      E     VS2      65.1    61   337 high 
## 10 0.23  Very Good H     VS1      59.4    61   338 high 
## # ... with 53,930 more rows
```

The `if_else()` function then just adds the first value ("high") if the condition is 'true' else it puts the second value.

## summarize() and group_by()

The `summarize()` function is a reductive function. It reduces entire dataframes to a single row, and returns an entirely new dataframe.


```r
diamonds %>%
  summarize( mean_price = mean(price) )
```

```
## # A tibble: 1 x 1
##   mean_price
##        <dbl>
## 1      3933.
```

`summarize()` is best used with `group_by()` which helps split dataframes into subsets. The `summarize()` function will run once for each subset created by `group_by()`. Let's find the mean price for every colour of diamonds. We'll do this by grouping the `diamonds` dataframe on `color`, then summarising.


```r
diamonds %>%
  group_by(color) %>%
  summarize(mean_price = mean(price) ) 
```

```
## # A tibble: 7 x 2
##   color mean_price
##   <ord>      <dbl>
## 1 D          3170.
## 2 E          3077.
## 3 F          3725.
## 4 G          3999.
## 5 H          4487.
## 6 I          5092.
## 7 J          5324.
```

This is where the power of dplyr starts to be obvious. Once we've got our dataframe into shape with the `select()`, `filter()` and `mutate()` functions, we can start to compute new information with `group_by()` and `summarize()` and some of the helper functions.

Let's group by two things, `color` and `cut`, and get the mean and standard deviation of `price`.


```r
diamonds %>%
  group_by(color, cut) %>%
  summarize( 
    mean_price = mean(price),
    sd = sd(price)
    )
```

```
## # A tibble: 35 x 4
## # Groups:   color [?]
##    color cut       mean_price    sd
##    <ord> <ord>          <dbl> <dbl>
##  1 D     Fair           4291. 3286.
##  2 D     Good           3405. 3175.
##  3 D     Very Good      3470. 3524.
##  4 D     Premium        3631. 3712.
##  5 D     Ideal          2629. 3001.
##  6 E     Fair           3682. 2977.
##  7 E     Good           3424. 3331.
##  8 E     Very Good      3215. 3408.
##  9 E     Premium        3539. 3795.
## 10 E     Ideal          2598. 2956.
## # ... with 25 more rows
```

Note how every combination of `color` and `cut` is made into subsets. 

### Helpful summarize() functions

There are numerous helpful summary functions. We've already seen `mean()` , `sum()` and `sd()`.

The function `n()` counts the number of items in a group. It doesn't need a column name to work on. 


```r
diamonds %>%
  group_by(color) %>%
  summarize( 
     count = n()
    )
```

```
## # A tibble: 7 x 2
##   color count
##   <ord> <int>
## 1 D      6775
## 2 E      9797
## 3 F      9542
## 4 G     11292
## 5 H      8304
## 6 I      5422
## 7 J      2808
```

Related is `n_distinct()` which counts the number of unique values in a group. This one needs to know which column of things you want to use. Let's see how many observations there are for each `cut` and how many different `color`s are observed in each `cut`


```r
diamonds %>%
  group_by(cut) %>%
  summarize( 
     items = n(),
     unique_colors = n_distinct(color)
    )
```

```
## # A tibble: 5 x 3
##   cut       items unique_colors
##   <ord>     <int>         <int>
## 1 Fair       1610             7
## 2 Good       4906             7
## 3 Very Good 12082             7
## 4 Premium   13791             7
## 5 Ideal     21551             7
```

There are other helpful summary functions, here's a non-exhaustive list

  * `max()` or `min()`  - maximum or minimum value in a column
  * `median()` - median value in a column
  * `IQR()` - interquartile range (distance) between 25th and 75th percentile
  * `first()` or `last()` - first or last values in a column


## arrange()

The `arrange()` function is a straightforward function that helps you arrange the final table from `summarize()` a bit more nicely. It simply orders the rows in a way that you specify. 


```r
diamonds %>% 
  arrange(price) %>%
  head()
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

To sort biggest first, use `desc()`


```r
diamonds %>% 
  arrange(desc(price)) %>%
  head()
```

```
## # A tibble: 6 x 10
##   carat cut       color clarity depth table price     x     y     z
##   <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  2.29 Premium   I     VS2      60.8    60 18823  8.5   8.47  5.16
## 2  2    Very Good G     SI1      63.5    56 18818  7.9   7.97  5.04
## 3  1.51 Ideal     G     IF       61.7    55 18806  7.37  7.41  4.56
## 4  2.07 Ideal     G     SI2      62.5    55 18804  8.2   8.13  5.11
## 5  2    Very Good H     SI1      62.8    57 18803  7.95  8     5.01
## 6  2.29 Premium   I     SI1      61.8    59 18797  8.52  8.45  5.24
```

## Missing Values

Many (many!) datasets will have some missing values at some points. These are encoded in R as `NA`. They need to be dealt with explicitly as they mess up lots of calculations.

Look at the toy dataframe `incomplete` below




```r
incomplete
```

```
##   group size
## 1     A 10.4
## 2     B   NA
## 3     C  8.0
## 4     A  6.0
## 5     B   NA
## 6     C   NA
```
When we try to `summarize()` we get stuck


```r
incomplete %>%
  group_by( group ) %>%
  summarize(mean_size = mean(size))
```

```
## # A tibble: 3 x 2
##   group mean_size
##   <fct>     <dbl>
## 1 A           8.2
## 2 B          NA  
## 3 C          NA
```

The groups with any `NA` can't be calculated. We need to tell our helper function to remove `NA` before we work with it.


```r
incomplete %>%
  group_by( group ) %>%
  summarize(mean_size = mean(size, na.rm = TRUE))
```

```
## # A tibble: 3 x 2
##   group mean_size
##   <fct>     <dbl>
## 1 A           8.2
## 2 B         NaN  
## 3 C           8
```

This works! Though because the group `B` only had `NA` in it the formula for mean fails because we can't divide by `0` and we get `NaN` (not a number). 

You might think you can use `filter()` to get rid of any rows with NA in, but you get a weird result


```r
incomplete %>%
  filter(size != NA) %>%
  group_by( group ) %>%
  summarize(mean_size = mean(size))
```

```
## # A tibble: 0 x 2
## # ... with 2 variables: group <fct>, mean_size <dbl>
```

By definition `NA` means `Not available`, which is a nice way of saying `don't know`, so, strictly, `x == NA` means "is `x` equal to something we don't know the value of?" To which the answer can only be `don't know`, for which R uses `NA`.  The result is that any comparison with `NA` in it is `NA`. `filter()` doesn't know whether any row passes so throws it out. You get no rows for `group_by()` to group.

If you want to check something is an `NA`, you can use `is.na()`


```r
incomplete %>%
  filter(! is.na(size)) %>%
  group_by( group ) %>%
  summarize(mean_size = mean(size))
```

```
## # A tibble: 2 x 2
##   group mean_size
##   <fct>     <dbl>
## 1 A           8.2
## 2 C           8
```


Note that you lose the information for the `B` group, which may be important. 

You may want to pair these operations with an `n()` column to give you an idea of how many values you use to get your answer.


```r
incomplete %>%
  group_by( group ) %>%
  summarize(
    mean_size = mean(size, na.rm = TRUE),
    sample_size = n()
    )
```

```
## # A tibble: 3 x 3
##   group mean_size sample_size
##   <fct>     <dbl>       <int>
## 1 A           8.2           2
## 2 B         NaN             2
## 3 C           8             2
```

```r
incomplete %>%
  filter(! is.na(size)) %>%
  group_by( group ) %>%
  summarize(mean_size = mean(size),
    sample_size = n()
    
    )
```

```
## # A tibble: 2 x 3
##   group mean_size sample_size
##   <fct>     <dbl>       <int>
## 1 A           8.2           2
## 2 C           8             1
```

With a more complicated call, you can explicitly get the number of `NA`s. 


```r
incomplete %>%
  group_by( group ) %>%
  summarize(
    mean_size = mean(size, na.rm = TRUE),
    sample_size = n(),
    nas = sum(is.na(size))
    )
```

```
## # A tibble: 3 x 4
##   group mean_size sample_size   nas
##   <fct>     <dbl>       <int> <int>
## 1 A           8.2           2     0
## 2 B         NaN             2     2
## 3 C           8             2     1
```

## Quiz

1. Load the package `nycflights13` which is a tidy data set of flight information out of New York. Look at the `flights` table that has been loaded and note the column names and types. 

2. The individual planes are identified by their `tailnum`. Which plane has the worst on-time record?

3. What time of day should you fly to avoid delays as much as possible?

4. For each destination compute the total minutes of delay? 

5. Find all destinations that have at least two carriers. 

(These exercises are taken from pg 75 of [R for Data Science](http://r4ds.had.co.nz/) - check there for more challenges.)
