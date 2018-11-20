# Motivation

Working with data is pretty much the foundation of all science. Every field, lab, and scientist; every research tool, machine and data type seems to have it's own favourite way of storing, searching, analysing, and reporting data. For the individual scientist trying to analyse some work, this can be a practical nightmare.

If you've ever needed to work with any non-trivial data sets, or even tried to re-analyse an old one, you'll know that working with data is a massive ... 

<div class="figure">
<img src="assets/matador.jpg" alt="A significant pain in the ..."  />
<p class="caption">(\#fig:unnamed-chunk-1)A significant pain in the ...</p>
</div>

I think you know what I'm trying to say. 

The aim of this book is to introduce you to some tools that can reduce the pain of data analysis. The techniques here won't get rid of all your problems - there's no magic bullet for that, but they will reduce them to a more manageable size. 

The techniques themselves take a bit of getting used to - there is a learning curve. But over all they're a much smaller pain and are worth it especially if you're going to re-analyse growing datasets or do similar analysis over again.

<div class="figure">
<img src="assets/baby_bull.jpg" alt="A smaller pain in the ..."  />
<p class="caption">(\#fig:unnamed-chunk-2)A smaller pain in the ...</p>
</div>

## Tidy data analysis

The method we'll look it is called `tidy` analysis. It's a set of ways of working that is supposed to make datasets easier to manipulate, analyse and visualise [^1]. The tools for doing this in `R` are encapsulated in a set of packages collectively called the tidyverse. In reality this is the separate packages `dplyr, tidyr, readr, ggplot2, tibble, and purr`. We'll get to know _some_ of them in this book.

## Aim of this book

We don't aim to make you an expert in, or show you every trick in the tidyverse. In honesty, we're only going to scratch the surface of what is available in that package. But what you do learn should be enough to allow you a fighting chance to apply tidy, reproducible data analysis principles to all your data sets in the future.  

## Organization of this book

In the first part of this book we'll look at tools for rationally and reproducibly getting answers from table-like datasets. We'll look at the `dplyr` R package that helps you split datasets into sub-groups, compute new data based on the data you already have, filter and reduce the data to useful parts. This part will be especially useful for developing statistics to compare groups and replicates and for generating plots that compare different parts of the data. In this part we'll work with built-in data sets


In the second part of this book, we're going to look at ways of loading in data from external sources and making your own data consistent, in the hope that it will help you to organize your own data and then work on it in a reproducible, tidy fashion.

## External Resources

Much of what is here is presented (sometimes in disparate fashion) in other resources. If you're looking for a different view on these techniques - this site [R for data science](http://r4ds.had.co.nz/) by the inventor of the tidyverse is the best place to go. It's available as a print version too.


[^1]: See Hadley Wickham's tidy data paper -  [http://vita.had.co.nz/papers/tidy-data.html](http://vita.had.co.nz/papers/tidy-data.html)
