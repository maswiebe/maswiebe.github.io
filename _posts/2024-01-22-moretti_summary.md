---
layout: post
title:  "Summary: replication of Moretti (2021)"
date:   2024-01-22 17:00:00
type: post
---

[Moretti (2021)](https://michaelwiebe.com/assets/moretti/moretti_comment) is about agglomeration effects in innovation: do inventors patent more when they're around other inventors? 
In other words, does the size of tech clusters cause patenting?
This question is relevant for housing policy, because high housing costs prevent inventors from congregating in tech clusters.
So if agglomeration effects are large, then constraints on housing supply are hampering overall technological progress.

Moretti finds a positive correlation between the number of patents an inventor has and the size of the tech cluster they're in.
But since correlation is not causation, Moretti uses two other techniques to establish a causal link: an event study and an instrumental variables strategy.
In my replication, I find that both supporting results have coding errors, and correcting the errors overturns the results.
Hence, we're left wondering whether the main finding is causal or not.

One source of confounding is selection bias, where promising young inventors select into large tech clusters.
This would generate a correlation between patents and cluster size, but not from size causing patenting.
To test this, Moretti uses an event study, where the event is inventors moving to a different city.
By using cluster size before and after the move, we can see whether cluster size affects patenting.
And if there is selection, we should see an increase in patenting in the years before an inventor moves.

Moretti finds a big increase in patents in the year an inventor moves, and no sign of selection.
But there's a coding error.
The event study estimates one coefficient per year, by interacting the treatment variable with a year indicator.
But Moretti did not do this interaction for the year of the move.
So β0, the corresponding coefficient, is estimated using data from all years, instead of capturing the effect only in the year of the move.
When I include the proper interaction, the big effect goes away.
So the event study does not even provide evidence for agglomeration effects, let alone test for selection bias.

The other source of confounding is omitted variable bias driving both patenting and cluster size.
For example, a city subsidizing biotech firms would increase both biotech patents and the size of the local biotech cluster.
To address this, Moretti uses an instrumental variables strategy.
The idea is to use the number of inventors in other cities as a proxy (or 'instrument') for your own cluster size, to avoid bias from factors like local subsidies.

When constructing this proxy, Moretti calculates the change over time in other-city cluster size.
That is, we do this subtraction: "other-city cluster size this year" minus "other-city cluster size last year".
However, there's another coding error.
Moretti did not sort the data by city, so this subtraction is taken across different cities, which doesn't make sense.
The code is mixing up cities, doing "city A's other-city cluster size this year" minus "city B's other-city cluster size last year".
Correcting this error makes the results go away.
As with the event study, the instrumental variable strategy does not provide evidence against confounding.

So there is a positive correlation between patenting and cluster size, but it's unclear whether we should interpret it as a causal effect.
