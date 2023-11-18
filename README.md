# Power Outage Investigation
by Max Yuen Sum Wong(y6wong@ucsd.edu), Chris Yuen Kei Wong(ykw001@ucsd.edu)

---
# Proejct Overview 

This is a data science project investigating the relationship of the values in a power outage dataset, then discussing a question with the assessment of missingness, and dealing with a hypothesis test. This project is for DSC80 at UCSD.



# Investigating Research Question and Introduction 

The content of the dataset is about the major power outages that happened in different states in the continental U.S. between January 2000 and July 2016. The dataset encompasses a diverse range of variables, including major outage data, geographical locations of the outages, date and time information, regional climatic details, land-use characteristics, electricity consumption patterns, and economic attributes of the affected states. This rich repository of information serves as a valuable resource for analyzing historical trends and patterns associated with major power outages. **To find out whether the high severe outages were distributed at random from the CAUSE.CATEGORY, we will investigate the correlation between category and other values recorded in the dataset.**

--- 

## Introduction to the Datasets in this Study
The dataset we are using contains the information on 1534 power outages cases from 2000 to 2016, with 59 columns recording the information. Based on the question we investigate, the columns are useful for us to use with the description as follow:

|Column                     |Description|
|---                        |---        |
|`'YEAR'`                   | Indicates the year in which the outage event occurred|
|`'CLIMATE.CATEGORY'`       | Signifies climate episodes for the corresponding years, categorized as "Warm," "Cold," or "Normal" based on a ± 0.5 °C threshold for the Oceanic Niño Index (ONI)|
|`'OUTAGE.START'`           | Indicates the time of day when the outage event commenced, as reported by the respective utility in the region|
|`'OUTAGE.RESTORATION'`     | Indicates the time of day when power was restored to all customers, as reported by the corresponding utility|
|`'OUTAGE.DURATION'`        | Denotes the duration of outage events in minutes|
|`'CAUSE.CATEGORY'`         | Classifies the various events that lead to major power outages|
|`'CAUSE.CATEGORY.DETAIL'`  | Classifies the detail of various events that lead to major power outages|
|`'DEMAND.LOSS.MW'`         | Represents the amount of peak demand lost during an outage event, measured in Megawatts [in some cases, total demand is reported]|
|`'CUSTOMERS.AFFECTED'`     | Quantifies the number of customers impacted by the power outage event|
|`'TOTAL.CUSTOMERS'`        | Annual count of total customers served in the respective U.S. state|
|`'HIGH_SEVERITY'`          | Binary indicator (True/False) denoting whether the outage event is classified as high severity, based on the outage duration being above the average for that specific year. Events with durations exceeding the annual average are marked as high severity. |

In our study,  we used the `'CAUSE.CATEGORY'` as the main concern in seeing the associations of the each value in the `'CAUSE.CATEGORY'` COLUMNS. The 'CAUSE.CATEGORY' variable likely represents the categorization of the causes of severe weather-induced major power outages. To understand whether high severe outages were distributed at random within this category, you may want to investigate the methodology and statistical analysis conducted in the study.

We attempt to replicate the analysis conducted in the study. This involves using the same variables and applying the same statistical methods to assess whether high severe outages are distributed at random within each cause category.

---

## Cleaning Data and Exploratory Data Analysis

In order to increase the readability and accuracy of our data, we have commit some changes to clean the original DataFrame we get. 

1. **Create a function to combine the `'OUTAGE.START.DATE'` and `'OUTAGE.START.TIME'` into a new column named `'OUTAGE.START'` where store the datetime value of the outage starting time. Doing the same as the `'OUTAGE.RESTORATION'` column in the `'cleaned'` dataset.**
2. **Defining the `'HIGH_SEVERITY'` column where it stores the boolean values whether the `'OUTAGE.DURATION'` is greater than the high_severity_threshold which we define as the mean of `'OUTAGE.DURATION'`.**

After data cleaning, the first 5 rows of the data in the new DataFrame called `'cleaned'` would be showing, moreover, we have added new columns for the data cleaning part. See as follow:

