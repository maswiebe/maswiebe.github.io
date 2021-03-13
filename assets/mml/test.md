---
layout: post
title:  "Did medical marijuana legalization reduce crime? A replication exercise"
date:   2021-03-04 20:00:00
type: post
---

Summary
=======
problems
- weighting
- dependent variable: level vs log vs poisson
- event study + differential trends


Introduction
============

This paper (published in Economic Journal) studies the effect of medical marijuana legalization on
crime in the U.S., finding that legalization decreases crime in states that border
Mexico. The paper uses a triple-diff method, essentially doing a
diff-in-diff for the effect of legalization on crime, then adding an
interaction for being a border state.

The paper uses county level data over 1994-2012, with treatment (medical marijuana legalization) occurring at the state level.
The authors use 'violent crimes' as the outcome variable in their main analysis, defined as the sum of homicides, robberies, and assaults, where each is measured as a rate per 100,000 population.
They also perform separate analyses for each of the three crime categories.

The basic triple-diff regression is:

$$ y_{cst} = \beta^{border} D_{st}B_{s} + \beta^{inland} D_{st} (1-B_{s}) + \gamma_{c} + \gamma_{t} + \varepsilon_{cst}.$$

Here $$y_{cst}$$ is the outcome in county $$c$$ in state $$s$$ in year $$t$$; $$D_{st}$$ is an indicator for having enacted MML by year $$t$$; $$B_{s}$$ is an indicator for bordering Mexico; $$\gamma_{c}$$ are county fixed effects; $$\gamma_{t}$$ are year fixed effects. The full model also includes time-varying controls, border-year fixed effects, and state-specific linear time trends.
The outcome is crime rates per 100,000 population, measured in levels, so the regression coefficients will not have a percentage interpretation; we'll come back to this later.

This isn't a standard triple-diff. In this model, $$\beta^{border}$$ is capturing the absolute effect of MML in border states, and not the differential effect relative to inland states. To see this, compare to:

$$ y_{cst} = \beta^{DD} D_{st} + \beta^{DDD} D_{st} \times B_{s} + \gamma_{c} + \gamma_{t} + \varepsilon_{cst}.$$

Here, $$\beta^{DD}$$ represents the effect of MML in inland states, and $$\beta^{DDD}$$ is the differential effect in border states (relative to the effect in inland states). That is, $$\beta^{inland} = \beta^{DD}$$ and $$\beta^{border} = \beta^{DD} + \beta^{DDD}$$.
This is perhaps an issue of taste. What I would primarily want to know is whether MML had a larger effect in border states relative to inland states; the absolute effect in border states is secondary.
Hence, I will report results from the second model (although the differences are small, because the inland effect is small: $$\beta^{inland} = \beta^{DD} \sim 0$$).

The authors find that, on average, MML reduces violent crimes by 35 crimes per 100,000 population, but the estimate is not statistically significant (the standard error is 22).
Then, zooming in on the border states, they find a significant reduction of 108 crimes per 100,000 (and a nonsignificant increase of 2.8 in inland states).
There are three border states that legalized medical marijuana: California, New Mexico, and Arizona. (Texas is the remaining border state.)
Splitting up the effect by treated border state, we have a reduction of 34 in Arizona, 144 in California, and 58 in New Mexico.

- generally: I don't really like this "zoom in on the significance" style of research. We can always find significance if we run enough interactions. And as we zoom in on subgroups, we lose external validity: what would we predict for a state or country that was legalizing marijuana and didn't border on Mexico?
- differential shocks: can't average these out over n=3

<!--
Regression weights
==================
The paper first reports the difference-in-differences estimate, for the average effect of MML on all crimes. This is -35, with a standard error of 22, so not significant.
Then they report a triple diff estimate of -107 for the border states, with three stars.
So this -107 is a weighted average of the effect for border states and the effect for inland states.
Using regression weights (link),
doesn't work: should be: -35 = -107*w_border + 2.8 * w_inland
w_in = 1-wb
-35 = -107*w +2.8 - 2.8w
-37.8 = -109.8w
w= 37.8/109.8 = .34, w_inland = .66
so -35 = -107*.34 + 2.8 * .66
regression weights are not designed for this.
The DD estimate is a variance-weighted average of the heterogeneous effects, but with different weights.
-->

