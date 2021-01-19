---
layout: post
title:  "P-values"
date:   2020-11-04 23:20:00
type: post
---
In a [previous post](https://michaelwiebe.com/blog/2020/11/pvalues), I discussed how p-values involve the thought experiment of running the exact same test on many samples of data.
When designing a test, researchers need to follow a procedure that is consistent with this thought experiment. In particular, they need to design the test independently of the data; this guarantees that they would run the same test on different samples.
As [Gelman and Loken](https://stat.columbia.edu/~gelman/research/published/ForkingPaths.pdf) put it: "For a p-value to be interpreted as evidence, it requires a strong claim that the same analysis would have been performed had the data been different."

In this post, I discuss an example of a researcher using a different dataset to study the same research question.
Will the researcher perform the same analysis when the data is different?
<!-- This is a convenient test case to see whether the same analysis is performed when the data is different, and hence, whether the original p-values should count as evidence. -->
Read on to find out...

----------
In my [last post](), I discussed my research replicating the literature on meritocratic promotion in China. The basic idea is that politicians compete in a promotion tournament, where politicians with higher GDP growth are rewarded with promotion up the political hierarchy. Since meritocratic promotion creates strong incentives to boost GDP, this is a potential explanation for China's rapid economic growth.

In one paper, [Yao and Zhang (2015)](https://sci-hub.st/https://link.springer.com/article/10.1007/s10887-015-9116-1), the analysis looked suspiciously like the garden of forking paths. The authors first estimate a leader's ability to boost growth, then test whether ability is correlated with promotion. But they find no effect, contradicting the idea of meritocratic promotion.

But they don't frame their results as evidence against the literature. Instead, they go on to interact ability with age, and find a significant positive interaction effect.
They conclude that leader ability matters for older politicians, because more years of experience produces a clearer signal of ability.

Had the average effect been significant, would the authors have still tested an interaction with age? I doubt it. The age effect is poorly motivated and partly contradicts the idea of meritocratic promotion.
Their interpretation implies that only old leaders are rewarded with promotion, in contrast to the simple model of all leaders competing to boost growth. This is a major discrepancy, since half of all promotions occur for 'young' leaders.
<!-- They interpret this finding as leaders having noisy signals of ability, which become detectable only after many years of experience. -->


<!-- need this here? this will go into the replication post -->

As mentioned in my last post, this interaction effect isn't robust. The authors include several control variables based on weak or no justification.
When I exclude these controls and re-run the analysis, the interaction with age loses significance.
So overall, I have problems with this paper on both interpretation and estimation.
<!-- - footnote? -->

<!-- My guess as to what happened here: the authors set out to find a positive correlation between ability and promotion, as you'd expect based on the promotion literature. When they didn't find one, they searched around for an interaction, and found statistical significance when interacting with age. Then they made up a sloppy rationalization of this result, and sent it off to the journals. -->

<!-- Question: why not embrace the null result? why conform/shoe-horn to the consensus in the literature? -->
<!-- I would publish this paper based on pre-results review; it had a good empirical method. The null result is interesting, since it upends the consensus. -->
<!-- put this in the replication post? -->

----------
Now, as it happens, Yao has recently posted a [working paper](https://www.semanticscholar.org/paper/The-Competence-Loyalty-Tradeoff-in-China%E2%80%99s-Wang-Yao/e43c2d1adff340d9c79ba15da6071f7f913a61d6) re-using the method in Yao and Zhang (2015).
Like the first paper, the new one also studies how ability affects promotion for prefecture-level leaders, using the same approach to estimate leader effects. Importantly, they update their data on prefecture cities by extending the time series from 2010 to 2017.

Thus, we have a perfect test case to see whether the same data-analysis decisions would be made when studying the same question and using a different dataset (drawn from the same population).

It turns out that the new paper doesn't interact with age at all!
Instead, it reports the average effect of ability on promotion, which is now significant, along with a new specification where ability is interacted with political connections (see Table 2).

So the p-value requirement is not satisfied: the researcher performs different analyses when the data is different.
Hence, our skepticism of original age interaction turns out to be justified. 
Since the researcher would not run the same test on new samples, the significant p-value is actually invalid.
