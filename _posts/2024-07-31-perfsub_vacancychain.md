---
layout: post
title: "Using microeconomics to model vacancy chains"
date:   2024-07-31 17:00:00
type: post
---
<!-- link to published html notebook -->

When we build new apartments, the people moving in vacate their old apartments; this reduces competition for old housing, making it more affordable. 
This is the vacancy chain effect: building expensive new housing helps improve affordability of cheaper old housing.
In this post I'll show how to model the vacancy chain effect using supply and demand.

Let's consider an example of n=3 consumers with [perfect substitutes](https://en.wikipedia.org/wiki/Substitute_good#Perfect_substitutes) preferences. 
People choose between Old and New apartments, and have a housing budget to spend. 
They differ in how much they prefer New vs Old apartments, and in the size of their housing budget.
With perfect substitutes, you choose one good or the another, and spend your entire budget on the chosen good.
For example, Person 1 will buy Old if price(Old) is less than half of price(New). 
Conversely, they buy New if price(New) is less than double price(Old).

Here are the individual demand curves for Old apartments.
Preference for Old (vs New) is decreasing from Person 1 to Person 3, while budget size is increasing.
Since New apartments are better, everyone gets more utility from a New apartment compared to an Old one, but they differ in *how much* more.
![](https://michaelwiebe.com/assets/vacancy_chain/indl_demand_oldapt.png){:width="100%"}

Demand for Old apartments is equal to 0 above some price threshold (here the demand curve overlaps the y-axis), is indeterminate at the threshold, and is equal to budget/price below it (recall the shape of a 1/x function).
The price threshold determines which good the consumer chooses; if price(Old) is too high, they choose a New apartment, and if price(Old) is low enough, they choose an Old apartment, and spend their whole budget on it.
Technically, the demand function has a discontinuity at the threshold, so you can imagine the horizontal part as being a dotted line.
(Note that I'm referring to quantity demanded as a function of price, even though price is on the y-axis. 
Here I'm following the convention to plot price against quantity.)
<!-- note: inverse demand function; thinking of Q(p), but have p on y-axis -->

{::options parse_block_html="true" /}

<details>
<summary markdown="span">Math</summary>

<!-- need $$ for inline math on kramdown (?), even though it renders as equations here -->
Let $$x_{1}$$ = Old apartments, $$x_{2}$$ = New apartments.
Let $$x_{i,j}$$ be quantity demanded for consumer $$i$$ of good $$j$$.
Given perfect substitutes utility, we can derive the demand functions with the threshold defined by equating the marginal rate of substitution with the price ratio.

$$
\begin{aligned}
\begin{split}
&u_{1}(x_{1},x_{2}) = x_{1} + 2x_{2} \\
&u_{2}(x_{1},x_{2}) = x_{1} + 3x_{2} \\
&u_{3}(x_{1},x_{2}) = x_{1} + 4x_{2} \\

&m_{1} = 10 \\
&m_{2} = 20 \\
&m_{3} = 30 
\end{split}
\end{aligned}

\begin{aligned}
\begin{split}
&x_{1,1} = \bigg\{ 
    \begin{matrix}
    0, p_{1} > p_{2}/2 \\
    m_{1}/p_{1}, p_{1} \leq p_{2}/2 \\
    \end{matrix}
&x_{1,2} = \bigg\{ 
    \begin{matrix}
    m_{1}/p_{2}, p_{1} > p_{2}/2 \\
    0, p_{1} \leq p_{2}/2 \\
    \end{matrix}

&x_{2,1} = \bigg\{ 
    \begin{matrix}
    0, p_{1} > p_{2}/3 \\
    m_{1}/p_{1}, p_{1} \leq p_{2}/3 \\
    \end{matrix}
&x_{2,2} = \bigg\{ 
    \begin{matrix}
    m_{1}/p_{2}, p_{1} > p_{2}/3 \\
    0, p_{1} \leq p_{2}/3 \\
    \end{matrix}

&x_{3,1} = \bigg\{ 
    \begin{matrix}
    0, p_{1} > p_{2}/4 \\
    m_{1}/p_{1}, p_{1} \leq p_{2}/4 \\
    \end{matrix}
&x_{3,2} = \bigg\{ 
    \begin{matrix}
    m_{1}/p_{2}, p_{1} > p_{2}/4 \\
    0, p_{1} \leq p_{2}/4
    \end{matrix}
\end{split}
\end{aligned}


$$

</details>

{::options parse_block_html="false" /}

What does it mean to demand multiple apartments? 
A more complicated model would have unit-demand (ie. people demand either one Old or one New apartment), but it's simpler to use continuous quantity to start.
You can interpret it as the square footage of the apartment, which is a continuous variable.
Or you can wait until we get to aggregate demand, and assume a large number of consumers.

We get aggregate demand by summing up the individual demand curves: for each price, count the quantity demanded across consumers.
This produces downward-sloping segments where we can identify which consumers contribute to aggregate demand.
Since Person 1 has the strongest preference for Old apartments, they show up at the top of the demand curve, while Person 3 (with the weakest preference) shows up only at the bottom.
![](https://michaelwiebe.com/assets/vacancy_chain/agg_demand_oldapt.png){:width="100%"}


Next, let's see the individual demand curves for New apartments. 
These are the reverse of demand for Old: because Person 1 had the strongest preference for Old (vs New), they have the weakest preference for New (vs Old).
Again, budget size is increasing from Person 1 to 3.
Note that everyone is willing to pay higher prices for New apartments (compared to Old), reflecting the higher utility from New apartments.
![](https://michaelwiebe.com/assets/vacancy_chain/indl_demand_newapt.png){:width="100%"}

Here's aggregate demand for New apartments. 
Now Person 3 has the strongest preference, so they contribute to aggregate demand in all segments, while Person 1 contributes only in the bottom segment.
![](https://michaelwiebe.com/assets/vacancy_chain/agg_demand_newapt.png){:width="100%"}

An equilibrium is prices $$(P_{Old}, P_{New})$$ that set supply equal to aggregate demand in both markets.
I'll show an equilibrium where Person 2 initially buys an Old apartment. 
To capture the vacancy chain effect, we'll increase supply of New apartments, lowering the price, which induces Person 2 to upgrade from Old to New and reduces demand for Old apartments.
<!-- it's really the lower price that both(induces I2 to upgrade, reduces D1) 
it's not I2 upgrading that reduces D1; rather, moving alond D1 surface from change in p2, and we happen to end up at a point where I2 is in the other corner solution.
-->

Here's an equilibrium for Old apartments. 
I've chosen the supply curve to be consistent with Person 1 & 2 buying Old, while Person 3 buys New (ie. we're on the second segment of the demand curve for Old apartments).
The price is $$P_{Old} = 3$$.
![](https://michaelwiebe.com/assets/vacancy_chain/eqm1_oldapt.png){:width="100%"}

Here's the corresponding equilibrium for New apartments. 
Supply intersects demand on the segment of the demand curve where only Person 3 buys a New apartment.
Here the price is $$P_{New} = 10$$.
![](https://michaelwiebe.com/assets/vacancy_chain/eqm1_newapt.png){:width="100%"}

(Exercise for the reader: verify that these prices are consistent with the individual demand functions.)

Stepping back, note that these demand functions are actually 2D slices of a 3D demand surface. 
Changing the price of the other good moves you along the surface, but in 2D this will look like shifting the demand curve.
This is important, because changing the cross-price does not shift demand in the sense of changing preferences.
Instead, we're evaluating the same demand function at different price.
![](https://michaelwiebe.com/assets/vacancy_chain/demand_surface3d.png){:width="100%"}

OK, let's start the vacancy chain. 
We do so by increasing the supply of new apartments. 
For example, upzoning reduces land costs, which makes developers willing to produce more at every price.
Shifting from S1 to S2, we move down the demand curve, with a new price $$P_{New}=7$$.
![](https://michaelwiebe.com/assets/vacancy_chain/eqm2_newapt.png){:width="100%"}

Note that now supply intersects demand in the segment where both Person 3 and 2 buy New. 
Person 2 has upgraded from Old to New, induced by the lower price of New apartments ($$P_{New}=7 < P_{Old}=10$$)! 
So we should see a corresponding drop in demand for Old apartments.

Here's the effect on Old apartments. 
The demand curve 'shifts' down from D1 to D2, and now supply intersects demand on the segment where only Person 1 buys Old. 
The price falls from $$P_{Old}=3$$ to $$P_{Old}=2.5$$.
Intuitively, when price(New) falls, Old apartments become relatively less attractive, so demand for Old apartments decreases.
![](https://michaelwiebe.com/assets/vacancy_chain/eqm2_oldapt.png){:width="100%"}

<!-- I2 upgrading is incidental to the result
- could have had a supply shift with i2 staying at x1, but still p2 falls and p1 falls
 -->

 As mentioned above, this is not really a shift in demand, but a movement along the 3D demand surface caused by changing price(New).
 Person 2's preferences haven't changed, they just make a different choice at a different price point.

 So we've shown the vacancy chain effect: increasing the supply of new apartments reduces their price (from 10 to 7), which induces people to upgrade from old to new apartments, thereby reducing demand for and the price of old apartments (from 3 to 2.5).

But if price(Old) fell, shouldn't that reduce demand for New apartments too? 
Yes. These are substitute goods, so a reduction in the other price makes a good less appealing.
In this case, demand for New apartments 'shifts' down, but not enough to change the equilibrium.
Hence, there are no more changes, and we've converged on a new equilibrium, with $$(P_{Old}, P_{New})$$ = (2.5, 7).
![](https://michaelwiebe.com/assets/vacancy_chain/eqm2b_newapt.png){:width="100%"}

If we added more consumers, we could show how the vacancy chain allows a poorer person to afford an Old apartment at the new lower price, when they were originally priced out.
And with more housing sub-markets (with multiple degrees of quality, instead of only Old vs New), we could trace out the vacancy chain itself, with someone upgrading in each submarket and reducing demand for their original housing type, thereby enabling someone in the submarket below to upgrade.
<!-- not doing unit demand, so not a standard vacancy chain  -->