Weighting
=========

The authors use weighted least squares (weighting by population) for their main results.
They justify weighting by performing a Breusch-Pagan test, and finding a positive correlation between the squared residuals and inverse population. This implies larger residuals in smaller counties. In other words, there is heteroskedasticity, and weighting will decrease the size of the standard errors, i.e., increase precision. However, in Appendix Table D7, you'll note that while they get a positive correlation when using homicides and assaults as the dependent variable, this coefficient is negative and nonsignificant for robberies. (And in Table D9, the unweighted robbery estimate has smaller standard errors than the weighted one: weighting is *reducing* precision.) So the robbery results actually should not be weighted!
And yet, the paper still uses weighting when estimating the effect of MML on robberies (in Table 4).
We'll see below that this makes a big difference for the effect size.

Choice of dependent variable
============================

The authors estimate the effect of MML on crime using a level dependent variable measuring crime, instead of taking the logarithm, which I had thought was standard.
In particular, their main results use the aggregate crime rate per 100,000 population. In secondary results, they break this into categories: homicides, robberies, and assaults (all in rates per 100,000 population).

Interpret their results in level terms: MML reduces the crime rate by 100 crimes per 100,000 population.
Normally, we'd use a log-level regression, taking log(y+1) for the dependent variable (adding 1 if there are zeroes in the data), which gives a semi-elasticity interpretation: MML reduces crime by $$100 \times (exp(\beta)-1) \%$$ (which is approximately $$\beta$$ when $$abs(\beta)<0.1$$).
The paper doesn't justify why they don't use a log-level model. This is even more surprising when you see that they manually calculate the semi-elasticity, again without mentioning the log-level approach.

<!-- see Dell drug war paper for use of log(hom+1) ; doesn't actually use it?-->


After spending some time looking into this question of logging the dependent variable for skewed, nonnegative data, I'm still pretty confused.
It seems the options are: (1) level-level regression, as used in this paper; (2) log-level regression; (3) transforming $$y$$ with the inverse hyperbolic sine; and (4) Poisson regression. But it's not clear what the 'correct' approach is.
I'd expect a true result to be robust across multiple approaches, so let's try that here.

I estimate the triple-diff model using a level-level regression (to directly replicate the paper), a log-level regression, and a Poisson regression. (The inverse hyperbolic sine approach is almost identical to log-level, so I skip it here.)
To see how the specification matters, I conduct a specification curve analysis using R's `specr` package. Specifically, I run all possible combinations of model elements, including or excluding covariates, weighting by population, state-specific linear time trends, and border-year fixed effects.
This will allow us to see whether possibly debatable modelling choices, such as state-specific linear trends, are driving the results.
 <!-- eg, including time trends and weighting by population, but excluding covariates. -->

Here are the homicide results, first in the level-level model (as in the paper).
The 'baseline' specification omits the state-specific trends, border-year fixed effects, and doesn't weight by population. The lower part of panel B indicates whether all or no covariates are included in the model. The full covariate list is: an indicator for decriminalization, log median income, log population, poverty rate, unemployment rate, and the fraction of males, African Americans, Hispanics, ages 10-19, ages 20-24. In general, I find that adding controls barely changes the $$R^{2}$$, so these variables aren't adding much beyond the county and year fixed effects.
The full specification (trends + border + weights) includes state-specific linear trends, border-year fixed effects, and weights by population.


