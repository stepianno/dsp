[Think Stats Chapter 8 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77) (scoring)

Import the goodies
```
from __future__ import print_function, division

%matplotlib inline

import numpy as np

import brfss
import random
import thinkstats2
import thinkplot
```
The RMSE function computes the RMSE given a list of estimates and the known quantity
```
def RMSE(estimates, actual):
    e2 = [(estimate-actual)**2 for estimate in estimates]
    mse = np.mean(e2)
    return np.sqrt(mse)
```
Next I make a function that computes an estimate for Lambda in an exponential distribution with the actual lambda equal to 2. To do this, I take a sample size of 10 and create random distributions 1000 times and average the mean of each of them. It also converts the list of means into a CDF and graphs them to visualize
```
def EstimateL_graph(n=10, iters=1000):
    lam = 2

    means = []
    for _ in range(iters):
        xs = np.random.exponential(1.0/lam, n)
        L = 1 / np.mean(xs)
        means.append(L)

    cdf = thinkstats2.Cdf(means)
    print('Standard Error L', RMSE(means, lam))
    print('90% CI L', cdf.Percentile(5), cdf.Percentile(95))
    
    thinkplot.Cdf(cdf)
    thinkplot.Config(xlabel='Estimated Lambda', ylabel='CDF')
    
EstimateL_graph()
```
Upon running the code, I came up with a standard error of 0.770 and a confidence interval of 1.305 to 3.634. The CDF graph depicts a typical exponential distribution with the median right around 2.  
  
Next I created a similar function which returns the RMSE for the estimate. I used a for loop to run the code for a range of sample sizes and graphed the sample sizes against their respective RMSE.
```
def EstimateL(n=10, iters=1000):
    lam = 2

    means = []
    for _ in range(iters):
        xs = np.random.exponential(1.0/lam, n)
        L = 1 / np.mean(xs)
        means.append(L)
    return RMSE(means, lam)

rmse = []
n = list(range(5, 26))
for i in n:
    rmse.append(EstimateL(n=i))
thinkplot.Plot(n, rmse)
thinkplot.Config(xlabel='Sample Size, n', ylabel='RMSE')
```
The resulting graph depicts a very high error for smaller values (~1.5 for n=5) and small errors for larger samples (~0.45 for n=25), with a huge slope for smaller sample sizes and a much smaller slope amongst larger sample sizes.
