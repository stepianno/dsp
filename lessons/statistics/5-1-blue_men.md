[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

```
import scipy.stats
```
As provided
```
mu = 178
sigma = 7.7
dist = scipy.stats.norm(loc=mu, scale=sigma)
```

I convert inches to centimeters in order to fit the data and then subtract the minimum height cdf from the maximum height cdf to return the fraction of the population that fits into this window
```
min_h = 70*2.54
max_h = 73*2.54
dist.cdf(max_h) - dist.cdf(min_h)
```
The return is that roughly 34% of men are eligible to apply for the Blue Man Group based on height restrictions assuming an average height of 178 cm and a standard deviation of 7.7 cm with a normal distribution
