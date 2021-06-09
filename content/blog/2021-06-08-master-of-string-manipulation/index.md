---
title: '"Master" of string manipulation'
author: Jesse Mostipak
date: '2021-06-08'
slug: []
categories:
  - learning
tags:
  - stringr
  - R
subtitle: "it's sarcasm, y'see?"
excerpt: "A quick note on using stringr functions within the filter() function."
images: ~
series: ~
layout: single
---
I have literally _never_ been able to remember how to use `{stringr}` functions within the `filter()` function. 
It doesn't come up often in my work, but every time it does I do a Google and then smack my forehead and go "oh yeeeeaaaaaaah." 

I can tell you exactly why it doesn't stick, too. 
In 99.99% of my `filter()` functions the pattern is something like this:


```r
data %>% 
  filter(column_name == value)
```

But when you use `filter()` with a `{stringr}` function, it looks like this:


```r
data %>% 
  filter(str_detect(column_name, "regular_expression_pattern"))
```

So of course literally _every_ time I want to use a `{stringr}` function with `filter()` I do this:


```r
data %>% 
  filter(column_name == str_detect("pattern"))
```

...and it never ends well.

So thank you (again and again and again) to the Twitch chat fam from last night walking me through this!

