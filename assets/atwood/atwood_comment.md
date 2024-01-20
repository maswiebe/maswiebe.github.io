---
layout: page
title: "The Long-Term Effects of Measles Vaccination on Earnings and Employment: Comment"
---

# Introduction

[Atwood (2022)](https://www.aeaweb.org/articles?id=10.1257/pol.20190509) ([original code](https://www.openicpsr.org/openicpsr/project/138401/version/V1/view), [my code](https://github.com/maswiebe/atwood_comment)) studies the long-term economic effects of the measles vaccine.
Using US data and a difference-in-differences strategy, Atwood finds that the national rollout of the vaccine in 1963 increased earnings and employment for adults by preventing measles during childhood.
The identification strategy interacts cross-sectional variation in pre-vaccine average reported measles incidence with time-series variation in the availability of the vaccine during childhood.

However, it is not clear how to interpret the geographic variation in pre-vaccine measles incidence, which is calculated as a state-level average over 1952-1963.
Atwood notes that measles is a universal disease, with 95% of the population contracting the virus by age 16.[^1]
While states could differ in short-run measles incidence, due to staggered timing of the epidemic cycle, this variation would be averaged out over 12 years.[^2]
Hence, differences in reported incidence reflect differences in reporting rates, since actual long-run incidence is the same across states.
Variation in reporting rates could be explained by differences in reporting capacity or differences in the impact of the disease.
States could vary in their capacity to report cases of contagious diseases, if better health infrastructure enables the detection of a larger fraction of cases.
Similarly, measles cases would be more severe in states with worse initial health, and more severe cases are more likely to be reported.

Atwood does not discuss reporting capacity or disease severity as sources of geographic variation in reported measles incidence,[^3]
and instead attributes differences in measles incidence to under-18 population density.
But the $$R^{2}$$ from a cross-sectional regression of average reported measles incidence on under-18 population density is only 0.05 (Table A1, Atwood (2022)).
Atwood claims that "states with higher prevaccine measles incidence benefited more from the introduction of the vaccine than states with lower levels of prevaccine measles incidence" (p.49).
This claim is plausible if states with worse initial health have high reported incidence due to having more severe cases, and the vaccine has larger benefits for individuals with poor health.[^4]
In contrast, if differences in reported incidence reflect reporting capacity, while actual disease incidence is the same, then it is not clear why states with higher reported incidence would benefit more from the vaccine.[^5]

Since the identifying variation is ambiguous, we should be worried that differences between high- and low-measles states are capturing trends unrelated to the measles vaccine.
In this comment, I replicate the main results using an expanded sample, and test for differential trends using an event study.
The regression results are similar, but
the event study shows post-vaccine trends that are inconsistent with a treatment effect of the vaccine.

# Regression results

Atwood defines the treatment window (exposure to the vaccine) to be 16 years long, since measles incidence after age 16 is negligible.
The treatment dose, vaccine exposure, is 0 for cohorts born in 1948 and earlier, increases by 1 per year until 1964, and is 16 for cohorts born afterwards.[^6]
To test for trends before and after the vaccine, we want symmetric windows for 1932-1948 and 1964-1980.
But Atwood uses 2000-2017 American Community Survey data on individuals aged 25-60, meaning the earliest cohort is 1940.
To include cohorts from 1932-1939, I extend the sample using [census data](https://usa.ipums.org/usa/) from 1960-1990.

Atwood's Table 2 estimates the long-run effects of childhood exposure to the measles vaccine on adult economic outcomes, using a difference-in-differences strategy with a continuous treatment variable.
I replicate these results using the extended sample.
The regression specification is

$$
y_{isct} = \beta \text{Measles}_{s} \times \text{Exposure}_{c} + \alpha_{t} + \gamma_{c} + \theta X_{isct} + \varepsilon_{isct},
$$

where $$y_{isct}$$ is the outcome (income, poverty, employment, or hours worked) for individual $$i$$ with state-of-birth $$s$$ in birthyear $$c$$, observed in survey year $$t$$.
*Measles*$$_{s}$$ is the state-level average measles rate over 1952-1962[^7],
and *Exposure*$$_{c}$$ is the number of years that the vaccine is available to a cohort.
Controls include fixed effects for survey year, birthyear, the interaction of age, Black, and female, and the interaction of state-of-birth, Black, and female.[^8]
See the code snippet below.

{::options parse_block_html="true" /}

<details><summary markdown="span">Code</summary>

```
* robustness: include region X birthyear FEs
use year sex age birthyr black bpl female cpi_incwage cpi_incwage_no0 ln_cpi_income poverty100 employed hrs_worked M12_exp_rate bpl_region9 exposure edu1960 employed1960 logfamincome1960 if missing(M12_exp_rate)==0 using "$data/acs_cleaned.dta", clear

local robust_fes year age#black#female bpl#black#female bpl_region9#birthyr

local i = 1

foreach x in cpi_incwage cpi_incwage_no0 ln_cpi_income poverty100 employed hrs_worked {

    reghdfe `x' M12_exp_rate, ab(`robust_fes') vce(cluster bpl#birthyr)
    est sto m`i'

    local i = `i' + 1
}

esttab m*, se label compress replace star(* 0.10 ** 0.05 *** 0.01) mtitles("Income" "Income ($>$0)" "Log income" "Poverty" "Employed" "Hours worked") nocons r2
```

</details>

{::options parse_block_html="false" /}

My results are in Table 1.
Panel A reproduces Table 2 using the extended sample.
The results are similar to the original, except that the effects on poverty and hours worked are now positive instead of negative.
In Panel B, I add fixed effects for census division by birthyear, to capture different trends across regions, following Table 4, Panel B in Atwood (2022).
Now the effect on poverty has the expected negative sign, indicating that division-birthyear fixed effects are needed to control for trends when using the longer panel.
The effect on hours worked is positive, matching Atwood's Table 4.
In Panel C, I additionally control for 1960 state-level averages of log family income, education, and employment interacted with vaccine exposure, following [Bleakley (2007)](https://academic.oup.com/qje/article-abstract/122/1/73/1924773).[^9]
The results are similar, showing that reported measles incidence captures variation separate from pre-vaccine state characteristics, conditional on the fixed effects.

![](https://michaelwiebe.com/assets/atwood/table1.png){:width="100%"}

# Event study

To test for differential trends in the original results, I run an event study interacting pre-vaccine average reported measles incidence with birthyear indicators.[^10]
The regression specification is

$$
y_{isct} = \sum_{j=1932, j \neq 1948}^{1980} \beta_{j} \text{Measles}_{s} \times \{1\}\{c=j\} + \alpha_{t} + \gamma_{c} + \theta X_{isct} + \varepsilon_{isct}.
$$

I restrict the sample to 1932-1980, to include symmetric pre- and post-periods matching the 16-year treatment window.
See the code snippet below.

{::options parse_block_html="true" /}

<details>
<summary markdown="span">Code</summary>

```
use year sex age birthyr black bpl female cpi_incwage cpi_incwage_no0 ln_cpi_income poverty100 employed hrs_worked avg_12yr_measles_rate bpl_region9 if inrange(birthyr,1932,1980) & missing(avg_12yr_measles_rate)==0 using "$data/acs_cleaned.dta", clear

gen measles_pc = avg_12yr_measles_rate/100000

forvalues i = 1932/1980 {
    gen d`i' = (birthyr==`i')
    gen int_`i' = d`i'*measles_pc
    lab var int_`i' " "
    drop d`i'
}

* loop to construct list by appending
    * put 1948 last, as omitted year
local interactions ""
forval y = 1932/1980 {
    if `y' != 1948 {
        local interactions "`interactions' int_`y'"
    }
}
local interactions "`interactions' int_1948"

