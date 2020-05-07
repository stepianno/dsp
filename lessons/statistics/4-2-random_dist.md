[Think Stats Chapter 4 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2005.html#toc41) (a random distribution)

Get Numpy
``` 
import numpy as mp
```
Make the random distributions to be plotted; both the CDF and the PMF
```
random = np.random.random(1000)
random_cdf = thinkstats2.Cdf(random)
random_pmf = thinkstats2.Pmf(random)
```
And graph the data
```
thinkplot.PrePlot(2, cols=2)
thinkplot.Cdf(random_cdf)
thinkplot.Config(xlabel='Random Number', ylabel='Percentile')

thinkplot.SubPlot(2)
thinkplot.Pmf(random_pmf)
thinkplot.Config(xlabel='Random Number', ylabel='Probability')
```
The CDF plot shows a nearly straight line with a slope of 1 suggesting that the distribution is evenly spread.
The PMF plot shows a square demonstrating that every value from 0 to 1 has exactly a probability of 0.0010 %.

The CDF in this instance is far more palatable as it's much easier on the eyes and the kinks in the line show glimpses at uneven distribution.
The PMF, on the other hand, is not such a great sight as it's just a box.
