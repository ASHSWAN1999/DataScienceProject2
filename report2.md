# Falling Life Expectancy in the US: Effects of the Opioid Crisis & Rising Suicide Rates 

Sabrina Pereira & Ashley Swanson


## **Introduction**

In recent years, a downward trend in the Average Life Expectancy in the United States has emerged. This is a distressing shift; the last time life expectancy decreased was during World War II. 

![](https://github.com/ASHSWAN1999/DataScienceProject2/blob/master/figures/ALE.png)

Meanwhile, the number of deaths by opioid poisoning have risen dramatically, and suicides have been steadily rising. In 1999, there were 6.0 opioid poisoning-related deaths for every 100,000 people - by 2017, this had risen to 21.6 deaths per 100,000. What is known as the "Opioid Epidemic" or "Opioid Crisis" was declared a Public Health Emergency by the Trump Administration's Health and Human Services in 2017.

![](https://github.com/ASHSWAN1999/DataScienceProject2/blob/master/figures/opioid_over_time.png)

Suicide rates have also been steadily increasing, with 10.4 deaths per 100,000 in 1999 and 14.5 deaths per 100,000 in 2017.

![](https://github.com/ASHSWAN1999/DataScienceProject2/blob/master/figures/suicide_over_time.png)

We have chosen these factors to examine as they capture premature deaths among a younger, otherwise healthy population. These deaths have a larger effect on the Average Life Expectancy than increased death rates in an elderly population, and it indicates alarming threats to individuals of all ages, including our youth. 

This prompted the question,

_**What are the effects of the Opioid Crisis and rising suicide rates on the Average Life Expectancy in the US?**_

 
 


## **Methodology**

To find out what effect a given cause of death has had on the Average Life Expectancy (ALE) in the US, we modeled what the ALE would look like if no one died from that given cause of death. 

In our case, we model populations for a Zero-Opioid Scenario (a theoretical scenario where any deaths related to opioid overdoses do not occur) and a Zero-Suicide Scenario (a theoretical scenario where any deaths by suicide do not occur).

### Data Sources

We use the Centers for Disease Control and Prevention's (CDC) National Center for Health Statistics (NCHS) data for Death rates and life expectancy at birth for the US (Available [here](https://data.cdc.gov/NCHS/NCHS-Death-rates-and-life-expectancy-at-birth/w9j2-ggv5), [terms of use](https://wonder.cdc.gov/mcd.html)). All mentions of life expectancies or ALE relate to the CDC's definition of life expectancy at birth. The Average Life Expectancy is a summary statistic taking into account the death rates of various age groups over various years, giving us a snapshot of the health of the US population at a given point in time.

We also use the CDC's Multiple Cause of Death Data for the years 1999-2017 (Available [here](https://wonder.cdc.gov/mcd.html), [terms of use](https://wonder.cdc.gov/mcd-icd10.html)) to extract the population size and number of deaths in each age group in the US, as well as the total numbers of deaths due to opioid overdoses/suicides in each age group. To define deaths due to opioid overdoses, we use the CDC's definition of the ICD-10 codes that related to "All opioid poisoning" (the chart containing the codes fitting this definition is available [here](https://www.cdc.gov/drugoverdose/pdf/pdo_guide_to_icd-9-cm_and_icd-10_codes-a.pdf)). To define suicides, we used cases from the CD-10 113 Cause List categorized “GR113-124 Intentional self-harm (suicide).”

Both datasets take population counts from the 2000 and 2010 Census data. The CDC created intercensal population estimates were for all other years.

For more information on the data in the report and how we organized it, please refer to the [project’s notebook](https://github.com/ASHSWAN1999/DataScienceProject2/blob/master/project2.ipynb).


### Approach

To model what the ALE would look like in Zero-Opioid and Zero-Suicide scenarios, we first created a function that models the true life expectancy of the US given populations and deaths for each year and age. This allows us to subtract out deaths from a given cause, such as opioid poisoning, and recalculate ALE. 

Our life expectancy function draws from the CDC's *A Method for Constructing Complete Annual U.S. Life Tables* for the formulas they use in calculating the probability of death (q<sub>x</sub>) and the survival function (l<sub>x</sub>) to compute the life expectancy at every age. The life expectancy at birth is the ALE. For simplifications in the model, we grouped any people surviving beyond 100 years of age with those that survived until 100.

Because the CDC’s dataset does not contain individual age population estimates for respondents 85 and older,  we needed to estimate these values. To do this, we made the assumption that the population of a given *n<sub>th</sub>* age group for a given year is approximately equal to the population of the  *n-1<sub>th</sub>* age group minus the deaths occurring in the *n<sub>th</sub>* age group (for example, the population of 85-year-olds could be estimated as the population of 84-year-olds minus how many people died at the age of 85 for a given year). To accommodate for more nuanced differences in population over each age group and to ensure that there is a negligible number of people surviving past the age of 100, we also multiplied the number of deaths we were subtracting by a scaling factor. 

For more information on the calculations, please refer to the [project’s notebook](https://github.com/ASHSWAN1999/DataScienceProject2/blob/master/project2.ipynb).

![](https://github.com/ASHSWAN1999/DataScienceProject2/blob/master/figures/true_vs_cal.png)

This figure plots ALEs reported by the NCHS against those calculated by our life expectancy function. This graph is visual validation for our calculations, as it traces the true ALE very closely, particularly in the region where the trend shifts from positive to negative. 

Around the year 2003, we see that our model for the ALE differs from the true ALE more than any other year, so our results for modeling what the ALE would look like under our different scenarios will not be as accurate around this year as they would be for the other given years. However, we are more concerned with the downward trend in the ALE in the most recent years, and our model is a very good fit for the most recent years. Beyond 2007, all estimates are within 0.05 of the reported values.

The close fit of our model indicates that we can make a good estimate of what the life expectancy would look like when we remove the effects of a given cause of death.


## **Results**

So, to revisit our original question: _**What are the effects of the Opioid Crisis and rising suicide rates on the Average Life Expectancy in the US?**_

### Opioid-Related Deaths

Graphed below are the ALEs calculated from the actual population data and the Zero-Opioid Scenario ALEs. 

![](https://github.com/ASHSWAN1999/DataScienceProject2/blob/master/figures/zero_opioid.png)

Before the year 2007, the Zero-Opioid ALEs are on average about 0.22 years higher than the calculated ALEs. However, in the most recent years, the gap between the two plots is larger than it has ever been. From the model, the ALE in 2017 would have been about 0.60 years higher if there had been no opioid-related deaths (79.22 years, compared to the calculated 78.62 years). This graph provides strong evidence that if we did not have opioid-related deaths, ALE in the United States would not be decreasing. 

### Deaths by Suicide 

Here, we plot the calculated ALEs from our model and the Zero-Suicide Scenario ALEs.

![](https://github.com/ASHSWAN1999/DataScienceProject2/blob/master/figures/zero_suicide.png)

Before the year 2007, the Zero-Suicide ALEs are on average about 0.27 higher than the calculated ALEs. When compared to the last graph, this tells us suicides are historically a larger factor in lowering the life expectancy than opioid-related deaths. However, around the year 2011, suicides are no longer the larger factor of the two. If there had been no deaths by suicide, the ALE in 2017 would have been about 0.38 years higher (79.00 years, compared to the observed 78.6 years). This graph provides little evidence that rising suicide rates have contributed to the recent downturn in ALE.

To reiterate, the Average Life Expectancy is a robust statistic that takes the vitality data from all people in the United States to produce a single number. The fact that deaths by opioid overdose are causing the statistic to drop in a country as developed as the United States indicates that preventable premature deaths are an alarming threat to public health.

