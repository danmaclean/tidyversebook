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
##   name  band   
##   <chr> <chr>  
## 1 Mick  Stones 
## 2 John  Beatles
## 3 Paul  Beatles
```

```r
band_instruments
```

```
## # A tibble: 3 x 2
##   name  plays 
##   <chr> <chr> 
## 1 John  guitar
## 2 Paul  bass  
## 3 Keith guitar
```

Note that the two dataframes have a column in common `name`. 

## Join functions

Join functions work to combine two dataframes side-by-side in some way. Usually they use one column as a base and add columns to that one from the other. 

### left_join()

The most common sort of join is the left join. This takes one dataframe, considers it to be on the left of the join and combines the second dataframe on to it, skipping rows in the right dataframe that have nowhere to join


```r
left_join( band_members, band_instruments, )
```

```
## Joining, by = "name"
```

```
## # A tibble: 3 x 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 Mick  Stones  <NA>  
## 2 John  Beatles guitar
## 3 Paul  Beatles bass
```

Note how the column in common `name` is used as the key through which to join and that the `band_member` `Keith` goes missing because it isn't in the `left` dataframe, which is the reference. 

### right_join()

`right_join()` is the complementary function. 


```r
right_join( band_members, band_instruments)
```

```
## Joining, by = "name"
```

```
## # A tibble: 3 x 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 John  Beatles guitar
## 2 Paul  Beatles bass  
## 3 Keith <NA>    guitar
```

See how this time `Keith` is retained as we're joining to the right table as the base, but as he has no entry in the left table, an `NA` is used to fill the missing value.

### inner_join()

`inner_join()` keeps only rows that are completely shared


```r
inner_join( band_members, band_instruments)
```

```
## Joining, by = "name"
```

```
## # A tibble: 2 x 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 John  Beatles guitar
## 2 Paul  Beatles bass
```

### full_join()

`full_join()` joins all rows as well as possible, generating `NA` as appropriate.


```r
full_join( band_members, band_instruments)
```

```
## Joining, by = "name"
```

```
## # A tibble: 4 x 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 Mick  Stones  <NA>  
## 2 John  Beatles guitar
## 3 Paul  Beatles bass  
## 4 Keith <NA>    guitar
```

### Joins with no common column names

What can we do when there is no common column names? Consider this variant of `band instruments`


```r
band_instruments2
```

```
## # A tibble: 3 x 2
##   artist plays 
##   <chr>  <chr> 
## 1 John   guitar
## 2 Paul   bass  
## 3 Keith  guitar
```

The `name` column is called artist - we can join by explicitly stating the column to join by


```r
left_join( band_members, band_instruments2, by = c("name" = "artist"))
```

```
## # A tibble: 3 x 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 Mick  Stones  <NA>  
## 2 John  Beatles guitar
## 3 Paul  Beatles bass
```

## Binding operations

These allow you to paste dataframes together.

`bind_rows()` sticks them together top-to-bottom.


```r
bind_rows(band_members, band_members)
```

```
## # A tibble: 6 x 2
##   name  band   
##   <chr> <chr>  
## 1 Mick  Stones 
## 2 John  Beatles
## 3 Paul  Beatles
## 4 Mick  Stones 
## 5 John  Beatles
## 6 Paul  Beatles
```

Note the column names need not be identical for this to work. `NAs` are propogated as required.


```r
bind_rows(band_members, band_instruments)
```

```
## # A tibble: 6 x 3
##   name  band    plays 
##   <chr> <chr>   <chr> 
## 1 Mick  Stones  <NA>  
## 2 John  Beatles <NA>  
## 3 Paul  Beatles <NA>  
## 4 John  <NA>    guitar
## 5 Paul  <NA>    bass  
## 6 Keith <NA>    guitar
```

`bind_cols()` sticks dataframes together side-by-side/

```r
bind_cols(band_members, band_instruments)
```

```
## # A tibble: 3 x 4
##   name  band    name1 plays 
##   <chr> <chr>   <chr> <chr> 
## 1 Mick  Stones  John  guitar
## 2 John  Beatles Paul  bass  
## 3 Paul  Beatles Keith guitar
```

Note how it doesn't do any sensible matching - it's just pasting them together. Repeated column names get modified. What happens if the dataframes aren't of equal length?


```r
data_4_rows <- tibble( names = letters[1:4], values = 1:4)
bind_cols(band_members, data_4_rows)
```

```
## Error: Argument 2 must be length 3, not 4
```

## Quiz

1. Examine the `table1` and `table4a` datasets. Combine  `table4a` to `table1` to create two new columns. Ensure the columns make sense and retain data integrity.
