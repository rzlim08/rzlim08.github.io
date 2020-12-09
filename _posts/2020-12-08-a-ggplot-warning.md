---
layout: post
title: "xlim, ylim, and boxplots"
categories: R, programming
---

The R library ggplot2 is my favourite tool for data visualization. Usually I go on and on about how intuitive ggplot is, but today I learned of one of the most unintuitive things about ggplot2. Apparently, changing the limits of a plot using xlim, ylim, or scale_\* will change the underlying computation of the plot. 

Now normally, this doesn't matter at all, since most of what I do with ggplot are descriptive statistics, but today I was trying to figure out why a boxplot I made wasn't showing the median that I was calculating separately, and found this was the issue. Looking back at the documentation, this is noted -- albeit a bit vaguely -- by saying "remove data outside the limits and this can produce unintended results". 

There's nothing wrong with this behaviour, but it just surpises me that:

1. The default way of calculating limits removes points before plotting. Intuitively if I wanted to restrict the calculation of the plot, I would restrict the data, not the plot. 

2. For some reason or another, the proper way of setting limits on a plot *without* dropping points is called "coord_cartesian". Why the hell is it called that? 
Anyway, I googled around, and with the keywords "xlim, ylim, boxplot median" I didn't see any immediate results so I decided to write this. 
