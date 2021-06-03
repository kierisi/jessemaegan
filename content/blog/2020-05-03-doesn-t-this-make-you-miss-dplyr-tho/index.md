---
title: Doesn't this make you miss {dplyr} tho?
author: Jesse Mostipak
date: '2020-05-03'
slug: doesn-t-this-make-you-miss-dplyr-tho
categories: []
tags:
  - data science
  - learning
  - learning to learn
  - python
  - R
  - teaching yourself
header:
  caption: ''
  image: ''
  preview: yes
---

{{% tweet "1256454640901783553" %}}

Listen I’m as surprised as you.
I’m on record in multiple places talking about how much I love `dplyr`.
I’ve referred to it as “my favorite package” more than once, and have definitely said that you could pry it “from my cold, dead hands.”
`dplyr` is a fantastic package for data manipulation, and has been the primary workhorse in my analytics workflow for the past few years.

But now it’s time for `pandas` - the Python equivalent of `dplyr` - and something about `pandas` clicked *just enough* for me that I for once found myself working in Python and not missing R.

## “Different enough” is perfect for learning

Once I thought I could learn Portuguese and Spanish at the same time *(I know)*, and as I was talking with a friend from Brazil he just started laughing and said “You’ve used English, Spanish, and Portuguese all in a single sentence.”

What “clicked” for me in `pandas` was accepting it for what it was - the Python way of manipulating data.
And what I actually *like* about `pandas` is that it’s written differently enough than `dplyr` that the learning doesn’t get muddled.

For example, here’s the code I wrote in `pandas`:

``` python
top_oceania_wines = reviews.loc[reviews.country.isin(['Australia', 'New Zealand']) & (reviews.points >= 95)]
```

And if I had written it using `dplyr` I’d do something like:

``` r
top_oceania_wines <- reviews %>% 
  filter(country %in% c("Australia", "New Zealand"),
         points >= 95)
```

## Mental models

I believe that programming is a literacy, and as such, a lot of how we teach literacy can be used to teach programming.
This is part of what makes the `tidyverse` so powerful as an entry to programming in data science - its syntax readily lends itself to literacy parallels that we’re already familiar with.
`pandas` isn’t written in the same way, which means that what needed to happen for me to start understanding `pandas` was to start building up a mental model of how to “read” the code.

Whereas I’d read the above `dplyr` code as:

> Create a tibble named `top_oceania_wines` by taking the `reviews` tibble then filtering for Australia or New Zealand in the `country` column AND values of at least 95 in the `points` column.

my mind reads the `pandas` code as:

> Create a DataFrame named `top_oceania_wines` by returning the rows within the `country` column in the `reviews` DataFrame that match either Australia or New Zealand AND the rows within the `points` column in the `reviews` DataFrame that have a value of at least 95.

My `pandas` statement is still a work in progress, and more verbose than it likely needs to be, but illustrates how we’re getting to the same destination using different paths.

I’ve only just started to scratch the surface of `pandas`, but am starting to get a handle on things and am excited to see where it takes me!

*Photo by <a href="https://unsplash.com/@brett_jordan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Brett Jordan</a> on <a href="https://unsplash.com/s/photos/miss-you?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*
