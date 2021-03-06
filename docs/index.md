---
title: Introduction to basic statistics with R
author: [Dr. Wesley Brooks]
output:
   rmdformats::readthedown
date: "2021-02-11"
url: "https://ucdavisdatalab.github.io/basic_stats_r_1/"
github-repo: ucdavisdatalab/basic_stats_r_1
site: bookdown::bookdown_site
knit: "bookdown::render_book"
---



![](img/datalab-logo-full-color-rgb.png)

# Overview
This workshop will introduce you to basic statistics using R. 


# Introduction
It is useful to begin with some concrete examples of statistical questions to motivate the material that we'll cover in the workshop. This will also help confirm that your R environment is working. These data files come from the `fosdata` package, which you can optionally install to your computer in order to get access to all the associated metadata in the help files. I will show loading the data from the workshop website.

```
# optional: install the fosdata package using remotes::install_github
install.packages( "remotes" )
remotes::install_github( "speegled/fosdata" )
```


```r
# load the mice_pot and barnacles datasets
mice_pot = read.csv( url( "https://raw.githubusercontent.com/wes-brooks/basic-stats/main/data/mice_pot.csv" ))
barnacles = read.csv( url( "https://raw.githubusercontent.com/wes-brooks/basic-stats/main/data/barnacles.csv" ))
```

## `mice_pot` dataset
The `mice_pot` dataset contains data on an experiment that dosed four groups of mice with different levels of THC. There was a low, medium, and high dosage group, as well as a control group that got no THC. The mice were were then observed for a while and their total movement was quantified as a percentage of the baseline group mean. Some statistical questions that might arise here are: were there differences in the typical amount of movement between mice of different groups? What was the average amount of movement by mice in the medium dose group?

Both of these questions can be approached in ways that relate to the particular sample, which is kind of interesting descriptions of the data. For instance, if you install the `dplyr` package in your R environment, we can make those calculations right now.


```r
with( mice_pot, by(percent_of_act, group, mean))
```

```
## group: 0.3
## [1] 97.3225
## ------------------------------------------------------------ 
## group: 1
## [1] 99.05235
## ------------------------------------------------------------ 
## group: 3
## [1] 70.66787
## ------------------------------------------------------------ 
## group: VEH
## [1] 100
```

The means aren't identical - there are clearly differences between all of the groups? Yes, **in terms of the sample**. But if you want to generalize your conclusion to cover what would happen to other mice that weren't in the study, then you need to think about the **population**. In this case, that's the population of all the mice that could have been dosed with THC.

Because we don't see we don't see data from mice that weren't part of the study, we rely on statistical **inference** to reach conclusions about the population. How is that possible? Well, some theorems from **mathematical statistics** can tell us about the **distribution** of the sample, relative to the population

## `barnacles` dataset
This dataset was collected by counting the barnacles in 88 grid squares on the Flower Garden Banks coral reef in the Gulf of Mexico. The counts were normalized to barnacles per square meter. Some questions that you might approach with statistical methods are, what is the average number of barnacles per square meter, and is it greater than 300?


```r
mean( barnacles$per_m )
```

```
## [1] 332.0186
```

From that calculation, we see that the mean is 332 barnacles per square meter, which is greater than 300. But again, the first calculation has told us only about the mean of the particular locations that were sampled. Wouldn't it be better to answer the questions by reference to the number of barnaces per square meter of reef, rather than square meter of measurement? Here, the **population** is then entire area of the Flower Garden Banks reef. Again, we will be able to answer the questions relative to the entire reef by working out the sample mean's distribution, relative to the population.


## Sample and population
I've made reference to samples and populations, and they are fundamental concepts in statistics. A sample is a pretty easy thing to understand - it is the data, the hard numbers that go into your calculations. The population is trickier. It's the units to which you are able to generalize your conclusions.

For the barnacle data, in order for the population to be the entire Flower Garden Banks reef, the sample must have been carefully selected to ensure it was representative of the entire reef (for instance, randomly sampling locations so that any location on the reef might have been selected).

For the mice on THC, the population is all the mice that might have been selected for use in the experiment. How big that population is depends on how the mice were selected for the experiment. Randomly selecting the experimental units from a group is a common way of ensuring that the results can generalize to that whole group. 

A non-random sample tends to mean that the population to which you can generalize is quite limited. What sort of population do you think we could generalize about if we recorded the age of everyone in this  workshop?

