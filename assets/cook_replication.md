---
layout: post
title:  "Replicating Cook (2014)"
date:   2020-11-04 23:20:00
type: post
---

substantial contribution in data collection, rich qualitative evidence, raising the research question, inspiring future research (cite brookings)

Time series regressions
-----------------------

Cook has two measures of patents per year: (1) using the year the patent was applied for, and (2) using the year the patent was granted. 
Figure 1 reports Black (and white) patents per million using grant-year, while Figure 2 shows Black patents per million using application year.
Comparing the two graphs, we immediately see that the scale differs by a factor of about 10.
Here I merge the two datasets and plot the application-year and grant-year variables on the same graph.

![](https://michaelwiebe.com/assets/cook_replication/fig_1_2.png){:width="80%"}

There is a huge discrepancy between the two patent variables.
Cook collected data on 726 patents over 1870-1940, but the average by grant-year is 0.16, while the average by application-year is 1.22.[^1]
Cook's replication data does not include the raw patent or population variables, so we can't say for sure what's going on here.
But the average [Black population](https://www.census.gov/content/dam/Census/library/working-papers/2002/demo/POP-twps0056.pdf) (see Table 1) was roughly 10 million, and 0.16 patents/M * 10M * 71 years / 1M  = 114, far less than the 726 patents recorded.
One possible explanation is that Cook used the white population (average 75 million) in the denominator, giving 0.16 * 75 * 71 = 852 patents, which is closer to 726.

If the grant-year patent variable is incorrect, this will affect the Table 6 results, which use the grant-year variable. Cook doesn't report a robustness check, so I will re-run the Table 6 regressions using application-year patents. In fact, this specification seems more appropriate, since Cook's mechanism is that racial violence deters innovation by Black inventors; so racial violence would first impact patent applications, and with a lag impact granted patents.

First, I am able to reproduce Cook's Table 6, using grant-year patents[^2]:

![](https://michaelwiebe.com/assets/cook_replication/table6a.png){:width="80%"}

The application-year patent variable is missing in 1940, which reduces the sample size for the robustness check by 1. To make a pure comparison, I re-run the grant-year regressions dropping 1940, and get similar results (see footnote).[^3]

Next, I redo Table 6 using application-year patents.

![](https://michaelwiebe.com/assets/cook_replication/table6c.png){:width="80%"}

The results are dramatically different: the negative effect of lynchings and riots disappears, as does the negative effect in 1921.
If the grant-year patent variable is incorrect and the application-year variable is correct, then the paper's main result is questionable.

Panel data regressions
----------------------

In Tables 7 and 8, Cook uses state-level panel data over 1870-1940 to run regressions of patents on lynching rates and riots.
However, we can immediately see a problem: there are 49 states and 71 years in the data, but only N=430 observations. A balanced panel would have 49 * 71 = 3479 observations. So Cook is using 430/3479 = 12% of the full sample. 

[//]: # Hence, before looking at the results, we already know that they are not externally valid for the U.S. over 1870-1940.

And the pattern of missing data is not random. 
Below I plot the number of observations by state and year. We see that the majority of states have fewer than 10 observations, while the sample size is increasing up to 1900 before dropping off and rising again starting in 1920.
![](https://michaelwiebe.com/assets/cook_replication/obs_state.png){:width="80%"}
![](https://michaelwiebe.com/assets/cook_replication/obs_year.png){:width="80%"}

Decomposing by region, we see that the Midwest and Mid-Atlantic regions are relatively overrepresented, while the West is relatively underrepresented.[^4]
![](https://michaelwiebe.com/assets/cook_replication/obs_region.png){:width="80%"}

This nonrandom pattern of missingness, combined with the low coverage of the full sample, severely undermines the external validity of the results. This dataset is not representative of the U.S. over 1870-1940.

To make this concrete, consider the riots variable. There are 35 riots in the time series data, but only 5 in the panel data. 
Similarly, there are 290 new segregation laws in the time series data, but only 19 in the panel data.[^5]
(The replication files don't have count data on lynchings, but the same problem applies there as well.)

Hence, it seems a near-certainty that the Table 7 and 8 estimates would be different if we ran the regressions using a balanced panel.
In other words, the state-level results are 'dead on arrival', and are not externally valid for the U.S. over 1870-1940.

[//]: # Cook interprets the riots estimate as follows: "One additional riot in a given state in a given year would diminish the state total by an average of nearly half a patent or by 17 patents in a given year for all states." 
[//]: # But it's very likely that this interpretation would be different if the regression included all 35 riots.

---------
To summarize, the main results in Cook (2014) do not hold up under scrutiny. 
Nonetheless, the conclusions remain plausible, because they have a high prior probability. Race riots and lynchings were a severe problem, and it would be astonishing if they didn't have pervasive effects on the lives of Black people.

But with the data available, statistically detecting causal effects is unrealistic; credible causal inference would require more complete data as well as a better identification strategy. Descriptive analysis is the most that this dataset can support, and is a valuable contribution in itself. Cook deserves credit for pursuing this important research question and putting in the work to collect the patent data. Hopefully her example can inspire future researchers to build upon this work and bring attention to the consequences of America's racist history (and it [already has](https://www.brookings.edu/research/the-black-innovators-who-elevated-the-united-states-reassessing-the-golden-age-of-invention/).

---------
In terms of computational reproducibility, Cook's code has a bunch of errors:
- The code for Table 6 refers to a variable `LMRindex`, but the dataset contains `DLMRindex`.
- The code for Table 7 includes a variable, `estbnumpc`, for the number of firms per capita, but it is not included in the dataset.
- The code for Column 1 in Table 7 erroneously includes the 'number of firms' variable, which in the table is only included in columns 3-6.
- In the notes to Tables 7 and 8, Cook writes that "Standard errors robust to clustering on state and year are in parentheses." However, the code only clusters by state, using `vce(stateno)`.
- The code for Table 9 does not reproduce the results in the paper.

There are also a few data errors:
- State 9 has the South dummy equal to 1 for all years, but also has the Mid-Atlantic dummy equal to 0.33 in 1888.
- State 14 has the Midwest dummy equal to 1 in all years except 1886, when both it and the South dummy are 0.5.
- State 31 in 1909 has a value of 0.333333 for 'number of new segregation laws', which should be integer-valued.

Both Cook and the Journal of Economic Growth must do better at publishing reproducible research.

-----------------

Footnotes
---------
See [here](https://github.com/maswiebe/metrics/blob/main/) for  code.

[^1]: Cook notes that "a comparison of a sample of similar patents obtained by white and African American inventors shows that the time between patent application and grant for the two groups was not significantly different, 1.4 years in each case." (p.226, fn. 15) Also, there is no application-year patent data for 1870-72.

[^2]: Cook's Table 6 incorrectly shows the lynching estimates in Columns 2 and 3 as having p-values less than 0.05.

[^3]: ![](https://michaelwiebe.com/assets/cook_replication/table6b.png){:width="80%"}

[^4]: Number of states by region: South 15, Midwest 12, Northeast 6, West 12, Mid-Atlantic 7.

[^5]: The actual number is 19.33. Somehow, one state-year observation has a value of 0.33 for the number of new segregation laws.