|   YEAR | CLIMATE.CATEGORY   | OUTAGE.START        | OUTAGE.RESTORATION   |   OUTAGE.DURATION | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   TOTAL.CUSTOMERS | HIGH_SEVERITY   |
|-------:|:-------------------|:--------------------|:---------------------|------------------:|:-------------------|:------------------------|-----------------:|---------------------:|------------------:|:----------------|
|   2011 | normal             | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |              3060 | severe weather     | nan                     |              nan |                70000 |           2595696 | True            |
|   2011 | normal             | 2011-07-08 10:00:00 | 2011-07-08 10:00:00  |                 0 | intentional attack | vandalism               |                0 |                    0 |           2595696 | False           |
|   2011 | normal             | 2011-05-11 15:55:00 | 2011-05-12 13:57:00  |              1322 | intentional attack | vandalism               |                0 |                    0 |           2595696 | False           |
|   2011 | cold               | 2011-04-19 22:44:00 | 2011-04-20 02:00:00  |               196 | severe weather     | nan                     |              100 |                64000 |           3174123 | False           |
|   2011 | normal             | 2011-06-22 09:46:00 | 2011-06-22 09:46:00  |                 0 | severe weather     | nan                     |              nan |               106300 |           3174123 | False           |

---

## Univariate Analysis
In this section, we explore individual variables to understand their distributions and characteristics. Two key variables are examined: **OUTAGE.DURATION** and **CAUSE.CATEGORY**.

1. ### `'OUTAGE.DURATION'` Distribution:

The histogram visualizes the distribution of outage durations, showcasing the frequency of outages across different duration ranges. It provides insights into the typical duration of power outages and highlights any notable outliers. 

<iframe src="assets/fig_duration.html" width=800 height=600 frameBorder=0></iframe>

- **Plot Explanation:** The histogram displays the distribution of outage durations in minutes. The x-axis represents the duration of outages, while the y-axis represents the frequency of outages within each duration range.

- **Interpretation:** The majority of outages have a relatively short duration, with a peak in the histogram. However, there are also a few instances of longer-duration outages, contributing to the right tail of the distribution.

2. ### `'CAUSE.CATEGORY'` Distribution:

The bar chart presents the distribution of major power outages categorized by their causes. It offers a comprehensive view of the prevalence of different causes, shedding light on the primary factors contributing to major power disruptions.

<iframe src="assets/fig_category.html" width=800 height=600 frameBorder=0></iframe>

- **Plot Explanation:** The bar chart illustrates the distribution of major power outages across different cause categories. Each bar represents a specific cause category, and the height of the bars corresponds to the frequency of outages in each category.

- **Interpretation:** The chart provides insights into the prevalence of different causes for major power outages. It appears that certain categories, such as severe weather, may be more common than others.

---

## Bivariate Analysis
In this section, we explore relationships between pairs of variables, shedding light on potential correlations and patterns within the dataset.

1. ### Outage Duration vs. Cause Category:

<iframe src="assets/fig_OD_vs_CC.html" width=800 height=600 frameBorder=0></iframe>

- **Insight:** The box plot reveals the distribution of outage durations across different cause categories. It assists in identifying variations in outage durations associated with specific causes, aiding in the understanding of the impact of different events on outage durations.

- **Interpretation:** From this box chart, we noticed that fuel supply emergency have a wide range of values which the outliers increment the mean of the outage duration. It seems that there are several high severity cases of the outage. With regard to other cause categories, their outage duration remain steady, and each categories contains a several outiers but not very significant.

2. ### Percentage of Climate Categories for Each Cause Category:

<iframe src="assets/fig_climate_percent.html" width=800 height=600 frameBorder=0></iframe>

- **Insight:** This bar chart displays the percentage distribution of climate categories within each cause category. It offers a nuanced perspective on the proportional contribution of climate conditions to various types of power outages, enhancing our understanding of their interdependencies.

- **Interpretation:** We observed that this grouped barcahrt, all categories are mostly having normal climate in majority of their outage diatribution. Speciafically, fuel supply emergency and equipment failure consist of more proportion of cold climate category which shows that these two categories of causes occurred more often when the climate is cold.

---

