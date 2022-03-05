---
layout: post
title:  "Replicating Cook (2014)"
date:   2020-11-04 23:20:00
type: post
---

substantial contribution in data collection, qualitative evidence, raising the research question, inspiring future research (cite brookings)

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

If the grant-year patent variable is incorrect, this will affect the Table 6 results, which use the grant-year variable. Cook doesn't report a robustness check, so I will re-run the Table 6 regressions using application-year patents. In fact, this specification seems more appropriate, since Cook's mechanism is that racial violence deters innovation by Black inventors; so racial violence would first impact patent applications, and with a lag impact patent grants.

First, I am able to reproduce Cook's Table 6, using grant-year patents:

![](https://michaelwiebe.com/assets/cook_replication/table6a.png){:width="80%"}

The application-year patent variable is missing in 1940, which reduces the sample size for the robustness check by 1. To make a pure comparison, I re-run the grant-year regressions dropping 1940, and get similar results (see footnote).[^2]

Next, I redo Table 6 using application-year patents.

![](https://michaelwiebe.com/assets/cook_replication/table6c.png){:width="80%"}

The results are dramatically different: the negative effect of lynchings and riots disappears, as does the negative effect in 1921.
If the grant-year patent variable is incorrect and the application-year variable is correct, then the paper's main result is questionable.



-----------------

Footnotes
---------
See [here](https://github.com/maswiebe/metrics/blob/main/) for  code.

[^1]: Cook notes that "a comparison of a sample of similar patents obtained by white and African American inventors shows that the time between patent application and grant for the two groups was not significantly different, 1.4 years in each case." (p.226, fn. 15) Also, there is no application-year patent data for 1870-72.

[^2]: ![](https://michaelwiebe.com/assets/cook_replication/table6b.png){:width="80%"}
