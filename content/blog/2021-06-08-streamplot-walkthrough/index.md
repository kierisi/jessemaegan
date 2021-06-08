---
title: Stream makes a streamplot
author: Jesse Mostipak
date: '2021-06-08'
slug: streamplot-walkthrough
categories:
  - walkthrough
  - reflection
tags:
  - R
  - streaming
  - ggstream
  - dataviz
  - Great Lakes fish
  - TidyTuesday
subtitle: 'Be careful with how you use `drop_na()` kids!'
excerpt: "Code, video, and an explanation of the stream plot created as part of June 7th's #TidyTuesday Unfiltered Twitch stream!"
images: ~
series: ~
layout: single
---

## Live streaming your coding mistakes to the world

A few weeks ago the thought of live streaming my coding – mistakes and all – to the world would have sent me screaming in the other direction.
But in a suprisingly short amount of time it’s become something that I find myself running *toward*.

The core of my educational philosophy is that learning is relational, that we learn better when we learn together, and that the quality of learning is affected by the relationships among learners as well as between the learner(s) and educator(s).
But I also recognize that a lot of my beliefs around learning being a community endeavor have been “for thee but not for me” – because of my ego.

Ego is a tough thing to battle, because if I make coding mistakes on my own and no one can see them, I get to preserve my ego.
No one knows that I made mistakes, and no one can judge the mistakes that I’ve made.

But the flip side of that is that I don’t always catch my mistakes, and I often spend *a lot* of time figuring out how to fix my mistakes.
And the cost?
Being vulnerable and opening up my ego to taking hits.

## Mistakes I made last night

There were soooo many!
SO MANY!
(Watching the [Fast and Furious 9 trailer](https://www.youtube.com/watch?v=aSiDu3Ywi8E&ab_channel=TheFastSaga) was not one of them though.)

I’m working on a “Today I learned” blog post summary, but here’s a quick list:

-   not being able to get a time series graph to work
-   liberally using `drop_na()` in all the wrong ways
-   allowing duplicate data through into my final plot
-   completely botching string filters
-   not knowing the difference between \`\` and "" when filtering

BUT!
Because I was learning out loud, on the fly, with a community, many of those mistakes were caught and handled immediately.
And I *learned* so much in such a short span of time – more than I would have if I sat down and tried to do it solo.

## Without further ado

Here’s a video walk through of the code used to generate the initial stream plot:
<iframe width="560" height="315" src="https://www.youtube.com/embed/E2nLPLFBghw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

And the code:

**setup**

``` r
library(tidyverse)
library(ggstream)
library(wesanderson)
```

**import**

``` r
fishing <- fishing <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-06-08/fishing.csv')
```

**graph**

``` r
fishing %>% 
  drop_na() %>% 
  group_by(year, lake) %>% 
  summarise(total_fish = sum(values)) %>% 
  ggplot(aes(x = year, y = total_fish, fill = lake)) +
  geom_stream() +
  scale_fill_manual(values = wes_palette("Darjeeling2")) +
  theme_minimal() 
```

**saving the plot**

``` r
ggsave("streamplot.png", device = "png")
```

## Next steps

There are *definitely* some issues with this plot!
For starters, the placement of `drop_na()` is going to eliminate data that we actually want to keep.

To see a great implementation of this, check out [Eugen Buehler’s](https://twitter.com/EugenBuehler) tweet:

{{% tweet "1402263889543319562" %}}

And as pointed out by [Christoph Nicault](https://twitter.com/cnicault) later in the thread, we need a `filter()` step before the sum, otherwise we’ll end up with duplicate data:

{{% tweet "1402274189789339662" %}}

As always, \#TidyTuesday Unfiltered is intended to get you *started* with a plot, but is never meant to be the final plot!
I hope you’re able to take this code and resources and run with them to create something fantastic, and please [tag me on Twitter](https://twitter.com/kierisi) when you do – I’d love to see what you create!

### Links to Resources

-   **[Isabella Velasquez’s blog](https://ivelasq.rbind.io/blog/other-geoms/)**, which contains the stream plot tutorial we used
-   **[Karthik Ram’s](https://github.com/karthik/wesanderson)** \`{wesanderson} color palette
-   **[\#TidyTuesday](https://github.com/rfordatascience/tidytuesday)** GitHub repo, which has this week’s data as well as all of the historical data
