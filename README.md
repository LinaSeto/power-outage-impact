# Power Outage Impact Analysis
Project for DSC80 at UCSD

by Lina Seto

## Introduction
A power outage is defined as an interruption in the supply of electricity to end users. Millions of customers experience power outages each year with the US Census Bureau reporting 1 in 4 households experiencing at least one. Power outages can disrupt daily life in the loss of lighting and hot water, but there are also more severe consequences such as failure of medical equipment, release of toxic carbon monoxide from gas stoves, or lack of traffic lights. In any case, the number of customers affected by a power outage instance is an extremely important indicator in the severity and scope of a power outage. Therefore, I will conduct this project around the central question: **What factors control or impact the number of customers affected by power outages?**

Throughout this project, I will use the power outages dataset. This dataset contains major power outage data in the continental United States from January 2000 to July 2016. The dataset also presents data on geographic location, date, time, regional climatic information, land-use characteristics, electricity consumption patterns, and economic characteristics of states affected by outages. The dataset was accessed through the Laboratory for Advancing Sustainable Critical Infrastructure at Purdue University.

The dataset contains 1534 rows where each row corresponds to a power outage instance. While the raw data contains 57 columns, I will choose the following 20 columns to use in my analysis.

| Column                  | Description                                                                                                                                                                                                                   |
|:------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 'YEAR'                    | Indicates the year when the outage event occurred                                                                                                                                                                             |
| 'MONTH'                   | Indicates the month when the outage event occurred                                                                                                                                                                            |
| 'U.S._STATE'              | Represents all the states in the continental U.S.                                                                                                                                                                             |
| 'NERC.REGION'             | The North American Electric Reliability Corporation (NERC) regions involved in the outage event                                                                                                                               |
| 'CLIMATE.REGION'          | U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)                                                                              |
| 'ANOMALY.LEVEL'           | This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W) |
| 'CLIMATE.CATEGORY'        | This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)                      |
| 'OUTAGE.START.DATE'       | This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region)                                                                                            |
| 'OUTAGE.START.TIME'       | This variable indicates the time of the day when the outage event started (as reported by the corresponding Utility in the region)                                                                                            |
| 'OUTAGE.RESTORATION.DATE' | This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region)                                                                             |
| 'OUTAGE.RESTORATION.TIME' | This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region)                                                                             |
| 'CAUSE.CATEGORY'          | Categories of all the events causing the major power outages                                                                                                                                                                  |
| 'OUTAGE.DURATION'         | Duration of outage events (in minutes)                                                                                                                                                                                        |
| 'DEMAND.LOSS.MW'          | Amount of peak demand lost during an outage event (in Megawatt) [but in many cases, total demand is reported]                                                                                                                 |
| 'CUSTOMERS.AFFECTED'      | Number of customers affected by the power outage event                                                                                                                                                                        |
| 'TOTAL.PRICE'             | Average monthly electricity price in the U.S. state (cents/kilowatt-hour)                                                                                                                                                     |
| 'TOTAL.SALES'             | Total electricity consumption in the U.S. state (megawatt-hour)                                                                                                                                                               |
| 'TOTAL.CUSTOMERS'         | Annual number of total customers served in the U.S. state                                                                                                                                                                     |
| 'POPPCT_URBAN'            | Percentage of the total population of the U.S. state represented by the urban population (in %)                                                                                                                               |
| 'AREAPCT_URBAN'           | Percentage of the land area of the U.S. state represented by the land area of the urban areas (in %)                                                                                                                          |

With this data, I will analyze the effect of urbanization level on the number of customers affected by a power outage and use various features to predict the severity of a power outage. Such a prediction model may be able to prepare customers and local government officials for a potential major power outage in order to enact safety precautions to minimize the impact and damage.


## Data Cleaning and Exploratory Data Analysis

## Hypothesis Testing

## Framing a Prediction Problem

## Final Model

## Fairness Analysis