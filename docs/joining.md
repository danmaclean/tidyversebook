# Combining Datasets

## About this chapter

1. Questions:
- How do I combine dataframes?
2. Objectives:
- Understanding keys
- Explore `join` functions
3. Keypoints:
- Dataframes get joined on key columns. The rows that are retained depends on the type of `join` performed


## Joining

Often you will want to combine data contained in more than one dataset. In this section we will look at the functions that help you do that.

### Key columns

The joining operation depends on the two datasets having some values in some column in common. The column in each dataset that allows you to combine columns is the key column. Consider these dataframes



```r
band_members
```

```
## # A tibble: 3 x 2
##    name    band
##   <chr>   <chr>
## 1  Mick  Stones
## 2  John Beatles
## 3  Paul Beatles
```

```r
band_instruments
```

```
## # A tibble: 3 x 2
##    name  plays
##   <chr>  <chr>
## 1  John guitar
## 2  Paul   bass
## 3 Keith guitar
```

The key column in `band_members` is `name` and in `band_instruments` it is `artist`.

## Join functions

The join functions in `dplyr` all have a common syntax. Here's `left_join()` as an example



```r
left_join(x, y, by.x = key_column_x, by.y = key_column_y)
```

The `x` is the first (or left) dataframe, the `y` is the second (or right) dataframe and `by.x` and `by.y` are the key columns to use.

### left_join()

The most common sort of join is the left join. This takes one dataframe, considers it to be on the left of the join and combines the second dataframe to it, skipping rows in the right dataframe that have nowhere to join


```r
left_join( band_members, band_instruments, by.x = name, by.y = artist )
```

```
## Joining, by = "name"
```

```
## # A tibble: 3 x 3
##    name    band  plays
##   <chr>   <chr>  <chr>
## 1  Mick  Stones   <NA>
## 2  John Beatles guitar
## 3  Paul Beatles   bass
```

Note how the `band_member` `Keith` goes missing because it isn't in the `x` (left) dataframe. Note also how the key column name has come from the `x` dataframe.

### right_join()

`right_join()` is the complementary function. 


```r
right_join( band_members, band_instruments, by.x = name, by.y = artist)
```

```
## Joining, by = "name"
```

```
## # A tibble: 3 x 3
##    name    band  plays
##   <chr>   <chr>  <chr>
## 1  John Beatles guitar
## 2  Paul Beatles   bass
## 3 Keith    <NA> guitar
```

See how this time `Keith` is retained as we're joining to the right table, but as he has no entry in the left table, an `NA` is used to fill the missing value.

### inner_join()

`inner_join()` keeps only rows that are completely shared


```r
inner_join( band_members, band_instruments, by.x = name, by.y = artist)
```

```
## Joining, by = "name"
```

```
## # A tibble: 2 x 3
##    name    band  plays
##   <chr>   <chr>  <chr>
## 1  John Beatles guitar
## 2  Paul Beatles   bass
```

### full_join()

`full_join()` joins all rows as well as possible, generating `NA` as appropriate.


```r
full_join( band_members, band_instruments, by.x = name, by.y = artist)
```

```
## Joining, by = "name"
```

```
## # A tibble: 4 x 3
##    name    band  plays
##   <chr>   <chr>  <chr>
## 1  Mick  Stones   <NA>
## 2  John Beatles guitar
## 3  Paul Beatles   bass
## 4 Keith    <NA> guitar
```

## Binding operations

These allow you to paste dataframes together.

`bind_rows()` sticks them together top-to-bottom


```r
bind_rows(band_members, band_members)
```

```
## # A tibble: 6 x 2
##    name    band
##   <chr>   <chr>
## 1  Mick  Stones
## 2  John Beatles
## 3  Paul Beatles
## 4  Mick  Stones
## 5  John Beatles
## 6  Paul Beatles
```

Note the column names need not be identical for this to work. `NAs` are propogated as required.


```r
bind_rows(band_members, band_instruments)
```

```
## # A tibble: 6 x 3
##    name    band  plays
##   <chr>   <chr>  <chr>
## 1  Mick  Stones   <NA>
## 2  John Beatles   <NA>
## 3  Paul Beatles   <NA>
## 4  John    <NA> guitar
## 5  Paul    <NA>   bass
## 6 Keith    <NA> guitar
```

`bind_cols()` sticks dataframes together side-by-side/

```r
bind_cols(band_members, band_instruments)
```

```
## # A tibble: 3 x 4
##    name    band name1  plays
##   <chr>   <chr> <chr>  <chr>
## 1  Mick  Stones  John guitar
## 2  John Beatles  Paul   bass
## 3  Paul Beatles Keith guitar
```

Note how it doesn't do any sensible matching - it's just pasting them together. Repeated column names get modified. What happens if the dataframes aren't of equal length?


```r
data_4_rows <- tibble( names = letters[1:4], values = 1:4)
bind_cols(band_members, data_4_rows)
```

```
## Error in cbind_all(x): Argument 2 must be length 4, not 3
```

## Quiz

1. Examine the `table1` and `table4a` datasets. Combine  `table4a` to `table1` to create two new columns. Ensure the columns make sense and retain data integrity.
