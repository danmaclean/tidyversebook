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
##   [1]  0.84368004  0.35285698  0.35445367 -1.02731226  1.92092836
##   [6] -0.51132065  1.28579234 -0.65270677 -1.00838496  1.91479774
##  [11] -0.05281773 -1.15233007 -0.58062211 -0.73181320 -2.85127608
##  [16] -0.62375605  0.35318309  1.34454963  0.32602222  0.48793249
##  [21] -0.89198764  1.60298844  0.75631560  0.72005730  1.22152621
##  [26] -0.87902120 -0.25451959 -1.48765511 -0.19648967 -1.46542387
##  [31]  0.38336061  0.19502396 -2.37388417  2.05827325  0.29703603
##  [36] -1.40916253  1.44151776 -1.02461406 -0.51653155 -0.62671400
##  [41]  0.13228229  1.30441541  1.93743316 -0.51970107  0.22478188
##  [46] -1.60699371  0.28070378 -0.41080579 -1.22024541  0.12678092
##  [51] -0.47806634 -0.40838962 -0.25215878 -0.46714442  0.05837852
##  [56]  0.41328658 -0.41283564 -1.14149272  0.26870352 -0.07873107
##  [61]  1.33042729  2.13125065  1.72058181 -0.97245659 -0.58720063
##  [66] -0.25422615  0.57026105 -0.04053965  0.85878724 -1.47253010
##  [71] -0.31991196 -1.02701336  0.06029663  0.15862482 -0.18916261
##  [76]  0.01494583 -0.85277153  0.11451737  1.44666276  0.72773602
##  [81] -1.81265358  0.24555348 -1.50512356  0.63383767  0.65525022
##  [86]  0.01658964 -0.63733583 -0.34539981  1.33020892  0.91420719
##  [91] -0.40442740  0.07376972  0.58800412  0.32771975 -0.14652743
##  [96] -0.53337855 -0.40693021  0.15159049 -1.36417239  0.21643736
```

We can combine objects, variables and functions to do more complex stuff in R, here's how we get the mean of 100 random numbers.


```r
numbers <- rnorm(100)
mean(numbers)
```

```
## [1] -0.1683675
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
