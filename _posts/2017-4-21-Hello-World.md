---
layout: post
title: Hmm, what should the title be...?
---

# Introduction

Spend some time at any climbing gym, sport crag, or online forum, and you'll quickly learn that the climbing tribe has no shortage of conventional wisdom on the best shoes to wear, fingerboard regimes to follow, or even German [skin-drying creams](http://eveningsends.com/review-antihydral/) to dab on one's fingers. I use the term "conventional wisdom" not to be dismissive, but as my best attempt to describe the accumulation of unstructured knowledge that climbers share and synthesize. Climbing is an incredibly complicated pursuit, and it is my intuition that performance will not be as easily dissected by the scientific method as it would be for more quantifiable and controlled sports, say running or powerlifting.

I was thinking of ways I could leverage my technical skills and combine them with my knowledge and curiosity for climbing. I came up with the idea of using the data from [8a.nu](https://www.8a.nu/) to get a unique angle on the landscape of climbing. The core function of "8a" is as a log for a climber's ascents. It serves as a forum to track your progress, jot down notes on a climb, burn off your friends, and get 15 minutes of fame on the sidebar after a hard ascent. I hoped that by exploring a dataset of bios and climbs I could answer questions about climbing performance and get a bird's eye view of the community as a whole. For example:
- What is the relationship between height and weight, and climbing performance?
- Where are the most popular places for climbers to live?
- What are the most popular climbs?
- How balanced are the "route pyramids" of most climbers? That is, do most people actually do a significant number of routes at a certain grade before progressing to the next?

I wrote up a [Python](https://www.python.org/) script to gather the data on the climbers and ascents available on 8a. Here I present some basic analysis I did. This is by no means meant to be exhaustive, or the final say; indeed, my hope is just to give a sample of what is possible and present the datasets to others. In the spirit of collaboration and reproducability, I would love nothing more than for other people to dissect my results and be inspired by the dataset to answer questions of their own!

# Where can I see the data?!?!

I thought you'd never ask! Right now the data is two tables, one for users and one for boulders.
- We're working with **BIG DATA** here, and I needed a petabyte scale data warehouse that was fault tolerant and offered network isolation. You can download a [SQLite](http://sqlite.org/index.html) dump of the data [here](https://github.com/andylamb/8a-data/raw/master/output.sql).
- You can also get CSV files for the [users](https://github.com/andylamb/8a-data/raw/master/users.csv) and [boulders](https://github.com/andylamb/8a-data/raw/master/boulders.csv) tables.
- See the [Jupyter notebook where I do all the analysis](https://github.com/andylamb/8a-data/blob/master/notebooks/Exploration.ipynb).

# What Shape are Climbers?

Climbers have long been interested in what sort of body can be dragged up the steepest cliffs and most heinous edges. It's fascinating that body types as different as [Ashima](https://www.instagram.com/ashimashiraishi) (5', ~90lbs) and [Jan Hojer](https://vimeo.com/66473915) (6'2'', ~170lbs) can both excel at the sport. So what does the data on 8a say? Let's dig in!

### Populations
I considered three different populations:
- All the climbers on 8a that listed both height and weight (these fields are optional on the bio). This group had 24979 climbers. Climbers shorter than 120cm (~3'11'') or taller than 245cm (~8') are discarded as measurement errors (i.e. likely trolling in their bios).
- The subset of climbers that had at least 10 ascents 8a or harder. This group had 577 climbers.
- The subset of climbers that had at least 10 ascents 8b or harder. This gropu had 97 climbers.

### Box-and-Whisker Plots
To get a high-level picture of the data, we now turn to our good friend from Intro. Statistics, the box-and-whisker plot. The "box" ranges from the 25th percentile of the data to the 75th (we call the height of this box the "interquartile range"), with a lines for the median and mean. The "whiskers" show the lowest and highest datums within the 1.5 times the interquartile range of the 25th and 75th percentiles, respectively. Check out [this video](https://youtu.be/b2C9I8HuCe4) for a more in-depth explanation.

## Height

<div>
    <a href="https://plot.ly/~andylamb/45/?share_key=Zxk4ewdjfgkuYvDiLLC2Jr" target="_blank" title="Heights Histogram" style="display: block; text-align: center;"><img src="https://plot.ly/~andylamb/45.png?share_key=Zxk4ewdjfgkuYvDiLLC2Jr" alt="Heights Histogram" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="andylamb:45" sharekey-plotly="Zxk4ewdjfgkuYvDiLLC2Jr" src="https://plot.ly/embed.js" async></script>
</div>

<div>
    <a href="https://plot.ly/~andylamb/71/?share_key=7EoAlr6Y2XzFHnG8NbmmpE" target="_blank" title="Heights" style="display: block; text-align: center;"><img src="https://plot.ly/~andylamb/71.png?share_key=7EoAlr6Y2XzFHnG8NbmmpE" alt="Heights" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="andylamb:71" sharekey-plotly="7EoAlr6Y2XzFHnG8NbmmpE" src="https://plot.ly/embed.js" async></script>
</div>

<div>
    <a href="https://plot.ly/~andylamb/83/?share_key=jww4wndQv3UrVly5ycrHAT" target="_blank" title="Heights Table" style="display: block; text-align: center;"><img src="https://plot.ly/~andylamb/83.png?share_key=jww4wndQv3UrVly5ycrHAT" alt="Heights Table" style="max-width: 100%;width: 600px;"  width="600" height="300" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="andylamb:83" sharekey-plotly="jww4wndQv3UrVly5ycrHAT" src="https://plot.ly/embed.js" async></script>
</div>

There's a lot going on here! Let's step through the box-and-whisker plot to make sure we're on the same page. If you mouse over one of the groups the plot should display a bunch of numbers. The top and bottom ones point to the maximum and minimum points in the dataset. So for "All Climbers" this would be 235 and 120. Next, we have numbers for the whiskers, 197 and 157 in this example. Remember that the placement of the whiskers are based on the interquartile range, and the important part is that any points outside the whiskers are considered outliers. On this plot those points are displayed as dots. The top and bottom of the box are the 75th and 25th percentiles, respectively, which are 182 and 172. Finally, the solid line inside the box is the median (178) and the dashed line is the mean (176.4).

Ok, so what can we say about the data? First, there do seem to be a lot of outliers. This is tricky, because these are likely "measurement errors", i.e. accounts that lie about their heights. We'll need to remember this in our analysis, so it's nice that the plot gives us a heads up. What about the medians? Mousing over the plots, we can see that the values are 178 for "All Climbers" and "8b Climbers" and 177 for "8a Climbers" - so it looks like all the groups are roughly the same. For the means, the values are 176.4, 176.0, and 176.9, so all the groups are within a centimeter of each other. Similarly, the 25th and 75th percentiles are all within a centimeter or two of each other.

Let's take a quick look at the histogram as well. The x-axis is weight and the y-axis is percentage, with a bin size of 3, and when you mouse over a column the number displayed is the center of the bin. For example, the second column from the left shows 164, so we can see that an 8b climber has a 2% chance of being between 162.5 and 165.5cm. The y-axis is normalized, i.e. show probabilities rather than raw counts, because the size of the different groups are different. Histograms are another nice way to get an exploratory view of the data. We can see that the heights of the three groups all seem to be distributed similarly, and approximately look approximately normal.

Going off the plots and summary statistics, it seems like the height distributions of the different groups aren't drastically different.

## Weight

<div>
    <a href="https://plot.ly/~andylamb/47/?share_key=WontCtu14MvpNNxKmhM7UD" target="_blank" title="Weights Histogram" style="display: block; text-align: center;"><img src="https://plot.ly/~andylamb/47.png?share_key=WontCtu14MvpNNxKmhM7UD" alt="Weights Histogram" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="andylamb:47" sharekey-plotly="WontCtu14MvpNNxKmhM7UD" src="https://plot.ly/embed.js" async></script>
</div>

<div>
    <a href="https://plot.ly/~andylamb/75/?share_key=KfYR0XXmkxc7KeKCQaSVBB" target="_blank" title="Weights" style="display: block; text-align: center;"><img src="https://plot.ly/~andylamb/75.png?share_key=KfYR0XXmkxc7KeKCQaSVBB" alt="Weights" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="andylamb:75" sharekey-plotly="KfYR0XXmkxc7KeKCQaSVBB" src="https://plot.ly/embed.js" async></script>
</div>

<div>
    <a href="https://plot.ly/~andylamb/85/?share_key=AMe9eMSXcuk6G8HpOGJNOw" target="_blank" title="Weights Table" style="display: block; text-align: center;"><img src="https://plot.ly/~andylamb/85.png?share_key=AMe9eMSXcuk6G8HpOGJNOw" alt="Weights Table" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="andylamb:85" sharekey-plotly="AMe9eMSXcuk6G8HpOGJNOw" src="https://plot.ly/embed.js" async></script>
</div>

So what's going on with the weight distributions? Mousing over the plots, it looks like the 25th percentiles and medians are the same for all groups. The 75th percentile is lower for "8b Climbers" than for the other two groups (68 vs. 73kg) and the mean for "All Climbers" is about 1.3kg higher than for the other two groups.

The histogram can give us a little more information. If we look at the 57, 62, and 67kg bins (the bin size is 5kg, so these would be 54.5 - 59.5kg, etc.), 8b climbers are the group with the highest probability, then 8a climbers, then all climbers. The order is reversed for the 77, 82, and 52 bins. So what do we make of this? Well it does seem that the 8b and 8a distributions are skewed lighter than the all climbers distribution. The 52 bin is a bit of an anomaly - looking at the box-and-whisker plot, we can see that there are lower outliers for the all climbers and 8a climbers groups.

## BMI

<div>
    <a href="https://plot.ly/~andylamb/43/?share_key=iEOO7UeAtZ1MdpZJ3nY6Du" target="_blank" title="BMI Histogram" style="display: block; text-align: center;"><img src="https://plot.ly/~andylamb/43.png?share_key=iEOO7UeAtZ1MdpZJ3nY6Du" alt="BMI Histogram" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="andylamb:43" sharekey-plotly="iEOO7UeAtZ1MdpZJ3nY6Du" src="https://plot.ly/embed.js" async></script>
</div>

<div>
    <a href="https://plot.ly/~andylamb/69/?share_key=qOtl8VPos9Dcc4aHFcZWuU" target="_blank" title="BMIs" style="display: block; text-align: center;"><img src="https://plot.ly/~andylamb/69.png?share_key=qOtl8VPos9Dcc4aHFcZWuU" alt="BMIs" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="andylamb:69" sharekey-plotly="qOtl8VPos9Dcc4aHFcZWuU" src="https://plot.ly/embed.js" async></script>
</div>

<div>
    <a href="https://plot.ly/~andylamb/89/?share_key=rILe2OBQ0CPnv1UFnC4KMb" target="_blank" title="BMIs Table" style="display: block; text-align: center;"><img src="https://plot.ly/~andylamb/89.png?share_key=rILe2OBQ0CPnv1UFnC4KMb" alt="BMIs Table" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plot.ly/404.png';" /></a>
    <script data-plotly="andylamb:89" sharekey-plotly="rILe2OBQ0CPnv1UFnC4KMb" src="https://plot.ly/embed.js" async></script>
</div>

BMI is defined as the climber's weight (in kg) divided by the square of their height (in meters squared). BMI is a way to classify body types, and a common categorization under 18.5 (underweight), 18.5 to 25 (normal weight), 25 to 30 (overweight), over 30 (obese). It is very rough metric - Michael Jordan was famously classified as overweight during his athletic prime. Since we are asking what the relationship between body type and climbing is, I would argue that it is still interesting to consider; LeBron James is freakishly athletic, but at 6'8" and 250lbs it seems doubtful he would be able to climb 8b. From a pragmatic perspective, our dataset doesn't contain more detailed body composition metrics.

So what are the charts showing? It looks like the stats are very slightly lower for the 8a and 8b groups than for all climbers. On median (21.8 vs. 21.3), (21.9, 21.5 for 8a climbers, and 21.3 for 8b climbers), 25th percentile (20.5, 20.3 for 8a climbers, and 19.9 for 8b climbers), and 75th percentile (23.0 vs. 22.5) the BMIs drop around half a point as we move to the more elite groups. Looking at the 19.5, 20.5, 21.5, and 22.5 bins (bin size is 1) of the histogram is interesting: on the 19.5 bin 8b climbers are overrepresented, on the 20.5 and 21.5 bins 8a climbers have the highest percentage, and on the 22.5 bin 8b climbers are underrepresented. This lets us understand our box-and-whisker plot a bit more.

It is also worth thinking about what 1 unit of BMI means. Say we have a climber that is 178cm. Then, in order to change their BMI by 1 unit of kg per meters squared, they would need to change their weight by 1.78 squared, or about 3.17kg. For example, if they were 69kg their BMI would be approximately 21.8 and if they were 65.8kg their BMI would be approximately 20.8.

So overall, the three groups have pretty similar distributions of BMIs, with 8b climbers and 8a climbers tending to be slightly lower. All three groups fall in the middle of the "normal weight" category, but as stated before, these classifications are often considered questionable.

# Let's do Some Significance Testing!

So far we've used box-and-whisker plots and histograms to get a feel for how the different populations look. But just staring at the plots, it's difficult to tell how different the populations are; that is, we want a way to answer the question of whether all climbers, 8a climbers, and 8b climbers are really do have different heights, weights, and BMIs.

Here we turn to [Analysis of Variance (ANOVA)](https://en.wikipedia.org/wiki/Analysis_of_variance), which tests whether the means of multiple groups are significantly different. Imagine there are some "true" distributions of heights (or weights or BMIs) out there for all climbers, 8a climbers, and 8b climbers, and the data we got from 8a are samples from these true distributions. Of course we can't observe these true distributions, only the samples, but we want to infer some knowledge about the true distributions, in this case their means. We can use ANOVA for this task: the input is the samples we got from 8a, and the test computes an F-value and [p-value](https://en.wikipedia.org/wiki/P-value). 

The intuition for the F-value is that there is some variance between groups and within groups, and we are interested in the ratio of these variances (which is where the name "Analysis of Variance" comes from). Say we find that there is much more variance between groups than within groups; in this case, the F-value will be higher and it seems more likely that the groups are fundamentally distributed differently. On the other hand, if we find there is high variance within groups and not much variance between groups, than it is less likely the groups are actually distributed differently. Once we have the F-value, we can use the p-value as a way to formalize this intuition.

The p-value is the probability that if the [null hypothesis](https://en.wikipedia.org/wiki/Null_hypothesis) (in our case all three of the population means being equal) is true, we would observe the samples. If the p-value is less than a chosen significance level (0.05 is a common choice), we would say that the results are [statistically significant](https://en.wikipedia.org/wiki/Statistical_significance). For example, if the results for BMI are statistically significant, it means that our observed BMI data is unlikely if the means the true BMI distributions were all the same.

Stats is pretty fun stuff - let's get cracking on these tests!

## ANOVA

| |Height | Weight | BMI|
|---|-------------|-------------|------------|
F-value | 0.637715 | 5.615863 | 6.507667 |
p-value | 0.52853  | 0.003656 | 0.001501 |

Now let's dissect the results of our hypothesis test. Remember that our null hypothesis is that the distributions (for height, weight, and BMI) between all three groups have the same mean. The F-value is the ratio of variance between groups to variance within a group, and the p-value is the probability of observing our data if the null hypothesis is true. For height, the F-value is low - there isn't much variance between groups - and the p-value is high: there is a 0.52 probability of observing our data if the height distributions all have the same mean. For height, it looks like the distributions between groups do have the same mean.

The story is different for the tests of weight and BMI. On both of these tests, there is a lot of variance between groups relative to the variance within groups (the F-value is high), and the p-values are low, so there is a low chance of observing our data if the means of the "true" distributions for the three groups were the same. In fact, our p-values for weight and BMI are lower than the chosen significance level (we chose 0.05) so we can reject the null hypothesis.

So what do these statements mean? First, we said that the height distributions for all climbers, 8a climbers, and 8b climbers have the same mean. Next, we said that the distributions for weight and BMI do not have the same mean. Note that this does not give information about how large the difference in their means are, whether two of the groups might have the same mean, or whether other statistics such as percentiles might be the same. This basically jives with what we were seeing when we looked at the charts and descriptive statistics: it looked like the distributions were slightly different. 

The really cool thing about a statistical test is that it can help us move beyond the intuition that something looks a bit different. I like to think of it as a computer looking over the thousands of data points for us to make sure we didn't miss anything. Of course, by codifying this analysis in equations we make a set of assumptions that may not hold with exact fidelity to reality, so I like to combine human intuition with mathematical formality and computational power.

# Wrapping Things Up

## A few Notes About the Dataset

The dataset I got from 8a can be a bit messy. Here I go through a few things I noticed that could potentially be issues.

### Weights are Bucketed

8a only allows climbers to log their weights in discrete intervals, e.g. 71 - 75kg. For height, climbers are allowed to input any number in cm.

### Outliers

As seen from the box-and-whisker plots, there are a number of "measurement errors", or accounts that do not input a correct height and weight. We attempted to clean these from our analysis; for example, requiring that the "All Climbers" group had logged at least 10 climbs seemed to help reduce the number of bogus accounts. However, there is definitely potential to figure out more ways to clean up the data.

## What's Next?

This analysis is just a start, and in the spirit of scientific advancement I'd love for other people to dissect my methods and figure out new ways to use this data to advance our collective climbing knowledge! I wanted to understand a rough relationship between body composition and bouldering achievement, and it seems that there is a relatively small correlation for weight and BMI. The data can be a bit messy, which is all the more reason for more inquiring minds to delve in. As Linus Tovalds, creator of the Linux operating system, said, "Given enough eyeballs, all bugs are shallow". I'm excited to see what you find!
