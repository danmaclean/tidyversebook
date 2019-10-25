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
##   [1] -1.533127382 -0.608363176  0.979294630 -0.122304201  1.668435612
##   [6] -0.532733672  0.186836709 -1.842828442  0.480847925 -0.695840337
##  [11] -1.641340744  1.864366893 -0.954649967  0.479762280 -1.573813113
##  [16]  0.429169220 -1.925346368  0.503042310  1.237165446  1.163434154
##  [21]  1.121319042 -0.725446172  1.741341080  0.218961112  0.744866785
##  [26]  0.542152908  1.137548223  0.430739538 -1.672066717 -0.328940414
##  [31] -1.420819887  1.556740816 -0.269699685 -0.621195688 -1.837262094
##  [36]  0.162908375  0.270604683  0.576681663  2.246178601 -0.868656956
##  [41] -0.827962306  0.139511900 -0.824098425  1.056324489 -0.191096566
##  [46] -1.663872746  0.314859247  0.795459374  0.174500790  2.139564480
##  [51] -1.830003485 -0.086729190  0.883279626  0.145028287  0.256899139
##  [56] -1.116154098  0.232832617  1.176569465  1.331101882 -1.067502379
##  [61] -0.022588779  1.123849592 -1.130871256  0.123875335  0.049919871
##  [66]  1.665467748 -0.797476871  0.506973591  0.453017655 -0.748919302
##  [71]  0.008827853 -0.912861271 -0.098747102 -0.875347274 -1.116795594
##  [76] -0.215386121 -2.089748056  0.430742289 -0.265881135  0.931913412
##  [81] -1.264253993 -0.216495013 -0.658984234 -0.583295870 -0.372439335
##  [86]  2.254232792  0.279890232 -1.069739530 -0.068071998  0.695135124
##  [91]  0.286662128  0.568251499  0.560103728 -0.256913633 -1.908959726
##  [96]  1.378479042 -0.311665924 -1.074427273  1.173788862 -0.518433337
```

We can combine objects, variables and functions to do more complex stuff in R, here's how we get the mean of 100 random numbers.


```r
numbers <- rnorm(100)
mean(numbers)
```

```
## [1] 0.02873494
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
