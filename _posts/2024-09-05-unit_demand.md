---
layout: post
title: "A supply and demand model of housing, part 1: unit demand"
date:   2024-09-05 17:00:00
type: post
---

In the [last post](https://michaelwiebe.com/blog/2024/08/perfsub_cts) I used continuous quantities of housing: people could buy, say, 1.26 homes.
Here I show how a supply and demand model works with discrete quantities, and with a unit demand constraint, where people buy at most one unit of housing.
This involves a different than the perfect substitutes case, but we arrive at the same conclusion regarding vacancy chains, demand cascades, and yuppie fishtanks.

# A supply and demand equilibrium

Consumers start with a willingness to pay for Old and New apartments.
They buy the housing type with the largest difference between valuation and price: $$v-p$$.
To satisfy the unit demand constraint, their demand only counts in the market of the type they choose.
So one rich person could buy up all of the homes, but the unit demand constraint prevents this, by requiring them to buy their highest-valued type, and removing their demand for the other types.
Here, there are five consumers, and everyone value New apartments more than Old.

![](https://michaelwiebe.com/assets/housing/unit/vac_chain/wtp.png){:width="100%"}

I'll show an initial equilibrium where Person 1 buys a New apartment, and Persons 2 and 3 buy Old.
Because Person 1 buys New, their demand is removed from the Old market.
Hence, the aggregate demand curve for Old apartments looks like this:

![](https://michaelwiebe.com/assets/housing/unit/vac_chain/demand_oldapt.png){:width="100%"}

Since each person demands one unit, aggregate demand tops out at four.
Next, here's the supply and demand equilibrium for Old apartments.
The supply curve is vertical, representing a fixed stock of Old apartments.
Since the supply curve overlaps the demand curve for some segment, technically the equilibrium is not well-defined.
However, this is not a big deal, because we can pretend the supply curve is slightly sloping upward, so the intersection is unique (though this brings in continuous quantities).
Alternatively, if we had a large number of consumers, the demand curve would be a straight line that intersects supply uniquely; working with discrete quantities is just a pain.

![](https://michaelwiebe.com/assets/housing/unit/vac_chain/eqm1_oldapt.png){:width="100%"}

So Persons 2 and 3 each get an Old apartment, and the price is 2.
Note that Persons 4 and 5 are initially priced-out, and don't get a home.
We can interpret this as having to move away, or living in their car (or being homeless).

Next we have the aggregate demand curve for New apartments.
Here we show Persons 1, 4, and 5, because Persons 2 and 3 buy Old apartments, so their demand for New is removed (by the unit demand constraint).

![](https://michaelwiebe.com/assets/housing/unit/vac_chain/demand_newapt.png){:width="100%"}

In the initial equilibrium, only Person 1 gets a new apartment, and the price is 4.
The supply curve is vertical, representing zoning constraints: developers cannot build any more, even if prices went up.
Note that Persons 4 and 5 are priced out here as well.

![](https://michaelwiebe.com/assets/housing/unit/vac_chain/eqm1_newapt.png){:width="100%"}

# Vacancy chains

To show a vacancy chain, we increase the supply of New apartments.
This will allow Person 2 to upgrade, vacating their Old apartment, which in turn allows Person 4 to get a home (instead of being homeless).

The higher supply of New apartments reduces the price, as we move down the demand curve.
This lower price induces Person 2 to re-evaluate their housing choice.
Their willingness to pay is 3.7 for New apartments, and 2.2 for Old. 
The price for New apartments was 4, so they chose Old.
But now the price is 2, so their payoff for a New apartment is 3.7 - 2 = 1.7, and the payoff for an Old apartment is 2.2 - 2 = 0.2
Hence, the New apartment is a better deal, so they switch markets from Old to New.

![](https://michaelwiebe.com/assets/housing/unit/vac_chain/eqm2a_newapt.png){:width="100%"}

Person 2 upgrading means that we also change which market their demand contributes to.
So we subtract their demand from the Old demand curve, and add it to the New demand curve, shifting it to the right.
For New apartments, this pushes up the price.
Note that we can end up out of equilibrium if this increase in price is not consistent with Person 2's willingness to pay.
Here, I'll use the modelling flexibility we have from the supply and demand curve overlapping to choose a new equilibrium price of 3.

![](https://michaelwiebe.com/assets/housing/unit/vac_chain/eqm2b_newapt.png){:width="100%"}

Since Person 2 upgrades, we remove their demand for Old apartments, shifting it left.
Now Persons 3 and 4 are able to get an Old apartment, and the price falls to 1.6.
(That is, supply intersects demand on the section of the demand curve where Persons 3 and 4 buy.)
This is the vacancy chain in action: Person 4 was initially priced out and didn't have a home, but by adding a New apartment, we have freed up an Old apartment for Person 4 to live in!

![](https://michaelwiebe.com/assets/housing/unit/vac_chain/eqm2_oldapt.png){:width="100%"}

To make sure this is an equilibrium, let's check the payoffs.
Person 1 is happy to buy New, because they get 4.7 - 3 = 1.7 from New and 2.6 = 1.6 = 1 from Old.
Person 2's payoffs are 3.7 - 3 = 0.7 (New) and 2.2 - 1.6 = 0.6, so they buy New.
Person 3 gets 2.2 - 3 < 0 for New and 2 - 1.6 = 0.4 from Old, so they buy Old.
So each consumer's choice is consistent, hence, these prices define an equilibrium.

Note that with unit demand, we are able to have consumers being priced out (like Person 4 initially).
With continuous quantities, this was not possible, since they could always buy some infinitesimal quantity, and so can't be priced out completely.
But with discrete units, Person 4 initially doesn't get anything, then thanks to the vacancy chain, they get an Old apartment.

