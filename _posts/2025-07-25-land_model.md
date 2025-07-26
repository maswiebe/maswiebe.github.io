---
layout: post
title: "A two-sector model of land and upzoning"
date:   2025-07-25 17:00:00
type: post
---

When we upzone land from single-family zoning to apartment zoning, we change the allocation of a city's fixed stock of land.
Upzoning makes apartment-zoned land more abundant and hence cheaper, while single-family-zoned land becomes scarcer and more expensive.
Because land is an input into the production of housing, upzoning reduces the cost of building apartments, and hence is a key policy option for shifting the supply curve.

In this post I'll set up a two-sector model of land and show the effects of upzoning.
House developers can use either single-family-zoned land or apartment-zoned land to produce houses, while apartment developers can only use apartment-zoned land.
Since apartments are more lucrative than houses, apartment developers are willing to pay more for land, resulting in a higher price of apartment-zoned land.
Since upzoning moves a parcel to the higher-price submarket, it effectively makes the parcel unavailable to house developers.
In the extreme case, if we upzone so much that demand from apartment developers is fully satiated, then house developers are indifferent between the two types of land, and we get an interior solution where prices are equal across submarkets.

# A two-sector model of land

Consider a representative house developer with perfect substitutes preferences over house-zoned and apartment-zoned land.
For the house developer, a parcel is a parcel, since regardless of the zoning they can build a house on it.
Hence, they treat both types of land as interchangeable, and will choose the lower-price option.
(The horizontal segment is is where they are indifferent.)

![](https://michaelwiebe.com/assets/housing/land_model/agg_dem_house.png){:width="100%"}

For the apartment-zoned land market, we have to aggregate the demand of both house and apartment developers.
Apartment developers spend their whole budget on apartment-zoned land, regardless of how cheap house-zoned land gets.
Because they have higher willingness to pay, their quantity demanded is larger at higher prices.
As before, house developers will spend everything on apartment-zoned land if it is cheap enough, but otherwise they go all-in on house-zoned land.

![](https://michaelwiebe.com/assets/housing/land_model/sec_dem_apt.png){:width="100%"}

Adding up the sector demand curves, we get aggregate demand for apartment-zoned land.
In the upper curved segment we will get a corner solution where apartment developers buy only apartment-zoned land, and house developers buy only house-zoned land.
The bottom curved segment is the interior solution where both developers buy apartment-zoned land.

![](https://michaelwiebe.com/assets/housing/land_model/agg_dem_apt.png){:width="100%"}

Here's an equilibrium for the corner solution.
To maintain incentive compatibility, we require that $$P_{A} > P_{H}$$, so that the house developer chooses the cheaper option (house-zoned land).
This holds when the (fixed) supply of house-zoned land is large and the supply of apartment-zoned land is small.

![](https://michaelwiebe.com/assets/housing/land_model/eqm1_house.png){:width="100%"}

![](https://michaelwiebe.com/assets/housing/land_model/eqm1_apt.png){:width="100%"}

Market clearing holds (all supply is demanded at these prices), so this is an equilibrium.
Note that we also have a market clearing condition for supply across sectors: $$S_{A} + S_{H} = S = 13$$.

# Small upzoning

Let's upzone a small amount by moving one parcel from the house market to the apartment market (so $$S_{A}$$ increases from 1 to 2 and $$S_{H}$$ falls from 13 to 12).
This shifts $$S_{A}$$ to the right, which reduces $$P_{A}$$ from 12 to 6.
At the same time, $$S_{H}$$ shifts left, increasing $$P_{H}$$ from 2 to 2.2.
Because the house developer treats the two land types as substitutes, these price changes also 'shift' the demand curves.
(Recall from [last time](https://michaelwiebe.com/blog/2024/08/perfsub_cts) that this is actually a movement along the 3D demand surface.)
House-zoned land becomes more expensive, so demand for its substitute (apartment-zoned land) increases; this is the shift up/right in demand for apartment-zoned land.

![](https://michaelwiebe.com/assets/housing/land_model/eqm2_apt.png){:width="100%"}

Similarly, apartment-zoned land becomes cheaper, so demand for its substitute (house-zoned land) falls, shown by the shift down/left in demand for house-zoned land.

![](https://michaelwiebe.com/assets/housing/land_model/eqm2_house.png){:width="100%"}

So upzoning works as expected: by making apartment-zoned land more abundant, competitive pressure is lessened and the price falls, making apartments cheaper to produce.
This is consistent with the upzoned parcel itself increasing in price, from 2 to 6.
There's no puzzle here, because that parcel was previously unavailable to apartment developers _at any price_, and now is available at $$P_{A}=6$$.
Conversely, houses become a bit more expensive, since house developers have to compete over a more limited stock of land.
This is progressive, because houses are inherently more of a luxury good than apartments.

# Large upzoning

Next let's explore the interior solution where $$P_{A} = P_{H}$$.
Here we shift supply enough that we reach the horizontal segment of the perfect substitutes demand function.
In this case, $$P_{A} = P_{H} = P = \approx 2.8$$; this is the maximum possible reduction in the price of apartment-zoned land.
We can choose $$S_{A} \geq 4.33$$ and $$S_{H} \leq 8.66$$, subject to $$S_{A} + S_{H} = 13$$; here I've used $$S_{A} = S_{H} = 6.5$$.

![](https://michaelwiebe.com/assets/housing/land_model/eqm3_apt.png){:width="100%"}

Since house developers are indifferent between the types of land, we can be anywhere on the horizontal segment as long as market clearing holds.
Because the demand curve is horizontal, these movements don't affect prices, and any allocation of supply is an equilibrium.

![](https://michaelwiebe.com/assets/housing/land_model/eqm3_house.png){:width="100%"}

# Maximum upzoning

Finally, let's show the case where the entire stock of land is upzoned for apartments.
Tihs is really the same case as the large upzoning, because we again have $$P_{A} = P_{H} = P = \approx 2.8$$, and the only difference is that we move all of the way to the edge of the interior solution.

![](https://michaelwiebe.com/assets/housing/land_model/eqm4_apt.png){:width="100%"}
![](https://michaelwiebe.com/assets/housing/land_model/eqm4_house.png){:width="100%"}

Note that we can't have an equilibrium in the other corner solution with $$P_{A} < P_{H}$$, because of market clearing: we can't have $$S_{H} <0 $$ or $$ S_{A} > S $$, so we can't shift $$S_{A}$$ farther than the horizontal segment of the demand curve.

A full general equilibrium model would include the output markets for houses and apartments, and show explicitly how output prices for housing are linked to input prices for land.
One interesting case to look at would be where mass upzoning reduces $$P_{H}$$ and hence further reduces house developers' demand for house-zoned land.
For example, a huge increase in the supply of apartments downtown induces people to move out the exurbs.