## Assessment of Missingness
### NMAR Analysis
One of the column in the dataset we observed with missing values that is possibly NMAR is the column. We observed the missing values of that columns are likely to be non-random and if there's a systematic reason behind their absence.

### Reasoning:

**Nature of Date:** The missing values in the "OUTAGE.RESTORATION" column are resonable if there is no restroation after the outage start. This assumption aligns with the logical flow of events during a power outage. If there is no restoration, the date and time in the columns would not be available, and recorded.

**Data Generating Process:** The nature of power outage events is such that restratoin may not occur immediately or may not occur at all incerain cases. Therefore, the absence of a restoration date could be inherent to the data generating process. For example, if a severe weather event or intentional attack cause extensice damage, it may not be possible to retore power immediately or abandon the construction permenantly due to the tedious amount of fee of maintainence. If the power is not stored, there is no date and time to record.

However, there are some exceptions to the column that if the OUTAGE.START is not recorded, it is reasonable to miss the value at random (MAR) in this case. If there is no outage recorded, the time of outage restoration will not be recored based on the process of taking the time record.

**Conclusion:** Considering the logical flow of events during power outages and the nature of the data generating process, it is reasonable to believe that the "OUTAGE.RESTORATION.DATE" column is likely NMAR. The missing values in this column are not randomly distributed but are tied to the occurrence (or lack) of power restoration following an outage. Further details on the circumstances of each outage event, especially those without restoration dates, would enhance the understanding of the missingness pattern.

--- 

## Interesting Aggregates



---

## Missingness Dependency

Since we interested in the relationship between `'CAUSE.CATEGORY'` and other columns, and we believe that their data can be expressed into a more detailed column like `'CAUSE.CATEGORY.DETAIL'`. As such, we would like to find how the missingness in `'CAUSE.CATEGORY.DETAIL'` are
depended on the other columns.

### `'TOTAL.CUSTOMERS'` and `'CAUSE.CATEGORY.DETAIL'` (MCAR)

**Null Hypothesis:** The missingness of `'CAUSE.CATEGORY.DETAIL'` does not depend on `'TOTAL.CUSTOMERS'`. 

**Alternative Hypothesis:** The missingness of `'CAUSE.CATEGORY.DETAIL'` depends on `'TOTAL.CUSTOMERS'`

Below are the distributions of missing values of `'CAUSE.CATEGROY.DETIAL'`, which we call it `'DETAIL.MISSING'`, and `'TOTAL.CUSTOMERS'` in a form of the plotly plot, we conducted the shape of the two distribution are unsimular. As such, running the Kolmogorov-Smirnov to collect the observed statistic is the purpose to run a permutation test to find out the p-value whether it is fall in the distribution. 
<iframe src="assets/fig_total_cutomer_dist.html" width=800 height=600 frameBorder=0></iframe>

Before that, we are using the cumulative distribution function (CDFs) to see the simularity between the two distributions in order to find out the K-S statistic that is roughly defined as the **largest difference between two CDFs**. Here you can see the difference of mean is quite large. 

<iframe src="assets/fig_total_cutomer_CDF.html" width=800 height=600 frameBorder=0></iframe>

After we run the permutation test, we can see the histogram below is showing the p-value is 1.0 since the obseved statistic is fall beyond the whole distribution. As a result, we can conclude that the missingness of `'CASUE.CATEGORY.DETAIL'` does not depend on the `'TOTAL.CUSTOMERS'` column. We fail to reject the null hypothesis. 

<iframe src="assets/fig_total_cutomer_KS.html" width=800 height=600 frameBorder=0></iframe>

### `'OUTAGE.DURATION'` and `'CAUSE.CATEGORY.DETAIL'` (MAR)

**Null Hypothesis:** The missingness of `'CAUSE.CATEGORY.DETAIL'` does not depend on `'OUTAGE.DURATION'` 

**Alternative hypothesis:** The missingness of `'CAUSE.CATEGORY.DETAIL'` depends on `'OUTAGE.DURATION'` 

