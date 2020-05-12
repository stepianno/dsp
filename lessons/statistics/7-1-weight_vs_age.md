[Think Stats Chapter 7 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2008.html#toc70) (weight vs. age)

First import all the good stuff
```
from __future__ import print_function, division

%matplotlib inline

import numpy as np
import pandas as pd
import brfss

import thinkstats2
import thinkplot
import first
```
Jitter is a function that creates variance for rounded values
```
def Jitter(values, jitter=0.5):
    n = len(values)
    return np.random.normal(0, jitter, n) + values
```
Read in the data
```
live, firsts, others = first.MakeFrames()
live = live.dropna(subset=['agepreg', 'totalwgt_lb'])
```
and extract the variable we're concerned with
```
weight = live.totalwgt_lb
age = Jitter(live.ager, 0.5)
```
Because age is rounded to an integer value in the data, any plotting depicts columns of integer values. In reality, most women don't give birth on their birthday. I used the Jitter function to resolve this issue. Weight does not need to be jittered because it's presented as a float with a decimal value  
  
Next, I make a scatter plot of the data
```
thinkplot.Scatter(age, weight)
thinkplot.Config(xlabel='Mother\'s Age', ylabel='Baby weight (lbs)')
```
The plot shows a thick, horizontal line with the middle almost lined up at around seven pounds. There are a handful of outliers, particularly on the low end of the weights, but from this graph, there doesn't seem to be any correlation between mother's age and birthweight.  
  
Next, I bin the entries based on the age of the mother and put them into groups. I create an array of the average age of each group
```
bins = np.arange(15, 45, 5)
indices = np.digitize(age, bins)
groups = live.groupby(indices)
mean_ages = [group.ager.mean() for i, group in groups]
```
For each group, I then make a CDF of the birthweights
```
cdfs = [thinkstats2.Cdf(group.totalwgt_lb) for i, group in groups]
```
I then create a graph of the different percentiles for each group against the mean ages of the mothers
```
for percent in [75, 50, 25]:
    weight_percentiles = [cdf.Percentile(percent) for cdf in cdfs]
    label = '%dth' % percent
    thinkplot.Plot(mean_ages, weight_percentiles, label=label)
    
thinkplot.Config(xlabel='Mother\'s Age',
                 ylabel='Weight (lb)',
                 legend=False)
```
The graph depicts three mostly horizontal lines. The 75th percentile lingers around 8.2 lbs, the 50th 7.3, and the 25th 6.5. There is no clear evidence here between these two variables.  
  
To make final conclusions, I check both the Pearson's and Spearman's correlation values using Pandas corr method
```
weight.corr(age) # 0.0392
weight.corr(age, method='spearman') # 0.0421
```
Both these values are extremely close to zero, thus suggesting no correlation between the two
