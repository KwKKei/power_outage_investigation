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

## Assessment of Missingness
### NMAR Analysis
One of the column in the dataset we observed with missing values that is possibly NMAR is the column. We observed the missing values of that columns are likely to be non-random and if there's a systematic reason behind their absence.

### Reasoning:

**Nature of Date:** The missing values in the "OUTAGE.RESTORATION" column are resonable if there is no restroation after the outage start. This assumption aligns with the logical flow of events during a power outage. If there is no restoration, the date and time in the columns would not be available, and recorded.

**Data Generating Process:** The nature of power outage events is such that restratoin may not occur immediately or may not occur at all incerain cases. Therefore, the absence of a restoration date could be inherent to the data generating process. For example, if a severe weather event or intentional attack cause extensice damage, it may not be possible to retore power immediately or abandon the construction permenantly due to the tedious amount of fee of maintainence. If the power is not stored, there is no date and time to record.

However, there are some exceptions to the column that if the OUTAGE.START is not recorded, it is reasonable to miss the value at random (MAR) in this case. If there is no outage recorded, the time of outage restoration will not be recored based on the process of taking the time record.

**Conclusion:** Considering the logical flow of events during power outages and the nature of the data generating process, it is reasonable to believe that the "OUTAGE.RESTORATION.DATE" column is likely NMAR. The missing values in this column are not randomly distributed but are tied to the occurrence (or lack) of power restoration following an outage. Further details on the circumstances of each outage event, especially those without restoration dates, would enhance the understanding of the missingness pattern.

--- 

## Missingness Dependency

---

## Hypothesis Testing