Below are the distributions of `'DETAIL.MISSING'` and `'OUTAGE.DURATION'` in a form of the plotly plot, we conducted the shape of the two distribution are , but this time the mean difference is quite small according to the distribution. We can also run the Kolmogorov-Smirnov to collect the observed statistic is the purpose to run a permutation test to find out the p-value whether it is fall in the distribution. 
<iframe src="assets/fig_outage_duration_dist.html" width=800 height=600 frameBorder=0></iframe>

After collected the ks-statistic and run the permutation test, we see a p-value of 0.01 which satisfy the signigicance level of 5%, so we can reject the nul hypothesis. The missingness of `'CAUSE.CATEGORY.DETAIL'` is depend on the values of `'OUTAGE.DURATION'` 

<iframe src="assets/fig_outage_duration_KS.html" width=800 height=600 frameBorder=0></iframe>

---

## Hypothesis Test

**Whether the high severe outages were distributed at random from the CAUSE.CATEGORY?**

### Null Hypothesis:
The distribution of high-severity outages across different cause categories is random, and there is no significant association between the cause category and the occurrence of high-severity outages.

### Alternative Hypothesis:
The distribution of high-severity outages across different cause categories is not random, and there is a significant association between the cause category and the occurrence of high-severity outages.


### Significance Level:
Our significance level is set at 0.05, representing a 5% chance of rejecting the null hypothesis when it is true.

* **Reason:**
The significance level, often denoted by 
α, is the threshold for determining statistical significance. A common choice is 0.05. We've set a conventional significance level, and if the p-value is less than 0.05, we would reject the null hypothesis. This decision threshold helps control the risk of Type I errors (incorrectly rejecting a true null hypothesis).

### Test Statistics:
To test these hypotheses, we employ a statistical method known as Total Variation Distance (TVD). which quantifies the difference between two probability distributions. In this case, it measures how much the distribution of high-severity outages deviates from a random distribution across cause categories.

* **Reason:**
The TVD is a suitable choice because it measures the discrepancy between two probability distributions. In this context, it assesses how different the distribution of high-severity outages is from a random distribution within the cause category. By calculating the TVD for the observed data and comparing it to an empirical distribution generated under the null hypothesis, we gain insights into the extent of this difference.


### Procedure:
1. Calculated the observed TVD from the actual data.
2. Conducted a bootstrap simulation to generate multiple samples assuming a random distribution.
3. Computed TVD for each bootstrap sample to create an empirical distribution.
4. Compared the observed TVD to the empirical distribution to calculate the p-value.

### Observation:
| CAUSE.CATEGORY                |   Low Severity |   High Severity |
|:------------------------------|---------------:|----------------:|
| equipment failure             |      0.0503145 |      0.00242718 |
| fuel supply emergency         |      0.0242588 |      0.0558252  |
| intentional attack            |      0.354897  |      0.0558252  |
| islanding                     |      0.0413297 |      0          |
| public appeal                 |      0.0548068 |      0.0194175  |
| severe weather                |      0.370171  |      0.842233   |
| system operability disruption |      0.104223  |      0.0242718  |

In our observed dataframe, we aggregate our data to the percentage of low-severity and high-severity outages of the categories within each column separately. With regard to our hypothesis, we are mainly focusing whether the severity of outage is random distributed across the categories of causes. 

<iframe src="assets/fig_hyp_dist.html" width=800 height=600 frameBorder=0></iframe>

By observing the grouped bar chart above, there is a significance on the distribution of High Severity Outage which is mainly occur when there is severe weather as the cause.

### Result:

<iframe src="assets/fig_hyp_tvd.html" width=800 height=600 frameBorder=0></iframe>

A p-value of zero indicates that the observed total variation distance (TVD) is extremely unlikely to occur under the assumption of the null hypothesis. In other words, the observed difference between the distributions is so significant that it falls outside the range of values obtained through random chance alone.

When the p-value is effectively zero and the observed statistic is far from the empirical distribution, it suggests strong evidence against the null hypothesis. In this case, it implies that the distribution of high-severity outages is not random with respect to the cause category; there is a significant association between the cause category and the occurrence of high-severity outages.

**As a result, we rejected the null hypothesis in which the data provides evidence in favor of our alternative hypothesis.**
---