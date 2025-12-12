# Power Outage Impact Analysis
Project for DSC80 at UCSD

by Lina Seto

## Introduction
A power outage is defined as an interruption in the supply of electricity to end users. Millions of customers experience power outages each year with the US Census Bureau reporting 1 in 4 households experiencing at least one. Power outages can disrupt daily life in the loss of lighting and hot water, but there are also more severe consequences such as failure of medical equipment, release of toxic carbon monoxide from gas stoves, or lack of traffic lights. In any case, the number of customers affected by a power outage instance is an extremely important indicator in the severity and scope of a power outage. Therefore, I will conduct this project around the central question: **What factors control or impact the number of customers affected by power outages?**

Throughout this project, I will use the power outages dataset. This dataset contains major power outage data in the continental United States from January 2000 to July 2016. The dataset also presents data on geographic location, date, time, regional climatic information, land-use characteristics, electricity consumption patterns, and economic characteristics of states affected by outages. The dataset was accessed through the Laboratory for Advancing Sustainable Critical Infrastructure at Purdue University.

The dataset contains 1534 rows where each row corresponds to a power outage instance. While the raw data contains 57 columns, I will choose the following 20 columns to use in my analysis.

| Column                      | Description                                                                                                                                                                                                                   |
|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `'YEAR'`                    | Indicates the year when the outage event occurred                                                                                                                                                                             |
| `'MONTH'`                   | Indicates the month when the outage event occurred                                                                                                                                                                            |
| `'U.S._STATE'`              | Represents all the states in the continental U.S.                                                                                                                                                                             |
| `'NERC.REGION'`             | The North American Electric Reliability Corporation (NERC) regions involved in the outage event                                                                                                                               |
| `'CLIMATE.REGION'`          | U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)                                                                              |
| `'ANOMALY.LEVEL'`           | This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W) |
| `'CLIMATE.CATEGORY'`        | This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)                      |
| `'OUTAGE.START.DATE'`       | This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region)                                                                                            |
| `'OUTAGE.START.TIME'`       | This variable indicates the time of the day when the outage event started (as reported by the corresponding Utility in the region)                                                                                            |
| `'OUTAGE.RESTORATION.DATE'` | This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region)                                                                             |
| `'OUTAGE.RESTORATION.TIME'` | This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region)                                                                             |
| `'CAUSE.CATEGORY'`          | Categories of all the events causing the major power outages                                                                                                                                                                  |
| `'OUTAGE.DURATION'`         | Duration of outage events (in minutes)                                                                                                                                                                                        |
| `'DEMAND.LOSS.MW'`          | Amount of peak demand lost during an outage event (in Megawatt) [but in many cases, total demand is reported]                                                                                                                 |
| `'CUSTOMERS.AFFECTED'`      | Number of customers affected by the power outage event                                                                                                                                                                        |
| `'TOTAL.PRICE'`             | Average monthly electricity price in the U.S. state (cents/kilowatt-hour)                                                                                                                                                     |
| `'TOTAL.SALES'`             | Total electricity consumption in the U.S. state (megawatt-hour)                                                                                                                                                               |
| `'TOTAL.CUSTOMERS'`         | Annual number of total customers served in the U.S. state                                                                                                                                                                     |
| `'POPPCT_URBAN'`            | Percentage of the total population of the U.S. state represented by the urban population (in %)                                                                                                                               |
| `'AREAPCT_URBAN'`           | Percentage of the land area of the U.S. state represented by the land area of the urban areas (in %)                                                                                                                          |

With this data, I will analyze the effect of urbanization level on the number of customers affected by a power outage and use various features to predict the severity of a power outage. Such a prediction model may be able to prepare customers and local government officials for a potential major power outage in order to enact safety precautions to minimize the impact and damage.


## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
In order to ensure effective analysis, I first cleaned the data.

0. Before starting to clean the data, I removed the initial first row from the dataframe as it consisted of units and did not represent a power outage instance.

1. First, I keep only the columns with features relevant to my analysis: `'YEAR'`, `'MONTH'`, `'U.S._STATE'`, `'NERC.REGION'`, `'CLIMATE.REGION'`, `'ANOMALY.LEVEL'`, `'CLIMATE.CATEGORY'`, `'OUTAGE.START.DATE'`, `'OUTAGE.START.TIME'`, `'OUTAGE.RESTORATION.DATE'`, `'OUTAGE.RESTORATION.TIME'`, `'CAUSE.CATEGORY'`, `'OUTAGE.DURATION'`, `'DEMAND.LOSS.MW'`, `'CUSTOMERS.AFFECTED'`, `'TOTAL.PRICE'`, `'TOTAL.SALES'`, `'TOTAL.CUSTOMERS'`, `'POPPCT_URBAN'`, `'AREAPCT_URBAN'`. I drop the rest of the features.

