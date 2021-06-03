---
title: 'TIL: Last observation carried forward (LOCF) using fill()'
author: Jesse Mostipak
date: '2018-10-03'
slug: til-locf
categories: []
tags:
  - learning
  - learning to learn
  - R
header:
  caption: ''
  image: ''
---

## The problem

I've been working with preliminary student [State of Texas Assessments of Academic Readiness](https://tea.texas.gov/student.assessment/staar/) (STAAR) data pulled from the [Data Interaction portal](https://txreports.emetric.net/?domain=1&report=1). The data I'm interested in includes proficiency levels for every subject for every individual school, disaggregated by all available demographic splits.

Because downloading _all_ of the data will likely require building a webcrawler, I've started this project by working with a small subset of 20 Elementary and Middle Schools in the Dallas Independent School District (DISD) for the 2018 school year.

After importing the data one of the first things I noticed was the data in the `Group` column:

![](https://i.imgur.com/SEWEVFe.png)

What you're seeing is a school name, in this case ADELFA BOTELLO CALLEJO EL, followed by 20+ rows of STAAR data split by various demographics for a given grade (we're seeing the 3rd grade). After all of the demographic information for the _third_ grade, the school will be listed again, followed by additional demographic information for the _fourth_ grade. This continues for all grades for a given school before we get data for the _next_ school in our list.

***

## The thought process

My first step is always to tinker with things, and in this case I was hoping that maybe `spread()` would just magically solve the problem _(spoiler: it didn't)_. I threw a few other ideas into R, all of which returned nothing of use, so I went to one of my personal favorites - drawing out what I had, and I what I wanted:

![](https://i.imgur.com/TOHGpXN.jpg)

Drawing things out is a great exercise in creating a concrete representation of what you're trying to accomplish by slowing you down in a way that prevents you from spiraling out of control with a bunch of garbage code thrown at your console, while also giving you space to consider the various nuances of what you're trying to do. In this case I noticed that my school names were always in all caps, and got the sense that I was going to be doing some sort of repeated operation that was going to differentiate between a school name and a demographic. My guess was that I was going to need to use `purrr()` in some capacity, but nothing quite seemed right. So I asked for help.

I hate asking for help for a lot of reasons, but I always encourage others to ask for help. So in an effort to practice practicing what I preach, I spent about 30 minutes on working out a solution, come up with what I thought was an informed guess, and could articulate the problem well enough to ask for a nudge in the right direction. 

> **Remember:** _asking for help after 30 minutes of working on something is infinitely less painful than asking for help after 30 hours of working on something._

***

## The question

Asking your question in a way that gets you the help that you need is an important skill. So I took to Slack with the following:

![](https://i.imgur.com/rpQuwhM.png)

***

## The solution

Ultimately - through the guidance of those who have been here before - I created an additional column by doing a `left_join()` with my data and a dataset of school names. This created a new column that had school names listed when the school name was present in the original data set, but had `NA` values for demographic splits: 

![](https://i.imgur.com/nD6nqIT.png)

Using the `fill()` function from the `tidyr()` package, I was able to convince R to fill in all of the `NA` values in the newly-created `school_name` column by carrying forward the school name until a change was encountered:

```r
library(tidyr)

# generally speaking:
dataset %>% 
  fill(column_to_be_filled)

# what I used in this specific case
sample_cref %>% 
  fill(school_name)
```
This ultimately resulted in a much happier (although not yet tidy) data set:

![](https://i.imgur.com/GzOcRdk.png)

***

## Where you come in!

I've set [this example up in a GitHub repository](https://github.com/jmostipak/til_locf_blogpost), and I want to invite and encourage anyone who is interested in leveling up their GitHub and/or wrangling skills to build out this repo. There's nothing in here that you can break, destroy, or modify in a way that makes it unusable - in fact I'd encourage you to use this as a low-stakes repo to contribute to! 

If you're not sure where to start, consider updating the README, approach this problem a different way, continue tidying up the code, or even do a full-blown analysis! Whatever you choose, I'm happy to support good-faith contributions and help you out as you develop your open source contributing skills.

If you're new to GitHub, check out Jenny Bryan and Jim Hester's [happygitwithr.com](http://happygitwithr.com/).

_Photo by <a href="https://unsplash.com/@swimstaralex?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Alexander Sinn</a> on <a href="https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>_
