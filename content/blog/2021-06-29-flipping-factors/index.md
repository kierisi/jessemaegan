---
title: 'Flipping factors: creating a sorted bar chart'
author: Jesse Mostipak
date: '2021-06-29'
slug: flipping-factors
categories:
  - tutorial
tags:
  - TidyTuesday
  - bar chart
  - ggplot
  - dataviz
subtitle: "Factors are not always as fun as you think they're going to be"
excerpt: "Code, video, and an explanation of the stream plot created as part of June 28th's #TidyTuesday Unfiltered Twitch stream!"
---
## Y'all.
Just when I think I've wrapped my head around factors a [#TidyTuesday dataset](https://github.com/rfordatascience/tidytuesday) drops that forces me to stare into space and question whether or not I even know how to code. 
Thankfully [Twitch chat](https://www.twitch.tv/videos/1070829066) was _there_ for me, and _because_ our community is so awesome I have a walkthrough of the code we used to create a sorted bar chart showing the total number of animals rescued in London from 2009--2021. 
There's _a lot_ of background knowledge required to grok what's happening with our wrangling code. 
If this looks unfamiliar don't sweat it too much -- I'm working on getting out some highlight videos that will go more in-depth with the wrangling steps.  

## Watch the walkthrough
<iframe width="560" height="315" src="https://www.youtube.com/embed/_BTkHbVrcfk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Set up our environment 
You don't necessarily need the `{here}` package for this walk-through. 
I use it at the very end when I save the plot that we create, but if you _aren't_ coding within an RStudio Project you can leave it out.   


```r
library(tidyverse)
library(here)
```

## Import and inspect our data
I copied and pasted the data from the [Week 27 page](https://github.com/rfordatascience/tidytuesday/blob/master/data/2021/2021-06-29/readme.md) in the #TidyTuesday GitHub repo.  


```r
animal_rescues <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-06-29/animal_rescues.csv', guess_max = 7544)

glimpse(animal_rescues)
```

## Wrangling our data
The goals of this wrangling step are to: 

* correct the typo and put `cat` and `Cat` in the same category
* list `Budgie` and `Pigeon` data as `Bird`
* list all of the rows that contain the phrase "Unknown - *" as `Unknown`
* convert all of our character data to factors


```r
data <- animal_rescues %>% 
  mutate(animal_group_parent = recode(animal_group_parent, 
                                      cat = "Cat",
                                      Budgie = "Bird",
                                      Pigeon = "Bird"),
         animal_group_parent = case_when(str_detect(animal_group_parent, "Unknown") ~ "Unknown", 
                                         TRUE ~ animal_group_parent)) %>% 
  mutate(across(where(is.character), factor))

glimpse(data)
```

## Create our sorted bar chart


```r
data %>% 
  count(animal_group_parent) %>% 
  ggplot(aes(reorder(animal_group_parent, n), n, fill = animal_group_parent)) +
  geom_col(show.legend = FALSE) + 
  coord_flip() 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

## Save our plot


```r
ggsave(here("06_animal_rescues", "sorted_bar.png"))
```

## Next steps
As always this plot is meant to get you started on your #TidyTuesday submission for this week, and some natural extensions of what we created are to: 

* refine the axis labels and add a title
* use a custom color palette and//or apply a new theme
* explore a specific species
* look at how counts change by year