#### Level-level model: homicides
![](https://michaelwiebe.com/assets/mml/hom_level.png){:width="80%"}
We can see that the estimate is negative and statistically significant in the full specification, with and without covariates.
Most estimates are nonsignificant; these are generally the unweighted models, indicating the importance of population weighting for these results.

#### Log-level model: homicides
![](https://michaelwiebe.com/assets/mml/hom_log.png){:width="80%"}
Next, in the log-level model, most estimates are insignificant, including the full specification.
Two models even have positive and significant results. Let's see the Poisson model:

#### Poisson model: homicides
![](https://michaelwiebe.com/assets/mml/hom_pois.png){:width="80%"}
Here I use the homicide count (instead of the rate per 100,000 population), though note that the controls include log population and I'm weighting by population.
<!-- In any case, the results are similar with the homicide rate -->
In this case, the estimates are almost exactly zero and nonsignificant in the full specification.
So, the homicide results only go through using the level-level regression, and not in the log-level or Poisson models.


For the other dependent variables, I'll show the graphs in the footnotes.
The results for robberies are more robust. The full specification is negative and significant across all three models.[^1]
However, the assault results are not robust, with the full specification nonsignificant for both log-level and Poisson regressions.[^2]

This doesn't look great for the paper. I'd expect real effects to be robust across the three models. I conclude that at best, the paper provides evidence for an effect of MML on robberies in border states, but not on homicides or assaults. And this is assuming the event study graph looks good for pretrends, which I'll discuss next.

Event study
===========
There are big trends in crime over this period. [Crime fell](https://www.statista.com/statistics/191219/reported-violent-crime-rate-in-the-usa-since-1990/) a lot during the 90s, and again after 2007.
To show that their results aren't driven by these trends, the authors present an event study graph in Figure 6, estimating a triple-diff coefficient in each year. Basically, this is estimating the triple diff for each year relative to an omitted year/period.

The authors perform two analyses, one using the 1994-2012 sample, and one using an extended sample from 1990-2012. The extended sample has issues, because it uses imputed data over 1990-1992, and the year 1993 is missing entirely. But having more pretreatment years is helpful, because California is treated in 1996, leaving only two years for estimating pretrends in the original sample.

Here I will show results from the extended sample, 1990-2012.

They include dummies for relative years -5 to 4, and bin all years 5+ in one dummy.
The omitted years are <-5, in contrast to the standard approach of omitting relative year -1.

Next I reproduce their event study graph, using a level dependent variable. Mine is slightly different because, as noted above, I am estimating the differential effect of MML in border states relative to inland states, while GKZ are estimating the absolute effect.[^3]

#### Event study: violent crimes (binning 5+)
![](https://michaelwiebe.com/assets/mml/es_violent_bin.png){:width="80%"}
<!-- can do higher dpi here -->
This looks pretty similar, but now the coefficients in -3 *and* -5 are negative and significant. This kind of pretreatment noise doesn't inspire confidence.

In any case, note that this graph is for the aggregated violent crime variable. Where are the event studies for the individual dependent variables? *The authors do not show them!* This is a major flaw, and I can't believe that the referees missed it. Even if there are no pretrends in the aggregate variable, what if there are in the component variables? Let's find out.

#### Event study: homicides (binning 5+)
![](https://michaelwiebe.com/assets/mml/es_hom_bin.png){:width="80%"}
First up, using the homicide rate as the dependent variable, we get... a bunch of nothing. It looks like MML had no effect on homicides, which we might expect based on the nonsignificant results we got above using log-level and Poisson models. Now we know why the authors didn't include separate event study graphs by dependent variable.

#### Event study: robberies (binning 5+)
![](https://michaelwiebe.com/assets/mml/es_rob_bin.png){:width="80%"}
Next, the robberies graph looks very similar to the violent crime graph.[^4]

#### Event study: assaults (binning 5+)
![](https://michaelwiebe.com/assets/mml/es_ass_bin.png){:width="80%"}
Finally, for assaults, we see a similar pattern as robberies, but with smaller coefficients.
Recall that 'violent crime' is defined as the sum of homicide, robbery, and assault rates. The averages of these variables are 5, 44, and 265. So clearly the violent crime results will be driven mostly by assaults and robberies, which swamp the null result for homicides.

What do these event studies look like using the log-level or Poisson models? I'll throw them in the footnote.[^5]


<!-- Bacon-goodman: adding years to sample changes DD estimate: more weight on California, since closer to middle; less weight on Ariz, NM, since closer to end -->
<!--
Normally, this omitted year would be period -1 in event time, where the treatment occurs in period 0.
However, the paper doesn't do this. Instead, they seem to use periods outside of the [-5,+5] window as the omitted period. -->

This seems problematic, since the sample starts in 1994 and California is treated in 1996, leaving only two preperiods. So if we omit period -1, there will only be a single preperiod coefficient in the event study to use in evaluating pretrends.

This is not great, because crime fell drastically in California in the 1990s.

I wanted to see what a proper event study graph would look like, so I did it:

They didn't do the event study separately by dependent variable!
And the graph for homicides looks terrible:

graphs: violent rate, homicides, robberies, assaults (all in levels; don't need to show poisson?)

I'm not sure if I did this right.

They only reported the event study for the triple-diff coefficient, and not the diff-in-diff coefficient.

<!-- The authors also use Uniform Crime Reports data to extend their sample back to 1990, but this data is not available in the replication files, so I couldn't check it. -->
  <!-- it is in the original data, just don't drop year<1994 -->
  <!-- no data on 1993 -->


Randomization inference
=======================

The paper calculates a (one-sided) randomization inference p-value of 0.03, and claims that this is evidence for their result being real.
However, as I discuss in [this post](https://michaelwiebe.com/blog/2021/01/randinf), this claim is false. There's no reason to expect RI and standard p-values to differ in this case, so a significant RI p-value provides no additional evidence.

------------------

Footnotes
---------
See here for R code.

[^1]: Robbery results:
    #### Level-level model: robberies
    ![](https://michaelwiebe.com/assets/mml/rob_level.png){:width="80%"}
    In the level-level model, we see a big difference between the weighted and unweighted results. Clearly, there are heterogeneous treatment effects, with larger effects in the higher-weight states (California, probably).
    As I noted above, the robbery estimates should not be weighted.

    #### Log-level model: robberies
    ![](https://michaelwiebe.com/assets/mml/rob_log.png){:width="80%"}

    #### Poisson model: robberies
    ![](https://michaelwiebe.com/assets/mml/rob_pois.png){:width="80%"}


[^2]: Assault results:
    #### Level-level model: assaults
    ![](https://michaelwiebe.com/assets/mml/ass_level.png){:width="80%"}

    #### Log-level model: assaults
    ![](https://michaelwiebe.com/assets/mml/ass_log.png){:width="80%"}

    #### Poisson model: assaults
    ![](https://michaelwiebe.com/assets/mml/ass_pois.png){:width="80%"}

[^3]: I also drop counties that have the black share of population greater than 100%. It seems the authors were doing some extrapolation that got out of control.

[^4]: Recall that the Breusch-Pagan test failed to justify weighting in the case of robberies. What does the event study graph look like if we don't weight by population?
    #### Event study: robberies, unweighted (binning 5+)
    ![](https://michaelwiebe.com/assets/mml/es_rob_bin_unw.png){:width="80%"}
    Now the effect size is smaller, and it looks more like a negative trend is driving the result: robberies were decreasing in treated border states before MML was implemented.

[^5]: Log-level results:
    #### Event study (log-level): homicides, (binning 5+)
    ![](https://michaelwiebe.com/assets/mml/es_lhom_bin.png){:width="80%"}

    #### Event study (log-level): robberies, unweighted (binning 5+)
    ![](https://michaelwiebe.com/assets/mml/es_lrob_bin.png){:width="80%"}

    #### Event study (log-level): assaults, (binning 5+)
    ![](https://michaelwiebe.com/assets/mml/es_lass_bin.png){:width="80%"}

    Poisson results:
    #### Event study (Poisson): homicides, (binning 5+)
    ![](https://michaelwiebe.com/assets/mml/es_phom_bin.png){:width="80%"}

    #### Event study (Poisson): robberies, unweighted (binning 5+)
    ![](https://michaelwiebe.com/assets/mml/es_prob_bin.png){:width="80%"}

    #### Event study (Poisson): assaults, (binning 5+)
    ![](https://michaelwiebe.com/assets/mml/es_pass_bin.png){:width="80%"}