local robust_fes year age#black#female bpl#black#female bpl_region9#birthyr

foreach x in cpi_incwage cpi_incwage_no0 ln_cpi_income poverty100 employed hrs_worked {
    reghdfe `x' `interactions', ab(`robust_fes') vce(cluster bpl#birthyr)
    coefplot, drop(_cons) vert omitted
}
```

</details>

{::options parse_block_html="false" /}


If the results in Table 1 are driven by the measles vaccine, we should observe the following:
no trend in the event study coefficients before 1948, as all cohorts have equal access to the vaccine (that is, none);
a trend in the coefficients from 1948-1964, as cohorts are increasingly exposed to the vaccine;
and no trend for 1964-1980, when all cohorts again have equal access to the vaccine (that is, full access).
In other words, the treatment effect should mirror the treatment dose.

The event study results are plotted in Figure 1.
There is no evidence of trends in the coefficients before 1948.
Moreover, the coefficients start to change in 1949, consistent with a treatment effect from the vaccine.
However, for five of six outcomes, the coefficients continue to change after the vaccine was introduced in 1963.
For example, in Panel (c), the coefficients for log income follow a rough positive trend from 1948 to 1980.
This is inconsistent with a treatment effect of the vaccine, since there is no change in vaccine exposure between successive cohorts after 1963.
That is, while the 1965 cohort benefits more from the vaccine than the 1955 cohort (because it is more likely that the former obtains immunity from the vaccine and avoids the health costs of measles), there is no such difference between the 1975 and 1965 cohorts.[^11]
Hence, the evidence in Atwood (2022) appears to reflect differential trends across states, rather than the effect of the measles vaccine.[^12]

![](https://michaelwiebe.com/assets/atwood/fig1.png){:width="100%"}

# Conclusion

I reanalyze Atwood (2022) by extending the dataset and using an event study to test for differential trends.
The identifying variation is from pre-vaccine average reported measles incidence, but for a contagious disease like measles, it is more plausible that this variation represents differences in reporting capacity or initial health levels than differences in actual disease incidence.
The event study results for long-run economic outcomes show trends that are inconsistent with an effect of the vaccine.

[^1]: [Chuard et al. (2022)](https://www.nber.org/papers/w30202) builds an epidemiological model based on the assumption that the ever-infected rate is roughly 100% in the pre-vaccine era.

[^2]: Differences in measles incidence could also be explained by differences in age structure, since younger children are more likely to be susceptible.
    But Atwood calculates the measles rate as the number of cases per 100,000 under-18 population, which controls for differences in age structure across states.

[^3]: Footnote 13 mentions that underreporting was common, and that state fixed effects control for time-invariant differences in reporting across states.
    But this does not address the treatment variation itself being driven by differences in reporting.

[^4]: Chuard et al. (2022) estimates the effect of the measles vaccine using pre-vaccine measles mortality as the cross-sectional treatment variable.
    They find long-run effects similar to those in Atwood (2022), and conclude that reported incidence is a proxy for disease severity.

[^5]: Using reported incidence seems more appropriate for diseases like malaria and hookworm, which have geographic variation in climatic suitability for the parasites that cause disease (Roodman ([2018a](https://www.econstor.eu/handle/10419/190496), [2018b](https://www.econstor.eu/handle/10419/192939)), Bleakley ([2007](https://academic.oup.com/qje/article-abstract/122/1/73/1924773), [2010](https://www.aeaweb.org/articles?id=10.1257/app.2.2.1))).
    There is no corresponding source of geographic variation for measles.

[^6]: Atwood defines vaccine exposure so that the 1963 cohort has 15 years of access.
    However, the vaccine is administered at one year of age,
    so babies born in 1963 would have vaccine access for all 16 years.

[^7]: Atwood calculates this average over 1952-1963.
    Since the vaccine was introduced in 1963, I omit this year to avoid post-treatment bias.
    I also correct a few transcription errors in the population data.

[^8]: Atwood clusters the standard errors at the birthyear by state-of-birth level.
    This is a restrictive assumption that does not allow the errors to be correlated between cohorts from the same state.
    As reported in Table A3 of Atwood (2022), the standard errors are at least three times larger when clustering by state-of-birth, and the income results are no longer statistically significant.
    Note that Bleakley (2007, 2010) clusters by state-of-birth.

[^9]: Table 4, Panel C in Atwood (2022) performs a weaker test, controlling for the state-level average of the dependent variable (adult economic outcomes) for pre-vaccine cohorts interacted with an indicator variable for pre-vaccine cohort.

[^10]: In Figures 3 and 4, Atwood runs an event study at the state-year level to test for the effect of the vaccine on contemporary disease incidence.
    Atwood frames this event study as testing for pretrends, but note that the absence of pretrends in reported disease incidence does not imply the absence of pretrends in long-run economic outcomes.

[^11]: [Barteska et al. (2023)](https://www.sciencedirect.com/science/article/pii/S0167629623001054) notes that vaccine takeup was low until the 1967-68 immunization campaign.
    On this view, vaccine exposure continues to increase until the 1968 cohort, so the trend in the event study coefficients should stop in 1968 (and should ramp up more slowly starting in 1948).
    However, the event study coefficients continue to change after 1968.

[^12]: Chuard et al. (2022) finds similar results to Atwood (2022) using measles mortality instead of incidence.
    Since they do not run an event study, it is possible that their results are also driven by trends.
