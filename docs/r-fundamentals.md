# R Fundamentals

## About this chapter

1. Questions:
  - How do I use R?
2. Objectives:
  - Become familiar with R syntax
  - Understand the concepts of objects and assignment
  - Get exposed to a few functions
3. Keypoints:
  - R's capabilities are provided by functions
  - R users call functions and get results

## Working with R 

In this workshop we'll use R in the extremely useful RStudio software For the most part we'll work interactively, meaning we'll type stuff straight into the R console in RStudio (Usually this is a window on the left or lower left) and get our results there too (usually in the consoled or in a window on the right). That's what you see in panels like the ones below - first the thing to type into R, and below it, the calculated result from R. Let's look at how R works by using it for it's most basic job - as a calculator:


```r
 3 + 5
```

```
## [1] 8
```

```r
 12 * 2
```

```
## [1] 24
```

```r
 1 / 3
```

```
## [1] 0.3333333
```

```r
 12 * 2
```

```
## [1] 24
```

```r
  3 / 0
```

```
## [1] Inf
```


Fairly straightforward, we type in the expression and we get a result. That's how this whole book will work, you type the stuff in, and get answers out. It'll be easiest to learn if you go ahead and copy the examples one by one. Try to resist the urge to use copy and paste. Typing longhand really encourages you to look at what you're entering.

As far as the R ouput itself goes, it's really straightforward - its just the answer with a `[1]` stuck on the front. This `[1]` tells us how far through the output we are. Often R will return long lists of numbers and it can be helpful to have this extra information

##  Variables

We can save the output of operations for later use by giving it a name using the assignment symbol `<-`. Read this symbol as 'gets', so `x <- 5` reads as 'x gets 5'. These names are called variables, because the value they are associated with can change.

Let's give five a name, `x` then refer to the value 5 by it's name. We can then use the name in place of the value. In the jargon of computing we say we are assigning a value to a variable. 


```r
 x <- 5
 x
```

```
## [1] 5
```


```r
 x * 2
```

```
## [1] 10
```


```r
y <- 3
x * y
```

```
## [1] 15
```


This is of course of limited value with just numbers but is of great value when we have large datasets, as the whole thing can be referred to by the variable.


### Using objects and functions

At the top level, R is a simple language with two types of thing: functions and objects. As a user you will use functions to do stuff, and get back objects as an answer. Functions are easy to spot, they are a name followed by a pair of brackets
 like `mean()` is the function for calculating a mean. The options (or arguments) for the function go inside the brackets: 


```r
 sqrt(16)
```

```
## [1] 4
```


Often the result from a function will be more complicated than a simple number object, often it will be a vector (simple list), like from the `rnorm()` function that returns lists of random numbers


```r
 rnorm(100)
```

```
##   [1]  0.515854044  1.472992822 -0.412793530  1.621646466  1.196007519
##   [6]  0.132343115 -1.145485547 -1.916004108 -0.206269943 -0.454147203
##  [11]  2.236535141  0.388469514 -0.418549693  0.615842513 -1.476466956
##  [16] -0.167253227 -0.310726895 -0.652536708 -0.099624648 -0.407687992
##  [21]  1.293842554  0.508313194  0.259026249 -0.961425446  0.798007138
##  [26]  0.241715437 -0.030607369 -2.105797644  0.672481803 -0.906185618
##  [31]  0.691228531  1.339855742  0.436090762 -0.692379116 -0.928196046
##  [36] -0.458375696 -0.661131262  0.955506092 -0.805543778  2.000752735
##  [41] -1.541466982  0.672344576  0.761207723  0.275856324  0.194537125
##  [46]  0.928809352 -0.104186965 -0.223277928 -1.175515539 -1.353597111
##  [51] -0.910159583 -0.757294256  0.586951543 -0.958022525  1.048160553
##  [56] -1.830321692  0.070515734  0.872753968  1.703080531 -0.468827246
##  [61] -0.070789147  0.823663911 -0.023018284  0.168454928 -0.326497815
##  [66]  0.849036782  0.026270380 -0.806837232  1.272464237 -0.397672540
##  [71]  0.427409762  1.182538356 -0.781968402  1.719923268  2.892567678
##  [76] -0.163384551 -0.374762549 -0.933315267 -0.916025985  0.686061135
##  [81]  1.212119201 -0.004756012 -0.064718191  0.879234409  0.324299255
##  [86] -0.720668756 -0.544138714  0.262004284 -0.796883037  0.101953500
##  [91]  0.088229302 -0.139472348 -0.746665808  0.905685858 -0.132587887
##  [96] -0.965271258 -2.499579141 -0.704666783 -0.231994345  0.158550263
```

We can combine objects, variables and functions to do more complex stuff in R, here's how we get the mean of 100 random numbers.


```r
numbers <- rnorm(100)
mean(numbers)
```

```
## [1] 0.2089735
```

Here we created a vector object with `rnorm(100)` and assigned it to the variable `numbers`. We than used the `mean()` function, passing it the variable `numbers`. The `mean()` function returned the mean of the hundred random numbers.

>## Bracket notation in this document
> I'm going to use the following descriptions for the symbols `()`, `[]` and `{}`: 
>
> `()` are brackets,
> `[]` are square brackets
> `{}` are curly brackets


## Quiz
1. Create two variables, `a` and `b`: Add them. What happens if we change a and then re-add a and b?
2. We can also assign `a + b` to a new variable, `c`. How would you do this?
3. Try some R functions: `round()`, `c()`, `range()`, `plot()` hint: Get help on a function by typing `?function_name` e.g `?c()`. Use the `mean()` function to calculate the average age of everyone in your house (Invent a housemate if you have to).
