---
title: '3-2-1 Mario Kart! #TidyTuesday Unfiltered'
author: Jesse Mostipak
date: '2021-05-24'
slug: 
categories:
  - tutorial
subtitle: "Is Rainbow Road actually challenging? Or does it just have a reputation of being challenging?"
tags:
  - #TidyTuesday
  - R
  - streaming
  - boxplot
  - Mario Kart
summary: "Code and video walkthrough of the boxplot we created as part of the #TidyTuesday Unfiltered stream."
---

At my first data science job I worked with a guy who would throw his hands up and shout "We did it, Reddit!" any time something went really well. 
And last night was a _huge_ "We did it, Reddit!" moment for me, and I hope for many of you as well.   

My goal with streaming #TidyTuesday Unfiltered is to show what it's like to work with an unfamiliar dataset, make mistakes in front of others, learn on the fly, and build a sense of community while also creating beginner-friendly content. You can check out the inaugural stream [here](https://www.twitch.tv/videos/1034279289).

Looking to skip past the 90 minutes of cats and conversation and dive right into creating our boxplot? 
I've got you covered. 
Here's a 10-minute video walking through the code: 

<iframe width="560" height="315" src="https://www.youtube.com/embed/g8vTeHERNp4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

And the code!

### set up our environment

```r
library(tidyverse)
library(skimr)
```

### data import

```r
tuesdata <- tidytuesdayR::tt_load('2021-05-25')
records <- tuesdata$records
```

### initial exploration of dataset

```r
glimpse(records)

records %>% 
  skim()

records_tt <- records %>% 
  mutate(track = factor(track))
glimpse(records_tt)
```

### let's create our initial boxplot

```r
records_tt %>% 
  ggplot(aes(x = track, y = record_duration)) +
  geom_boxplot()
```

### add alpha 0.6 to geom_boxplot, swap x and y variables

```r
records_tt %>% 
  ggplot(aes(x = record_duration, y = track)) +
  geom_boxplot(alpha = 0.6)
```

### add a fill, where fill = type in aes() so we can see single vs triple laps

```r
records_tt %>% 
  ggplot(aes(x = record_duration, y = track, fill = type)) +
  geom_boxplot(alpha = 0.6)
```

### print a .png version of the graph

```r
ggsave("25-05-2021_mario_kart.png", last_plot(), device = "png")
```

### What is #TidyTuesday?

The [#TidyTuesday project](https://github.com/rfordatascience/tidytuesday) is a weekly data visualization challenge that asks community members to take a dataset, tidy it as needed, and create code and visualizations to share on [Twitter](https://twitter.com/search?q=%23tidyTuesday&src=typed_query). The visualizations are _phenomenal_ and well worth perusing each week.  

_Photo by <a href="https://unsplash.com/@ravipalwe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Ravi Palwe</a> on <a href="https://unsplash.com/s/photos/mario-kart?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>_
