[Think Stats Chapter 6 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2007.html#toc60) (household income)

Using the provided code

```
from __future__ import print_function, division

%matplotlib inline

import numpy as np

import brfss

import thinkstats2
import thinkplot
```
```
def RawMoment(xs, k):
    return sum(x**k for x in xs) / len(xs)
def StandardizedMoment(xs, k):
    var = xs.var()
    std = np.sqrt(var)
    return CentralMoment(xs, k) / std**k
def Skewness(xs):
    return StandardizedMoment(xs, 3)
```
```
def InterpolateSample(df, log_upper=6.0):
    """Makes a sample of log10 household income.

    Assumes that log10 income is uniform in each range.

    df: DataFrame with columns income and freq
    log_upper: log10 of the assumed upper bound for the highest range

    returns: NumPy array of log10 household income
    """
    # compute the log10 of the upper bound for each range
    df['log_upper'] = np.log10(df.income)

    # get the lower bounds by shifting the upper bound and filling in
    # the first element
    df['log_lower'] = df.log_upper.shift(1)
    df.loc[0, 'log_lower'] = 3.0

    # plug in a value for the unknown upper bound of the highest range
    df.loc[41, 'log_upper'] = log_upper
    
    # use the freq column to generate the right number of values in
    # each range
    arrays = []
    for _, row in df.iterrows():
        vals = np.linspace(row.log_lower, row.log_upper, int(row.freq))
        arrays.append(vals)

    # collect the arrays into a single sample
    log_sample = np.concatenate(arrays)
    return log_sample
```
```
import hinc
income_df = hinc.ReadData()
```
```
log_sample = InterpolateSample(income_df, log_upper=6.0)
log_cdf = thinkstats2.Cdf(log_sample)
thinkplot.Cdf(log_cdf)
thinkplot.Config(xlabel='Household income (log $)',
               ylabel='CDF')
```
```
sample = np.power(10, log_sample)
cdf = thinkstats2.Cdf(sample)
thinkplot.Cdf(cdf)
thinkplot.Config(xlabel='Household income ($)',
               ylabel='CDF')
```
The graphs depict a heavy inbalance towards lower incomes

I then calculate the required values for the sample data
```
mean = sample.mean() # 74278.7
var = sample.var()
std = np.sqrt(var) # 93946.9
median = cdf.Value(0.5) # 51226.4
```
Seems a bit odd that the standard deviation is so much larger than either than median or the mean
To find the fraction of people living below the mean
```
cdf.Prob(mean) # 0.660
```
So 66% of people live below an income of $74,280

Then for the skewness
```
skewness = Skewness(sample) # 4.949
pearson_median_skewness = 3*(mean - median) / std # 0.7361
```
Both depict a heavy skewness to the right

As a bonus, I graphed the PDF to give a better depiction of the actual income distribution
```
pdf = thinkstats2.EstimatedPdf(sample)
thinkplot.Pdf(pdf)
thinkplot.Config(xlabel='Household income ($)',
               ylabel='PDF')
```
By zooming in,
```
xlim=[0, 100000]
```
you can easily identify the mode, mean, and median on the graph with the mode equaling about $21,000 
The mean has about as high a frequency as a $1000 income
