[Think Stats Chapter 8 Exercise 3](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77)


Start by importing what we need
```
from __future__ import print_function, division

%matplotlib inline

import numpy as np

import random

import thinkstats2
import thinkplot
```
```
def RMSE(estimates, actual):
    """Computes the root mean squared error of a sequence of estimates.

    estimate: sequence of numbers
    actual: actual value

    returns: float RMSE
    """
    e2 = [(estimate-actual)**2 for estimate in estimates]
    mse = np.mean(e2)
    return np.sqrt(mse)
```
```
def MeanError(estimates, actual):
    """Computes the mean error of a sequence of estimates.

    estimate: sequence of numbers
    actual: actual value

    returns: float mean error
    """
    errors = [estimate-actual for estimate in estimates]
    return np.mean(errors)
```
The SimulateGame function predicts the number of goals scored in a game given a goals per game rate.
```
def SimulateGame(lam):
    goals = 0
    t = 0
    while True:
        time_between_goals = random.expovariate(lam)
        t += time_between_goals
        if t > 1:
            break
        goals += 1
    L = goals
    return L
```
Next, I write a function which runs SimulateGame to simulate a thousand games with a given goal-scoring rate in order to predict the goal scoring rate for those games. The function returns the bias and the RMSE of the predictions given this approach.
```
def games(L=3):
    games = []
    for _ in range(1000):
        games.append(SimulateGame(L))
    return (MeanError(games, L), (RMSE(games, L)))
```
Lastly, I was curious to see how increasing the goal scoring rate would increase the RMSE, so I ran it for a range of values and plotted the resulting RMSEs against the range
```
n = list(range(1,6))
rmse = []
for i in n:
    stats = games(i)
    rmse.append(stats[1])
    
thinkplot.Plot(n, rmse)
thinkplot.Config(xlabel='Goals per Game', ylabel='RMSE')
```
The results are clear: the larger the rate of goals scored, the larger the RMSE of the game prediction.