2. Next, I combine `'OUTAGE.START.DATE'` and `'OUTAGE.START.TIME'` into one column `'OUTAGE.START.DATETIME'` that represents the timestamp of the start of the power outage. I do the same with the restoration time, combining`'OUTAGE.RESTORATION.DATE'`and `'OUTAGE.RESTORATION.TIME'` into one column `'OUTAGE.RESTORATION.DATETIME'` that represents the timestamp at which power was restoreed. I then dropped the old columns `'OUTAGE.START.DATE'`, `'OUTAGE.START.TIME'`, `'OUTAGE.RESTORATION.DATE'`, and `'OUTAGE.RESTORATION.TIME'`.

3. I convert `'YEAR'` from a float to an integer datatype and `'MONTH'` from a number to the name of the month. I also convert all numerical columns not stored as numerical datatypes, `'ANOMALY.LEVEL'`, `'OUTAGE.DURATION'`, `'DEMAND.LOSS.MW'`, `'CUSTOMERS.AFFECTED'`, `'TOTAL.PRICE'`, `'TOTAL.SALES'`, `'TOTAL.CUSTOMERS'`, `'POPPCT_URBAN'`, `'AREAPCT_URBAN'`, into float datatypes.

4. I added the values of `'CLIMATE.REGION'` for Hawaii and Alaska as their state names. The values for these two states were previously marked as null because they are not given climate regions by the National Centers for Environmental Information.

5. Finally, I engineered two variables to use for future analysis. The first is `'is_HIGH_URBAN'` which holds values of True for areas with high urbanization where over 80% of the state population lives in urban areas (`'POPPCT_URBAN'` > 80%) and False for areas of low urbanization where under or approximately 80% of the state population lives in urban areas (`'POPPCT_URBAN'` <= 80%). The second is `'is_MAJOR_OUTAGE'` which hold values of True for major outages which is defined as one lasting over 60 minutes and affecting over 50,000 customers (`'OUTAGE.DURATION'` > 60 and `'CUSTOMERS.AFFECTED'` > 50,000) and False otherwise (`'OUTAGE.DURATION'` <= 60 and `'CUSTOMERS.AFFECTED'` <= 50,000).

Below is the first few rows of the cleaned dataframe. The cleaned dataframe has 1534 rows and 20 columns.

|   YEAR | MONTH   | U.S._STATE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   TOTAL.PRICE |   TOTAL.SALES |   TOTAL.CUSTOMERS |   POPPCT_URBAN |   AREAPCT_URBAN | OUTAGE.START.DATETIME   | OUTAGE.RESTORATION.DATETIME   | is_HIGH_URBAN   | is_MAJOR_OUTAGE   |
|-------:|:--------|:-------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|------------------:|-----------------:|---------------------:|--------------:|--------------:|------------------:|---------------:|----------------:|:------------------------|:------------------------------|:----------------|:------------------|
|   2011 | July    | Minnesota    | MRO           | East North Central |            -0.3 | normal             | severe weather     |              3060 |              nan |                70000 |          9.28 |   6.56252e+06 |       2.5957e+06  |          73.27 |            2.14 | 2011-07-01 17:00:00     | 2011-07-03 20:00:00           | False           | True              |
|   2014 | May     | Minnesota    | MRO           | East North Central |            -0.1 | normal             | intentional attack |                 1 |              nan |                  nan |          9.28 |   5.28423e+06 |       2.64074e+06 |          73.27 |            2.14 | 2014-05-11 18:38:00     | 2014-05-11 18:39:00           | False           | False             |
|   2010 | October | Minnesota    | MRO           | East North Central |            -1.5 | cold               | severe weather     |              3000 |              nan |                70000 |          8.15 |   5.22212e+06 |       2.5869e+06  |          73.27 |            2.14 | 2010-10-26 20:00:00     | 2010-10-28 22:00:00           | False           | True              |
|   2012 | June    | Minnesota    | MRO           | East North Central |            -0.1 | normal             | severe weather     |              2550 |              nan |                68200 |          9.19 |   5.78706e+06 |       2.60681e+06 |          73.27 |            2.14 | 2012-06-19 04:30:00     | 2012-06-20 23:00:00           | False           | True              |
|   2015 | July    | Minnesota    | MRO           | East North Central |             1.2 | warm               | severe weather     |              1740 |              250 |               250000 |         10.43 |   5.97034e+06 |       2.67353e+06 |          73.27 |            2.14 | 2015-07-18 02:00:00     | 2015-07-19 07:00:00           | False           | True              |

