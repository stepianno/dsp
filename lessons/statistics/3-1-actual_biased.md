[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

```
%matplotlib inline

import numpy as np

import nsfg
import first
import thinkstats2
import thinkplot
```
Import all our modules
```
def BiasPmf(pmf, label):
    new_pmf = pmf.Copy(label=label)

    for x, p in pmf.Items():
        new_pmf.Mult(x, x)
        
    new_pmf.Normalize()
    return new_pmf
```
Define the Bias function
```
resp = nsfg.ReadFemResp()
```
Get the data
```
numkdhh = resp.numkdhh
pmf = thinkstats2.Pmf(numkdhh, label='kids per family')
```
The part we care about along with the probability of each value
```
biased_pmf = BiasPmf(pmf, label='observed')
```
Find the biased probability
```
thinkplot.PrePlot(2)
thinkplot.Pmfs([pmf, biased_pmf])
thinkplot.Config(xlabel='Class size', ylabel='PMF')
```
Graph the two probabilities along side one another
```
pmf.Mean() #1.0
biased_pmf.Mean() # 2.4
```
Obviously, the biased probability of more kids per family is way higher than the actual probability
