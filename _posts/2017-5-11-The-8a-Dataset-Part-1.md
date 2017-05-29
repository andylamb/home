---
layout: post
title: The 8a Dataset, Part 1
---

* TOC
{:toc}

# TL;DR

I like data and climbing rocks. I wrote a quick script to crawl [8a.nu](https://www.8a.nu/) for the climbers' publicly available profiles and bouldering scorecards. Looking at the heights, weights, and BMIs was quite interesting: elite climbers have slightly lower BMIs and weights, while there was no statistically significant difference in heights. Anyone who finds this sort of thing interesting should dig into the data!

# Introduction

Spend some time at any climbing gym, sport crag, or online forum, and you'll quickly learn that the climbing tribe has no shortage of opinions on the best shoes to wear, fingerboard regimes to follow, or [German skin-drying creams](http://eveningsends.com/review-antihydral/) to dab on one's fingers. In addition to the unstructured knowledge that climbers share and synthesize, ideas from sport science have been applied to climbing; for example, the [Rock Climber's Training Manual](https://rockclimberstrainingmanual.com/) is an excellent guide to designing a [periodized](https://en.wikipedia.org/wiki/Sports_periodization) training plan for climbing.

The approaches top climbers take to improvement are quite heterogeneous: while some swear by a strictly measured training program, many just go to the gym and take burns on their latest project. I view this not as a problem, but as a beauty of our sport - climbing is incredibly complicated and varied, and it is likely that performance will not be as easily dissected by the scientific method as for more quantifiable and controlled sports, say running or powerlifting.

I was curious how I could apply techniques from [data mining](https://en.wikipedia.org/wiki/Data_mining) to gain insight into climbing.  Maybe the data from [8a.nu](https://www.8a.nu/) would give a unique angle on the landscape of climbing? "8a" lets climbers track their ascents, and serves as a forum to track progress, jot down notes on a climb, burn off  friends, and get 15 minutes of fame on the "sidebar" after a hard ascent. I hoped that by exploring a dataset of bios and climbs I could answer questions about climbing performance and the community as a whole. A few sample questions are:
- What is the relationship between height, weight, and climbing performance?
- Where are the most popular places for climbers to live?
- What are the most popular climbs?
- How balanced are the "route pyramids" of most climbers? Do most people actually do a significant number of routes at a certain grade before progressing to the next?

Here's the approach I took:
- [Getting the data](#getting-the-data): I wrote up a [Python](https://www.python.org/) script to gather the bios of climbers and their bouldering ascents, which are publicly available on 8a. An anonymized version is available - you should check it out!
- [What Shape are Climbers?](#what-shape-are-climbers): Box-and-whisker plots, histograms, and summary statistics are techniques to start to get a handle on the data. I broke climbers into three populations, based on what grades they climbed, and started to look at what patterns could be discerned in the bios of each population.
- [Let's do Some Significance Testing!](#lets-do-some-significance-testing): [Statistical hypothesis testing](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing) allows us to use computational power to make conclusions about our datasets. I used the [Analysis of Variance (ANOVA)](https://en.wikipedia.org/wiki/Analysis_of_variance) technique to see how the mean heights, weights, and BMIs of each of the three populations (broken down by grade) differed.

This is by no means meant to be exhaustive, or the final say; indeed, my hope is just to give a sample of what is possible and present the datasets to others. In the spirit of collaboration and reproducibility, I would love nothing more than for other people to dissect my results and be inspired by the dataset to answer questions of their own!

# Getting the data

I thought you'd never ask! Right now the data is two CSV files, one for [users](https://s3.amazonaws.com/andrewlamb/8a/users.csv) and one for [boulders](https://s3.amazonaws.com/andrewlamb/8a/boulders.csv). 

The names of the users are removed. The users file has all other fields available on an 8a profile, including height, weight, country, city, DOB, when they started climbing, occupation, other interests, best comp results, favorite climbing areas, where they can guide, sponsors, visits to their profile, and 8a scores and rankings. The boulders file has names, grades, dates, type of ascent (flash, onsight, redpoint), whether it is recommended, area, tags (e.g. "soft"), comments, and stars. **You should download them and play with them yourself!**.

# What Shape are Climbers?

Climbers have long been interested in what sort of body can be dragged up the steepest cliffs and most heinous edges. It's fascinating that body types as different as [Ashima](https://www.instagram.com/ashimashiraishi) (5', ~90lbs) and [Jan Hojer](https://vimeo.com/66473915) (6'2'', ~170lbs) can both excel at the sport. So what does the data on 8a say? Let's dig in!

### Populations
I considered three different populations:
- All the climbers on 8a that listed both height and weight (these fields are optional on the bio). This group had 5910 climbers. Climbers shorter than 120cm (~3'11'') or taller than 245cm (~8') are discarded as measurement errors (i.e. likely trolling in their bios). I call this "All Climbers".
- The subset of climbers that had at least 10 ascents 8a or harder. This group had 455 climbers. I call this "8a Climbers".
- The subset of climbers that had at least 10 ascents 8b or harder. This group had 91 climbers. I call this "8b Climbers"
- Note that these populations are disjoint, i.e. climbers from "All Climbers" are not in "8a Climbers".

### Plots and Descriptive Statistics

We'll use a couple different tools to get a high-level picture of the data. First, some basic [descriptive statistics](https://en.wikipedia.org/wiki/Descriptive_statistics): the mean, median, 25th quartile and 75th quartile. [Histograms](https://youtu.be/4eLJGG2Ad30) group the data into bins, e.g. "climbers from 162.5 to 165.5cm", and show how many data points fall into that bin. Finally, we now turn to our good friend from Intro. Statistics, the [box-and-whisker plot](https://youtu.be/b2C9I8HuCe4). The "box" ranges from the 25th percentile of the data to the 75th (we call the height of this box the "interquartile range"), with lines for the median and mean. The "whiskers" show the lowest and highest datums within the 1.5 times the interquartile range of the 25th and 75th percentiles, respectively.

I used [Plot.ly](https://plot.ly/) to display the data, so it might take a few seconds to load from their servers. If you are having trouble seeing things, try clicking the "Autoscale" or "Reset axes" buttons.

## Height

<iframe width="800" height="550" frameborder="0" scrolling="no" src="//plot.ly/~andylamb/45.embed?showlink=false"></iframe>

<iframe width="800" height="550" frameborder="0" scrolling="no" src="//plot.ly/~andylamb/71.embed?showlink=false"></iframe>

<table>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th style="color:#1F77B4">All Climbers</th>
      <th style="color:#FF7F0E">8a Climbers</th>
      <th style="color:#2CA02C">8b Climbers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Mean</td>
      <td style="color:#1F77B4">176.43</td>
      <td style="color:#FF7F0E">176.05</td>
      <td style="color:#2CA02C">176.94</td>
    </tr>
    <tr>
      <td>Median</td>
      <td style="color:#1F77B4">178.0</td>
      <td style="color:#FF7F0E">177.0</td>
      <td style="color:#2CA02C">178.0</td>
    </tr>
    <tr>
      <td>25th Percentile</td>
      <td style="color:#1F77B4">172.0</td>
      <td style="color:#FF7F0E">171.0</td>
      <td style="color:#2CA02C">172.0</td>
    </tr>
    <tr>
      <td>75th Percentile</td>
      <td style="color:#1F77B4">182.0</td>
      <td style="color:#FF7F0E">181.0</td>
      <td style="color:#2CA02C">180.0</td>
    </tr>
  </tbody>
</table>

There's a lot going on here! Let's step through the box-and-whisker plot to make sure we're on the same page. If you mouse over one of the groups the plot should display a bunch of numbers. The top and bottom ones point to the maximum and minimum points in the dataset. So for "All Climbers" this would be 235 and 120. Next, we have numbers for the whiskers, 197 and 157 in this example. Remember that the placement of the whiskers are based on the interquartile range, and the important part is that any points outside the whiskers are considered outliers. On this plot those points are displayed as dots. The top and bottom of the box are the 75th and 25th percentiles, respectively, which are 182 and 172. Finally, the solid line inside the box is the median (178) and the dashed line is the mean (176.4).

Ok, so what can we say about the data? First, there do seem to be a lot of outliers. This is tricky, because these are likely "measurement errors", i.e. accounts that lie about their heights. We'll need to remember this in our analysis, so it's nice that the plot gives us a heads up. What about the medians? Mousing over the plots, we can see that the values are 178 for "All Climbers" and "8b Climbers" and 177 for "8a Climbers" - so it looks like all the groups are roughly the same. For the means, the values are 176.4, 176.0, and 176.9, so all the groups are within a centimeter of each other. Similarly, the 25th and 75th percentiles are all within a centimeter or two of each other.

Let's take a quick look at the histogram as well. The x-axis is height and the y-axis is percentage, with a bin size of 3, and when you mouse over a column the number displayed is the center of the bin. For example, the second column from the left shows 164, so we can see that an 8b climber has a 2% chance of being between 162.5 and 165.5cm. The y-axis is normalized, i.e. it shows probabilities rather than raw counts, because the size of the different groups are different. Histograms are another nice way to get an exploratory view of the data. We can see that the heights of the three groups all seem to be distributed similarly, and look approximately normal.

Going off the plots and summary statistics, it seems like the height distributions of the different groups aren't drastically different.

## Weight

<iframe width="800" height="550" frameborder="0" scrolling="no" src="//plot.ly/~andylamb/47.embed?showlink=false"></iframe>

<iframe width="800" height="550" frameborder="0" scrolling="no" src="//plot.ly/~andylamb/75.embed?showlink=false"></iframe>

<table>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th style="color:#1F77B4">All Climbers</th>
      <th style="color:#FF7F0E">8a Climbers</th>
      <th style="color:#2CA02C">8b Climbers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Mean</td>
      <td style="color:#1F77B4">68.21</td>
      <td style="color:#FF7F0E">66.81</td>
      <td style="color:#2CA02C">66.92</td>
    </tr>
    <tr>
      <td>Median</td>
      <td style="color:#1F77B4">68.0</td>
      <td style="color:#FF7F0E">68.0</td>
      <td style="color:#2CA02C">68.0</td>
    </tr>
    <tr>
      <td>25th Percentile</td>
      <td style="color:#1F77B4">63.0</td>
      <td style="color:#FF7F0E">63.0</td>
      <td style="color:#2CA02C">63.0</td>
    </tr>
    <tr>
      <td>75th Percentile</td>
      <td style="color:#1F77B4">73.0</td>
      <td style="color:#FF7F0E">73.0</td>
      <td style="color:#2CA02C">68.0</td>
    </tr>
  </tbody>
</table>



So what's going on with the weight distributions? Mousing over the plots, it looks like the 25th percentiles and medians are the same for all groups. The 75th percentile is lower for "8b Climbers" than for the other two groups (68 vs. 73kg) and the mean for "All Climbers" is about 1.3kg higher than for the other two groups.

The histogram can give us a little more information. If we look at the 57, 62, and 67kg bins (the bin size is 5kg, so these would be 54.5 - 59.5kg, etc.), 8b climbers are the group with the highest probability, then 8a climbers, then all climbers. The order is reversed for the 77, 82, and 52 bins. So what do we make of this? Well it does seem that the 8b and 8a distributions are skewed lighter than the all climbers distribution. The 52 bin is a bit of an anomaly. Looking at the box-and-whisker plot, we can see that there are lower outliers for the all climbers and 8a climbers groups; on the histogram, there are no 8b climbers in the 52 bin, while there are non-negligible percentages for the other two groups (around 5%).

## BMI

<iframe width="800" height="550" frameborder="0" scrolling="no" src="//plot.ly/~andylamb/43.embed?showlink=false"></iframe>

<iframe width="800" height="550" frameborder="0" scrolling="no" src="//plot.ly/~andylamb/69.embed?showlink=false"></iframe>

<table>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th style="color:#1F77B4">All Climbers</th>
      <th style="color:#FF7F0E">8a Climbers</th>
      <th style="color:#2CA02C">8b Climbers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Mean</td>
      <td style="color:#1F77B4">21.86</td>
      <td style="color:#FF7F0E">21.52</td>
      <td style="color:#2CA02C">21.34</td>
    </tr>
    <tr>
      <td>Median</td>
      <td style="color:#1F77B4">21.8</td>
      <td style="color:#FF7F0E">21.33</td>
      <td style="color:#2CA02C">21.3</td>
    </tr>
    <tr>
      <td>25th Percentile</td>
      <td style="color:#1F77B4">20.53</td>
      <td style="color:#FF7F0E">20.34</td>
      <td style="color:#2CA02C">19.88</td>
    </tr>
    <tr>
      <td>75th Percentile</td>
      <td style="color:#1F77B4">23.04</td>
      <td style="color:#FF7F0E">22.53</td>
      <td style="color:#2CA02C">22.53</td>
    </tr>
  </tbody>
</table>


[BMI](https://en.wikipedia.org/wiki/Body_mass_index) is defined as the climber's weight (in kg) divided by the square of their height (in meters squared). BMI is a way to classify body types, and a common categorization is 18.5 or lower (underweight), 18.5 to 25 (normal weight), 25 to 30 (overweight), and 30 or over (obese). [It is very rough metric](http://www.musclebuildingfoodshq.com/body-mass-index-chart-limitations/) - Michael Jordan would have been classified as overweight during his athletic prime and Marshawn Lynch (5'11'', 215lbs) is on the edge of "obese". Since we are asking what the relationship between body type and climbing is, I would argue that it is still interesting to consider; LeBron James is freakishly athletic, but at 6'8" and 250lbs it seems doubtful he would be able to climb 8b. From a pragmatic perspective, our dataset doesn't contain more detailed body composition metrics, such as body fat percentage or ape index.

So what are the charts showing? It looks like the stats are very slightly lower for the 8a and 8b groups than for all climbers. On median (21.8 vs. 21.3), mean (21.9, 21.5 for 8a climbers, and 21.3 for 8b climbers), 25th percentile (20.5, 20.3 for 8a climbers, and 19.9 for 8b climbers), and 75th percentile (23.0 vs. 22.5) the BMIs drop around half a point as we move to the more elite groups. Looking at the 19.5, 20.5, 21.5, and 22.5 bins (bin size is 1) of the histogram is interesting: on the 19.5 bin 8b climbers are overrepresented, on the 20.5 and 21.5 bins 8a climbers have the highest percentage, and on the 22.5 bin 8b climbers are underrepresented. This lets us understand our box-and-whisker plot a bit more.

It is also worth thinking about what 1 unit of BMI means. Say we have a climber that is 178cm. Then, in order to change their BMI by 1 unit of kg per meters squared, they would need to change their weight by 1.78 squared, or about 3.17kg. For example, if they were 69kg their BMI would be approximately 21.8 and if they were 65.8kg their BMI would be approximately 20.8.

So overall, the three groups have pretty similar distributions of BMIs, with 8b climbers and 8a climbers tending to be slightly lower. All three groups fall in the middle of the "normal weight" category, but as stated before, these classifications are often considered questionable.

# Let's do Some Significance Testing!

So far we've used box-and-whisker plots, histograms, and summary statistics to get a feel for how the different populations look. But just staring at the plots and numbers, it's difficult to tell how different the populations are. We can use [statistical hypothesis testing](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing) to answer the question of whether all climbers, 8a climbers, and 8b climbers are really do have different heights, weights, and BMIs.

Don't be intimidated if this hypothesis testing thing sounds intimidating. It's pretty easily demonstrated with a concrete example. Say we run an experiment where 100 rats are fed a diet of [CLIF® Nut Butter Filled Energy Bars](http://www.clifbar.com/products/clif/nut-butter-filled) (Chocolate Hazelnut flavor) and 100 rats just get the normal rat food. Then we measure the time each rat takes to run through a maze, and want to know if the special diet made any difference. 

Let's run a simple simulation of this experiment. In the simulation, the times are drawn from a random normal distribution with the same standard deviation, and the mean for the group with the special food is slightly lower. This means that the Nut Butter Filled Bars really are making a difference; but the key here is that we don't get to see the distributions the times are drawn from, we **only** get to see the 100 samples. From these samples, we need to figure out if the bars are making a difference.

To analyze our experiment, we'll use a [null hypothesis](https://en.wikipedia.org/wiki/Null_hypothesis) that the populations are the same and use a [t-test](https://en.wikipedia.org/wiki/Student%27s_t-test) to calculate a [p-value](https://en.wikipedia.org/wiki/P-value). That was a lot of terminology, but the idea is pretty simple! We've observed some data from our experiment, and we want to know if the times are due to random chance, or if the two groups really are different. For a second, assume that the bars make no difference, i.e. the distributions they are drawn from are exactly the same. This is the null hypothesis, and it is says that the variable we were experimenting with (the food) didn't have an effect. The p-value is the probability that if the null hypothesis is true (the groups are the same) we would observe the data. So if the p-value is low, it means that the timing data we observed is unlikely if the food is making no difference. If the p-value is below some threshold called the significance level - 0.05 is a common choice - we say that the result is [statistically significant](https://en.wikipedia.org/wiki/Statistical_significance). OK, let's look at a really short Python script for this simulation!

```python
# Import numpy and scipy, common scientific computing packages.
import numpy as np
from scipy import stats

# We use a random seed so the same data is always 
# generated - don't worry about this too much.
np.random.seed(13)

# Generate data for the group with the bars and the group with
# the normal food. The group with the bars has a mean time of 5.5s
# and the other group has a mean time of 6.0s
bar_group = np.random.normal(5.5, 1.0, 100)
control_group = np.random.normal(6.0, 1.0, 100)

# Print statistics about our data.
print """
The bar group has mean {:.3f} and std {:.3f}.
The control group has mean {:.3f} and std {:.3f}.
""".format(
    bar_group.mean(), bar_group.std(), 
    control_group.mean(), control_group.std()
)

# Here is the fun part! We use a t-test to calculate the p-value,
# i.e. the probability that if the means in both groups were the 
# same, we would observe our data.
t, prob = stats.ttest_ind(bar_group, control_group)

print """
Ran t-test. The p-value is {:.3f}
""".format(prob)
```

This generates the output
```
The bar group has mean 5.558 and std 0.932.
The control group has mean 5.854 and std 0.809.

Ran t-test. The p-value is 0.018
```
Let's go over what happened. First we generated 100 random samples from the group that ate the bars (`bar_group = np.random.normal(5.5, 1.0, 100)`) and the group that ate the normal food (`control_group = np.random.normal(6.0, 1.0, 100)`). The sample mean of the bar group (5.558) was lower than the sample mean of the control group (5.854) - this is what we'd expect, because the population mean for the bar group (5.5) is lower than the population mean for the control group (6.0). But it is **crucial** that we put ourselves in the mindset of scientists that only observe the values of `bar_group` and `control_group`! 

We pose the null hypothesis that the population means are the same, and use a t-test to find the probability of observing these data if the null hypothesis is in fact true (`stats.ttest_ind(bar_group, control_group)`). We calculate a p-value of 0.018, so this probability is in fact quite low. If we chose a significance level of 0.05 (a common choice), we could reject our null hypothesis, and make the statement that we believe the population means are in fact different.

I hope this quick example was helpful! If you're still a bit lost or want to dive deeper into the statistical machinery we're using there are a lot of resources out there; I found that [this](https://youtu.be/-FtlH4svqx4) video was a great place to start.

## But There are More Than Two Groups...?

An astute reader would point out that in our data there are three groups (all climbers, 8a climbers, and 8b climbers), but in my rat simulation there were only two groups. The test we used (`stats.ttest_ind`) only works for two groups, so what should we do? I decided to use [Analysis of Variance (ANOVA)](https://en.wikipedia.org/wiki/Analysis_of_variance), which is similar to the t-test we just used, but works for more than two groups. So in this case, we assume the null hypothesis is that the population means of all of the groups are the same. 

For full clarity, let's do one more simulation. Say that we want to see if the type of nut butter has an affect on the rats. We'll try out Chocolate Hazelnut, Peanut, and Coconut Almond, feeding each flavor to 100 rats. Continuing the code from above:
```python
# Generate data for the three different groups. All
# groups have the same mean time.
hazelnut_group = np.random.normal(5.5, 1.0, 100)
peanut_group = np.random.normal(5.5, 1.0, 100)
almond_group = np.random.normal(5.5, 1.0, 100)

# Print statistics about our data.
print """
The Chocolate Hazelnut group has mean {:.3f} and std {:.3f}.
The Peanut group has mean {:.3f} and std {:.3f}.
The Coconut Almond group has mean {:.3f} and std {:.3f}.
""".format(
    hazelnut_group.mean(), hazelnut_group.std(),
    peanut_group.mean(), peanut_group.std(),
    almond_group.mean(), almond_group.std(),
)

# Here we use ANOVA, which can take in more than two groups.
f, prob = stats.f_oneway(hazelnut_group, peanut_group, almond_group)

print """
Ran ANOVA. The p-value is {:.3f}
""".format(prob)
```

And the output is
```
The Chocolate Hazelnut group has mean 5.447 and std 1.058.
The Peanut group has mean 5.551 and std 0.944.
The Coconut Almond group has mean 5.417 and std 0.921.

Ran ANOVA. The p-value is 0.596
```

Here we see that the p-value is much larger than before. If we assume that the data for each group is drawn from a distributions with the same population mean (again, **only we** know that this is true, if we actually ran this experiment we would only see the data), the probability of seeing the data is 0.596. So we can not reject the null hypothesis: there is no evidence that flavor has an effect on the time it takes to complete the maze.

If you want to learn more about ANOVA, I would check out [these videos](https://www.khanacademy.org/math/statistics-probability/analysis-of-variance-anova-library). Stats is pretty fun stuff - let's get cracking on the real data!

## ANOVA Results

<table>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th>Height</th>
      <th>Weight</th>
      <th>BMI</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>p-value</td>
      <td>0.52853</td>
      <td>0.003656</td>
      <td>0.001501</td>
    </tr>
  </tbody>
</table>

Now let's dissect the results of our hypothesis test. Remember that our null hypothesis is that the distributions (for height, weight, and BMI) between all three groups have the same mean. The p-value is the probability of observing our data if the null hypothesis is true. For height, the p-value is high: there is a 0.52 probability of observing our data if the height distributions all have the same mean. Thus, we cannot reject the null hypothesis; it looks like the height distributions between groups do have the same mean.

The story is different for the tests of weight and BMI. On both of these tests, the p-values are low, so there is a low chance of observing our data if the means of the "true" distributions for the three groups were the same. In fact, our p-values for weight and BMI are lower than the chosen significance level (we chose 0.05) so we can reject the null hypothesis.

So what do these statements mean? First, we said that the height distributions for all climbers, 8a climbers, and 8b climbers have the same mean. Next, we said that the distributions for weight and BMI do not have the same mean (for these distributions we rejected the null hypothesis). Note that this does not give information about how large the difference in their means are, whether two of the groups might have the same mean, or whether other statistics such as percentiles might be the same. In fact, from our work above with the charts and descriptive statistics it looked like any differences were quite small. For example, the difference in mean BMIs between all climbers and 8b climbers was only about half a unit (21.86 vs. 21.34) and the difference in mean weights was less than 1.5 kgs (68.21 vs. 66.92).

The really cool thing about a statistical test is that it can help us move beyond the intuition that something looks a bit different. I like to think of it as a computer looking over the thousands of data points for us to make sure we didn't miss anything. Of course, by codifying this analysis in equations we make a set of assumptions that may not hold with exact fidelity to reality, so I like to combine human intuition with mathematical formality and computational power.

# Wrapping Things Up

## A few Notes About the Dataset

The dataset I got from 8a can be a bit messy. Here I go through a few things I noticed that could potentially be issues.

### Weights are Bucketed

8a only allows climbers to log their weights in discrete intervals, e.g. 71 - 75kg. For height, climbers are allowed to input any number in cm.

### Outliers

As seen from the box-and-whisker plots, there are a number of "measurement errors", or accounts that do not input a correct height and weight. We attempted to clean these from our analysis; for example, requiring that the "All Climbers" group had logged at least 10 climbs seemed to help reduce the number of bogus accounts. However, there is definitely potential to figure out more ways to clean up the data.

## What's Next?

This analysis is just a start, and in the spirit of scientific advancement I'd love for other people to dissect my methods and figure out new ways to use this data to advance our collective climbing knowledge! I wanted to understand a rough relationship between body composition and bouldering achievement, and it seems that there is a relatively small correlation for weight and BMI. The data can be a bit messy, which is all the more reason for more inquiring minds to delve in. As [Linus Tovalds](https://en.wikipedia.org/wiki/Linus_Torvalds), creator of the Linux operating system, said, "Given enough eyeballs, all bugs are shallow". I'm excited to see what you find!

# Appendix: Why is it called Analysis of Variance?

When we run ANOVA, we calculate a statistic called the f-value (which is why the function is called `stats.f_oneway`). The intuition for the F-value is that there is some variance between groups and within groups, and we are interested in the ratio of these variances (which is where the name "Analysis of Variance" comes from). Say we find that there is much more variance between groups than within groups; in this case, the F-value will be higher and it seems more likely that the groups are fundamentally distributed differently. On the other hand, if we find there is high variance within groups and not much variance between groups, than it is less likely the groups are actually distributed differently. Once we have the F-value, we can use the p-value as a way to formalize this intuition.

<!-- # Appendix: Why is it called Student's t-test? -->