### Exploratory Data Analysis
#### Univariate Analysis
To understand the data, I performed a univariate analysis on several variables to understand the distribution relative to number of power outages.

First, I examined the distribution of years.
<iframe
  src="assets/outage_per_year.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The number of power outages each year has increased slightly beginning in 2000 with a sharp increase and peak in 2011 before gradually decreasing until 2016.

As I will focus on the number of customers affected by power outages in my analysis, I examine the distribution of customers affected next.
<iframe
  src="assets/cust_affect.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The histogram indicates that the number of customers affected is heavily skewed to the right with a peak at the 0 to 49,999 customers bin. This suggests that outages affecting a smaller number of customers are more frequent than those that affect over 500,000 customers.

To understand the number of customers affected in different states, I created a choropleth map.
<iframe
  src="assets/map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
California has the highest number of customers affected with over 25 million total customers affected. Texas and Michigan are next with over 20 million and 13 million total customers affected respectively. Generally, there appears to be more states with over 4 million total customers affected in the northeast region of the US.

#### Bivariate Analysis
In order to identify relationships between features, I performed several bivariate analyses.

First, I plotted the percentage of state population living in urban areas, which I will call urban population percentage, against the number of customers affected.
<iframe
  src="assets/cust_affect_vs_urban.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
There seems to be a slight positive correlation with more customers affected in states with higher urban population percentage. However, most of the data remains clustered in under 1 million customers affected for the entire range of urban population percentage.

I also looked at the number of customers affected per climate region, including Hawaii and Alaska. 
<iframe
  src="assets/cust_affect_vs_region.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
I found that there are more customers affected in the Northeast, West, South, and Southwest regions of the US, which all saw over 20 million total customers affected. On the other hand, the Southwest and West North Central regions along with Hawaii and Alaska all had under 2 million total customers affected.

#### Aggragate Analysis
I first found the average outage duration and number of customers affected for each cause category.

| CAUSE.CATEGORY                |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |
|:------------------------------|------------------:|---------------------:|
| equipment failure             |          1816.91  |        101936        |
| fuel supply emergency         |         13484     |             0.142857 |
| intentional attack            |           429.98  |          1790.53     |
| islanding                     |           200.545 |          6169.09     |
| public appeal                 |          1468.45  |          7618.76     |
| severe weather                |          3883.99  |        188575        |
| system operability disruption |           728.87  |        211066        |

This chart provides interesting information about what outage cause categories tend to have high or low duration and number of customers affected. For example, fuel supply emergencies tend to have long outage durations with few customers affected. On the other hand, outages caused by equipment failure, severe weather, and system operability disruption all affect a large number of customers, but have relatively short outage durations compared to outages caused by fuel supply emergencies.

I also found the number of power outages that are major and not major, as decided by the column `'is_MAJOR_OUTAGE'`, for each cause category.

| CAUSE.CATEGORY                |   not_major |   is_major |
|:------------------------------|------------:|-----------:|
| equipment failure             |          18 |         12 |
| fuel supply emergency         |           7 |          0 |
| intentional attack            |         197 |          2 |
| islanding                     |          34 |          0 |
| public appeal                 |          21 |          0 |
| severe weather                |         101 |        616 |
| system operability disruption |          40 |         43 |

This pivot table shows which cause categories tend to lead to major or non-major outages. Equipment failure and system operability disruption tend to lead equally to major and non-major outages. Fuel supply emergencies, intentional attacks, islanding, and public appeal lead to non-major outages. However, severe weather often leads to major outages.

## Assessment of Missingness
### NMAR Analysis
One column that may be Not Missing At Random (NMAR) is `'OUTAGE.DURATION'`. Extremely short outages lasting only a few seconds may be reported as missing. Therefore, values that should be reported as 0 minutes may all be missing from the data. 

Certain sensors may not have low enough thresholds for recording duration of an outage while others are able to record outages lasting only a few seconds. Therefore, an additional column of minimum duration detectable can make `'OUTAGE.DURATION'` Missing At Random (MAR) dependent on minimum duration detectable. Higher minimum duration detectable values would correspond to missing outage durations as those sensors would not be able to detect short outages.

### Missingness Dependency
To understand the missingness in one of my key columns for analysis, `'CUSTOMERS.AFFECTED'`, I will test if the missingness in this column is dependent on the columns `'CLIMATE.CATEGORY'` and `'TOTAL.CUSTOMERS'`. For both permutation tests, I will use a significance level of 0.05
#### Climate Category
I will use a test statistic of total variation distance (TVD) to compare the categorical distribution of `'CLIMATE.CATEGORY'` when `'CUSTOMERS.AFFECTED'` is missing and when it is not missing.

