---
title: 'Surviving #TidyTuesday'
author: Jesse Mostipak
date: '2021-05-31'
slug: 
categories: 
  - tutorial
tags: 
  - R
  - #TidyTuesday
  - Survivor
  - scatterplot
summary: "We'll wrangle this week's #TidyTuesday Survivor data using {lubridate}, mutate(), and pivot_longer(), and then create our beginner-level scatterplot."
---
### The hardest part about Survivor
This week's [#TidyTuesday](https://github.com/rfordatascience/tidytuesday) dataset focused on the reality show [Survivor](https://en.wikipedia.org/wiki/Survivor_(American_TV_series)). 
I didn't know much about the show when the data dropped, so I spent a couple of days watching the first half of Season 7 to get acclimated. 
At this point I'm convinced that the hardest part of Survivor isn't being in a remote location, or doing physical challenges, or even having to hunt your own food -- it's quickly and efficiently figuring out the ever-shifting political landscape of the strangers you find yourself with. 

If you're interested in the full two-hour [stream](https://www.twitch.tv/videos/1041901196) it's up on Twitch!

### Digging into the data
Because I'm focused on creating beginner-level content, which I define as content that someone new-ish to R could look at and recognize it from a `{ggplot}` reference (such as the [ggplot cheat sheet](https://www.rstudio.com/resources/cheatsheets/) or [R for Data Science](https://r4ds.had.co.nz/) text) and feel confident that they could _also_ recreate this plot, I had to make some choices in how to approach this data.  

The GitHub repo indicates that there are a multitude of datasets available to us, which presents the opportunity to go over joins while also creating a richer dataset. 
However the initial `summary` dataset has some relatively straightforward wrangling steps that can be tricky for beginners, and in the end this is where I chose to focus, since we could look at:

* converting all character data into factors
* using `pivot_longer()` 
* creating time intervals using `{lubridate}`

So without further ado, I've got two videos and the associated code for you below. 
The first video goes through the wrangling steps, and the second video picks up with the scatterplot we created.  

### Wrangling the Survivor `summary` dataset
**Video walkthrough:**  
<iframe width="560" height="315" src="https://www.youtube.com/embed/OEweA4yVAuI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

**And the code:**  
Set up our environment:

```r
library(tidyverse)
library(lubridate)
```

Import our data:

```r
summary <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-06-01/summary.csv')
```

Explore wrangling options:

```r
# converting characters to factors
summary %>% 
  mutate(across(where(is.character), as.factor)) %>% 
  glimpse()

# calculating intervals: days filmed, days aired
summary %>% 
  mutate(days_filming = time_length(interval(filming_started, filming_ended), unit = "day")) %>% 
  mutate(days_aired = time_length(interval(premiered, ended), unit = "day")) %>% 
  select(-premiered:-filming_ended) %>% 
  glimpse()

# pivoting data - combining metrics for show type
summary %>% 
  pivot_longer(
    cols = viewers_premier:viewers_reunion,
    names_to = "show_type",
    names_prefix = "viewers_",
    values_to = "total_views",
    values_drop_na = TRUE
  ) %>% 
  glimpse()
```

Our wrangling works, so let's go ahead and remove the `glimpse()` functions, put all of the steps together, and assign everything to a new variable, `summary_tidy`:

```r
summary_tidy <- summary %>% 
  # convert character cols to factors
  mutate(across(where(is.character), as.factor)) %>% 
  # calculate days filmed
  mutate(days_filming = time_length(interval(filming_started, filming_ended), unit = "day")) %>% 
  # calculate days aired
  mutate(days_aired = time_length(interval(premiered, ended), unit = "day")) %>% 
  # remove our four original date columns
  select(-premiered:-filming_ended) %>% 
  # pivot dataset
  pivot_longer(
    cols = viewers_premier:viewers_reunion,
    names_to = "show_type",
    names_prefix = "viewers_",
    values_to = "total_views",
    values_drop_na = TRUE
  ) 

# let's not forget to check our work!
glimpse(summary_tidy)
```


### Creating a Survivor scatterplot
**Video walkthrough:**  
<iframe width="560" height="315" src="https://www.youtube.com/embed/alUhgBXcRHo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

**And the code:**  
This code builds directly off of our wrangling code, so be sure to run the above code before running this chunk, which creates our scatterplot:

```r
summary_tidy %>% 
  filter(show_type %in% c("premier", "finale")) %>% 
  ggplot(aes(x = viewers_mean, y = total_views)) +
  geom_point(aes(color = show_type))
```

![Scatterplot showing the average viewers by total views for the show Survivor, broken out by show type (finale or premiere). We see a positive linear relationship for both show types, although there are outliers present for the finale and premiere starting at around 25 average viewers (scale of 25 is unknown - could be millions?)](images/survivor_scatterplot.png)

### Next steps
There are a multitude of directions you can go with our resulting scatterplot, depending on what you're looking to dig into a little bit with R. 
My suggestions are to try:  

* adding in a title and updating the axis labels
* changing the theme of the plot
* adding regression lines to each of the datasets
* doing a deep dive into the outliers we see at the far right of the graph -- what makes those points unique? (This would also be a great starting point for additional visualizations as well as a blog post!)

_Photo by <a href="https://unsplash.com/@karim_manjra?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Karim MANJRA</a> on <a href="https://unsplash.com/s/photos/survivor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>_
  
