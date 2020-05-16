[Think Stats Chapter 9 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2010.html#toc90) (resampling)

Import the stuff
```
from __future__ import print_function, division

%matplotlib inline

import numpy as np

import random

import thinkstats2
import thinkplot
import first
```
Define the Hypthesis Test Class
```
class HypothesisTest(object):

    def __init__(self, data):
        self.data = data
        self.MakeModel()
        self.actual = self.TestStatistic(data)

    def PValue(self, iters=1000):
        self.test_stats = [self.TestStatistic(self.RunModel()) 
                           for _ in range(iters)]

        count = sum(1 for x in self.test_stats if x >= self.actual)
        return count / iters

    def TestStatistic(self, data):
        raise UnimplementedMethodException()

    def MakeModel(self):
        pass

    def RunModel(self):
        raise UnimplementedMethodException()
```
And DiffMeansPermute
```
class DiffMeansPermute(thinkstats2.HypothesisTest):

    def TestStatistic(self, data):
        group1, group2 = data
        test_stat = abs(group1.mean() - group2.mean())
        return test_stat

    def MakeModel(self):
        group1, group2 = self.data
        self.n, self.m = len(group1), len(group2)
        self.pool = np.hstack((group1, group2))

    def RunModel(self):
        np.random.shuffle(self.pool)
        data = self.pool[:self.n], self.pool[self.n:]
        return data
```
Bring in the data
```
live, firsts, others = first.MakeFrames()
data_lngth = firsts.prglngth.values, others.prglngth.values
data_wgt = firsts.totalwgt_lb.values, others.totalwgt_lb.values
```
Create the DiffMeansResample class
```
class DiffMeansResample(DiffMeansPermute):
    
    def RunModel(self):
        sample = np.random.choice(self.pool, len(self.pool), replace=True)
        data = sample[:self.n], sample[self.n:]
        return data
```
Run the data through it
```
ht = DiffMeansResample(data_lngth)
pvalue = ht.PValue()
pvalue # 0.179
```
```
ht = DiffMeansResample(data)
pvalue = ht.PValue()
pvalue # 0.0
```
These are just about the same values you get when running the data through DiffMeansPermute so the difference between the two is miniscule as they're doing very similar things.
