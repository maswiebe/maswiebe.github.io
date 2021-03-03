---
layout: post
title:  "replication exercise"
date:   2021-03-04 20:00:00
type: post
---

Choice of dependent variable
============================

The authors estimate the effect of MML on crime using a level dependent variable measuring crime, instead of taking the logarithm, which I had thought was standard.
In particular, their main results use the aggregate crime rate per 100,000 population. In secondary results, they break this into categories: homicides, robberies, and assaults (all in rates per 100,000 population).

Interpret their results in level terms: MML reduces the crime rate by 100 crimes per 100,000 population.
Normally, we'd use a log-level regression, taking log(y+1) for the dependent variable (adding 1 if there are zeroes in the data), which gives a semi-elasticity interpretation: MML reduces crime by $$100 \times (exp(\beta)-1) \%$$ (which is approximately $$\beta$$ when $$abs(\beta)<0.1$$).
The paper doesn't justify why they don't use a log-level model. This is even more surprising when you see that they manually calculate the semi-elasticity, again without mentioning the log-level approach.

<!-- see Dell drug war paper for use of log(hom+1) -->

After spending some time looking into this question of logging the dependent variable for skewed, nonnegative data, I'm still pretty confused.
It seems the options are: (1) level-level regression, as used in this paper; (2) log-level regression; (3) transforming $$y$$ with the inverse hyperbolic sine; and (4) Poisson regression. But it's not clear what the 'correct' approach is.
I'd expect a true result to be robust across multiple approaches, so let's try that here.

I estimate the triple-diff model using a level-level regression (to directly replicate the paper), a log-level regression, and a Poisson regression. (The inverse hyperbolic sine approach is almost identical to log-level, so I skip it here.)
To see how the specification matters, I conduct a specification curve analysis using R's `specr` package. Specifically, I run all possible combinations of model elements, including or excluding covariates, weighting by population, state-specific linear time trends, and border-year fixed effects.
This will also us to see whether possibly debatable modelling choices, such as including state-specific linear time trends, are driving the results.
 <!-- eg, including time trends and weighting by population, but excluding covariates. -->

Here are the homicide results, first in the level-level model (as in the paper).
Level-level model
=================
![](https://michaelwiebe.com/assets/mml/hom_level.png){:width="80%"}
We can see that the estimate is negative and statistically significant in the main specification (trends + border + weights, all covariates).

Next, the log-level model:
![](https://michaelwiebe.com/assets/mml/hom_log.png){:width="80%"}
Now, most estimates are insignificant and zero or positive, including the main specification.
Two models even have positive and significant results.

Finally, the Poisson model, using the homicide count (instead of the rate per 100,000 population):
![](https://michaelwiebe.com/assets/mml/hom_pois.png){:width="80%"}
Here, the estimates are almost exactly zero and nonsignificant in the main specification.
So it seems that the homicide results only go through using the level-level regression.
This leads me to doubt whether the paper's results are actually real.
