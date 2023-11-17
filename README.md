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

|Column                   |Description|
|---                      |---        |
|`'YEAR'`               | 	Indicates the year in which the outage event occurred|
|`'MONTH'`               | 	Represents the month when the outage event took place|
|`'CAUSE.CATEGORY'` | Classifies the various events that lead to major power outages|
|`'OUTAGE.DURATION'` | Denotes the duration of outage events in minutes|
|`'TOTAL.CUSTOMERS'`   |Annual count of total customers served in the respective U.S. state|
|`'CLIMATE.CATEGORY'` |Signifies climate episodes for the corresponding years, categorized as "Warm," "Cold," or "Normal" based on a ± 0.5 °C threshold for the Oceanic Niño Index (ONI)|
|`'OUTAGE.START	'` | Indicates the time of day when the outage event commenced, as reported by the respective utility in the region|
|`'OUTAGE.RESTORATION.TIME	'` | Indicates the time of day when power was restored to all customers, as reported by the corresponding utility|
|`'DEMAND.LOSS.MW'`| Represents the amount of peak demand lost during an outage event, measured in Megawatts [in some cases, total demand is reported]|
|`'CUSTOMERS.AFFECTED'` | 	Quantifies the number of customers impacted by the power outage event|

In our study,  we used the `'CAUSE.CATEGORY'` as the main concern in seeing the associations of the each value in the `'CAUSE.CATEGORY'` COLUMNS. The 'CAUSE.CATEGORY' variable likely represents the categorization of the causes of severe weather-induced major power outages. To understand whether high severe outages were distributed at random within this category, you may want to investigate the methodology and statistical analysis conducted in the study.

We attempt to replicate the analysis conducted in the study. This involves using the same variables and applying the same statistical methods to assess whether high severe outages are distributed at random within each cause category.

---

## Cleaning Data

We have added new columns for the data cleaning part
|   YEAR | CLIMATE.CATEGORY   | OUTAGE.START        | OUTAGE.RESTORATION   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   TOTAL.CUSTOMERS | HIGH_SEVERITY   |
|-------:|:-------------------|:--------------------|:---------------------|:-------------------|:------------------------|------------------:|-----------------:|---------------------:|------------------:|:----------------|
|   2011 | normal             | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  | severe weather     | nan                     |              3060 |              nan |                70000 |           2595696 | True            |
|   2011 | normal             | 2011-07-08 10:00:00 | 2011-07-08 10:00:00  | intentional attack | vandalism               |                 0 |                0 |                    0 |           2595696 | False           |
|   2011 | normal             | 2011-05-11 15:55:00 | 2011-05-12 13:57:00  | intentional attack | vandalism               |              1322 |                0 |                    0 |           2595696 | False           |
|   2011 | cold               | 2011-04-19 22:44:00 | 2011-04-20 02:00:00  | severe weather     | nan                     |               196 |              100 |                64000 |           3174123 | False           |
|   2011 | normal             | 2011-06-22 09:46:00 | 2011-06-22 09:46:00  | severe weather     | nan                     |                 0 |              nan |               106300 |           3174123 | False           |


<iframe src="assets/fig_duration.html" width=800 height=600 frameBorder=0></iframe>

---