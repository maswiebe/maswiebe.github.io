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

![](https://michaelwiebe.com/assets/cook_replication/fig_1_2.png){:width="100%"}

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

In Tables 7 and 8, Cook uses state-level panel data over 1870-1940 to run regressions of patents on lynching rates, riots, and segregation laws.
However, we can immediately see a problem: there are 49 states and 71 years in the data, but only N=430 observations. A balanced panel would have at least 37 * 71 = 2627 observations, since the [number of states](https://www.statista.com/statistics/1043617/number-us-states-by-year/) was 37 in 1870, growing to 48 in 1940. (I'm not sure why Cook has 49 states, when Alaska wasn't [added](https://en.wikipedia.org/wiki/List_of_U.S._states_by_date_of_admission_to_the_Union) until 1959.)
So as an upper bound estimate, Cook is using 430/2627 = 16% of the full sample.

And the pattern of missing data is not random. 
Below I plot the number of observations by state and year. 
First, we see that the majority of states have fewer than 10 observations over 71 years.
![](https://michaelwiebe.com/assets/cook_replication/obs_state.png){:width="80%"}

Next, the sample size is increasing up to 1900 before dropping off and rising again starting in 1920.
![](https://michaelwiebe.com/assets/cook_replication/obs_year.png){:width="80%"}

Decomposing by region, we see that the Midwest and Mid-Atlantic regions are relatively overrepresented, while the West is relatively underrepresented.[^4]
![](https://michaelwiebe.com/assets/cook_replication/obs_region.png){:width="90%"}

Moreover, consider how this imbalanced panel compares to the full time series.
There are 726 patents in the time series, and 702 in the panel data (for 97% coverage).
But the violence variables are drastically under-reported: there are 35 riots in the time series data, but only 5 in the panel data (14%).
Similarly, there are 290 new segregation laws in the time series data, but only 19 in the panel data (7%).[^5]
(The same problem applies with lynchings, but the replication files don't have count data, so we can't quantify it.)

This nonrandom pattern of missingness, combined with the low coverage of the full sample, casts doubt on the reliability of the results.



It's possible that the results from this limited sample would remain stable as the missing data was filled in. But I'm skeptical. 
Statistically significant results are easy to get when you're working with small effects and noisy data.

For example, let's check for heterogeneous effects by region.
From Table 7, I run the Column 1 regression separately for each region:

![](https://michaelwiebe.com/assets/cook_replication/table7_region.png){:width="100%"}

The lynchings estimate for the South is similar to the average effect (-0.075 in the subsample, -0.058 in the full sample).
But there's no estimate at all for the Midwest and Northeast, since there were zero lynchings in those regions.
The estimate for the Mid-Atlantic is huge with two stars, 200x bigger than the South estimate. But this is almost certainly a [Type M](https://cran.r-project.org/web/packages/retrodesign/vignettes/Intro_To_retrodesign.html) error, as the lynching rate for the Mid-Atlantic is 3% of the average.

For the riots estimates, with only 5 riots in the dataset, it's no surprise that there's no estimate for the Midwest, Northeast, or West regions (which had zero riots in this data). The effect size is similar for the South and Mid-Atlantic, perhaps indicating a more homogeneous effect of riots on patenting.

For segregation laws, the Table 7, Column 1 estimate is -0.1. The effects for the South and West are in the ballpark, at -0.19 and -0.16. But the effects for the Midwest and Mid-Atlantic are positive, massive, and have three stars! 
But statistical significance doesn't mean anything here, because the data is noisy.
There are 19.33 new segregation laws in the data, with 17 in the South, 1 in the Midwest, 1 in the West, and 0.33 in the Mid-Atlantic (presumably a data error).

My takeaway is that statistical power is low, and we're seeing [Type S and Type M errors](https://cran.r-project.org/web/packages/retrodesign/vignettes/Intro_To_retrodesign.html). 
And if we're disregarding statistically significant subsample results, we should also be skeptical of the full sample results based on a highly imbalanced panel.

<!-- Hence, it seems a near-certainty that the Table 7 and 8 estimates would be different if we ran the regressions using a balanced panel.
In other words, the state-level results are 'dead on arrival', and are not externally valid for the U.S. over 1870-1940. -->

[//]: # Cook interprets the riots estimate as follows: "One additional riot in a given state in a given year would diminish the state total by an average of nearly half a patent or by 17 patents in a given year for all states." 
[//]: # But it's very likely that this interpretation would be different if the regression included all 35 riots.

---------
To summarize, the main time series result in Cook (2014) does not hold up to a robustness check, and the panel data results are questionable.
Nonetheless, the conclusions remain plausible, because they have a high prior probability. Lynchings, race riots, and segregation laws were a severe problem, and it would be astonishing if they didn't have pervasive effects on the lives of Black people.

But with the data available, it's unrealistic to think we can statistically detect causal effects; credible causal inference would require more complete data as well as a better identification strategy than a panel regression. Descriptive analysis is the most that this dataset can support, and is a valuable contribution in itself. Cook deserves credit for pursuing this important research question and putting in the work to collect the patent data. Hopefully her example can inspire future researchers to build upon this work and bring attention to the consequences of America's racist history (and it [already has](https://www.brookings.edu/research/the-black-innovators-who-elevated-the-united-states-reassessing-the-golden-age-of-invention/)).

---------
In terms of computational reproducibility, Cook's code has several errors:
- The code for Table 6 refers to a variable `LMRindex`, but the dataset contains `DLMRindex`.
- The code for Table 7 includes a variable, `estbnumpc`, for the number of firms per capita, but it is not included in the dataset.
- The code for Column 1 in Table 7 includes the 'number of firms' variable, but the paper only includes it in columns 3-6.
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