**Null Hypothesis:** The distribution of `'CLIMATE.CATEGORY'` is the same when `'CUSTOMERS.AFFECTED'` is missing and when it is not missing. (The missingness in `'CUSTOMERS.AFFECTED'` is not dependent on `'CLIMATE.CATEGORY'`)

**Alternate Hypothesis:** The distribution of `'CLIMATE.CATEGORY'` is different when `'CUSTOMERS.AFFECTED'` is missing and when it is not missing. (The missingness in `'CUSTOMERS.AFFECTED'` is dependent on `'CLIMATE.CATEGORY'`)

<iframe
  src="assets/miss_by_climate_cat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After performing a permutation test with 10,000 simulations, I found an  **observed statistic** of about 0.0353 with a **p-value** of 0.3679. Because the p-value is > 0.05, I fail to reject the null hypothesis. There is not convincing evidence of a significant difference in the distribution of `'CLIMATE.CATEGORY'` when `'CUSTOMERS.AFFECTED'` is missing and not missing. This indicates that the missingness in `'CUSTOMERS.AFFECTED'` is not dependent on `'CLIMATE.CATEGORY'`.

<iframe
  src="assets/miss_emp_by_climate_cat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Total Customers
I will use a the Kolmogorov-Smirnov (K-S) test statistic to compare the numerical distribution of `'TOTAL.CUSTOMERS'` when `'CUSTOMERS.AFFECTED'` is missing and when it is not missing. I chose this test statistic as the numerical distributions have different shapes.

**Null Hypothesis:** The distribution of `'TOTAL.CUSTOMERS'` is the same when `'CUSTOMERS.AFFECTED'` is missing and when it is not missing. (The missingness in `'CUSTOMERS.AFFECTED'` is not dependent on `'TOTAL.CUSTOMERS'`)

**Alternate Hypothesis:** The distribution of `'TOTAL.CUSTOMERS'` is different when `'CUSTOMERS.AFFECTED'` is missing and when it is not missing. (The missingness in `'CUSTOMERS.AFFECTED'` is dependent on `'TOTAL.CUSTOMERS'`)

<iframe
  src="assets/miss_by_total_cust.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After performing a permutation test with 10,000 simulations, I found an **observed statistic** of about 0.1976 with a **p-value** of approximately 0.0. Becasue the p-value < 0.05, I reject the null hypothesis. There is convincing evidence of a significant difference in the distribution of `'TOTAL.CUSTOMERS'` when `'CUSTOMERS.AFFECTED'` is missing and not missing. This indicates that the missingness in `'CUSTOMERS.AFFECTED'` is dependent on `'TOTAL.CUSTOMERS'`.

<iframe
  src="assets/miss_emp_by_total_cust.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing
The main focus of this project is on the factors that may impact the number of customers affected by power outages. One such factor that I will investigate through a permutation test is urbanization level. Using the column `'is_HIGH_URBAN'`, I will compare the average number of customers affected in areas with high urbanization and low urbanization levels. This analysis will provide insight into whether the level of urbanization plays a role or impacts the number of customers affected in a power outage. With the national trend towards urban areas, I want to know if the average number of customers affected is greater in areas with high urbanization.

**Null Hypothesis:** The mean number of customers affected by power outages is the same for high and low urbanization levels 

**Alternate Hypothesis:** The mean number of customers affected by power outages is greater for high than for low urbanization levels

**Test Statistic:** Difference in mean number of customers affected in high urbanization and low urbanization areas (high_urban_mean - low_urban_mean)

**Significance Level:** 0.05

I chose a permutation test in order to test whether the two samples of high urbanization levels and low urbanization levels were from the same population. I set the alternate hypothesis as high urbanization levels having more customers affected as there is a current move towards cities and increasing population in urban areas. If there are more customers affected on average in states with high urbanization areas, there should be greater concern and precautionary measures taken to ensure the safety of these customers in the event of a power outage. By looking at the difference in mean number of customers in these two urbanization levels, I can see which level has a greater mean and come to a more effective conclusion. 

<iframe
  src="assets/perm_urban.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After performing a permutation test with 10,000 simulations, I found an **observed statistic** of about 45705.2383 with a **p-value** of about 0.0026. Because the p-value < 0.05, I reject the null hypothesis. There is convincing evidence that the mean number of customers affected by power outages is significantly higher for high than for low urbanization levels. This implies that areas with high urbanization tend to affect a greater number of customers on average, causing a greater need for precautions in place in high urban areas to lessen impact of power outages.

<iframe
  src="assets/perm_emp.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem

## Final Model

## Fairness Analysis