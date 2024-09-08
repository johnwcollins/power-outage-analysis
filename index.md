---
layout: default
title: Power Outages Data Analysis
---

# Power Outage Analysis Project

By John Collins

## Introduction

This project examines a dataset of major power outages in the USA from January 2000 to July 2016. These outages affected thousands of people and caused unplanned energy demand lost in the hundreds of MegaWatts. The dataset, obtained from Perdue University contains information about the outages themselves, along with geographical, climactic, land use, electricity consumption patterns and economic characteristics.

##### My research question is:

"Are power outages caused by intentional attacks more frequent or longer lasting than outages caused by other factors?"

This question is important because understanding these patterns can help energy companies and policymakers to develop more effective preventative measures and response strategies. By comparing the duration and frequency of outages caused by intentional attacks to those caused by other factors we can gain insights in the relative impact and severity of these events.

##### Dataset Overview

The original dataset contains 1534 rows each representing an outage event, and 57 columns. We focus on a subset of these columns that are most relevant to our research question. They key columns are:

- YEAR and MONTH: Times of outages.
- U.S._STATE and NERC.REGION: Location information.
- CLIMATE.REGION and ANOMALY.LEVEL: Environmental context
- OUTAGE.START and OUTAGE.RESTORATION: Exact timing.
- CAUSE.CATEGORY and CAUSE.CATEGORY.DETAIL: The cause of the outages.
- OUTAGE.DURATION: Length in minutes of the outage.
- CUSTOMERS.AFFECTED: The scale of the impact.

## Data Cleaning and Exploratory Data Analysis

##### Data Cleaning

To prepare the dataset for analysis, we used several data cleaning steps:

1. Column selection: We retained only the necessary columns for our data analysis, reducing the number of columns from 57 to 20.
2. Handling Missing Values:
	- Rows with NA values in critical columns were dropped. This only affected 9 rows, which was  
	deemed an insignificant amount of data loss.
	- For OUTAGE.DURATION and CUSTOMERS.AFFECTED, missing values were imputed using median 		values and grouped by NERC region and cause category. If still missing, the overall median 	was used.
	- Flags were created to indicate which rows had imputed values for the duration and 		customers affected columns
3. Date and Time Handling:
	- Combined separate data and time columns into single datetime objects for outage start and 	restoration times.
	- Original separate date and time columns were dropped.
4. Column Naming: 
	- Standardized column names by converting to lowercase and replacing periods with 			underscores. Specifically changed 'u_s__state' to 'state'.
5. Cause Category Detail: For missing values in CAUSE.CATEGORY.DETAIL, we filled the missing values with corresponding CAUSE.CATEGORY value.

##### Here is a look at the cleaned data frame:


|   OBS |   year |   month | state     | postal_code   | nerc_region   | climate_region     |   anomaly_level | climate_category   | cause_category     | cause_category_detail   |   outage_duration |   customers_affected |   total_customers |   pc_realgsp_state |   poppct_urban |   popden_urban | outage_start        | outage_restoration   | duration_imputed   | customers_affected_imputed   | cause_detail_missing   |
|------:|-------:|--------:|:----------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|---------------------:|------------------:|-------------------:|---------------:|---------------:|:--------------------|:---------------------|:-------------------|:-----------------------------|:-----------------------|
|     1 |   2011 |       7 | Minnesota | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | severe weather          |              3060 |                70000 |           2595696 |              51268 |          73.27 |           2279 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  | False              | False                        | True                   |
|     2 |   2014 |       5 | Minnesota | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |                 1 |                    0 |           2640737 |              53499 |          73.27 |           2279 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  | False              | True                         | False                  |
|     3 |   2010 |      10 | Minnesota | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |              3000 |                70000 |           2586905 |              50447 |          73.27 |           2279 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  | False              | False                        | False                  |
|     4 |   2012 |       6 | Minnesota | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |              2550 |                68200 |           2606813 |              51598 |          73.27 |           2279 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  | False              | False                        | False                  |
|     5 |   2015 |       7 | Minnesota | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | severe weather          |              1740 |               250000 |           2673531 |              54431 |          73.27 |           2279 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  | False              | False                        | True                   |

##### Univariate Analysis:

To begin our exploration of power outages, we will look at the distribution of outage durations across different cause categories.

<iframe
  src="assets/outage_durations_vs_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This box plot reveals the distribution of outage durations for each cause category. You can see that intentional attacks shows a narrow range of durations compared to other causes, like fuel supply emergencies. This suggests that while intentional attacks do not typically result in the longest outages, they might have a more predictable duration window.

##### Bivariate Analysis:

To further explore the relationship between outage causes and their impact, we examined the correlation between outage duration and the number of customers affected.

<iframe
  src="assets/customers_vs_cause_duration_part1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

And now the same plot with only intentional attack values and plot scaled down.

<iframe
  src="assets/customers_vs_cause_duration_part2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Please note the change in the scale of the x and y axis in the second plot.

These scatter plots illustrate the relationship between outage duration and the number of customers affected. There are points colored by their cause category. As you can see, intentional attacks are not visible in the first plot. When we check in the second plot it indicates they cluster together more so than other cause category variables, and that other causes tend to have a broader range of duration and customer impact. 


##### Interesting aggregates:

To get a more comprehensive view of outage causes across different regions, we will look at a heat map of outage frequencies.

<iframe
  src="assets/causes_vs_NERC_region_heat_map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


This heat map shows a clear visualization of outage frequencies across different NERC regions and cause categories. It allows us to quickly identify which regions experience more intentional attacks compared to other causes. For instance, we can see that severe weather is the dominant cause of power outages across most regions, while intentional attacks have varying frequency depending on region. 

Lastly, let's look at the trend of intentional attacks over the years.

|   year |   intentional attack |
|-------:|---------------------:|
|   2000 |                    2 |
|   2001 |                    0 |
|   2002 |                    1 |
|   2003 |                    2 |
|   2004 |                    0 |
|   2005 |                    0 |
|   2006 |                    0 |
|   2007 |                    0 |
|   2008 |                    0 |
|   2009 |                    0 |
|   2010 |                    0 |
|   2011 |                  121 |
|   2012 |                   89 |
|   2013 |                   80 |
|   2014 |                   47 |
|   2015 |                   43 |
|   2016 |                   33 |

We see an interesting trend, with a massive spike in 2011 that levels out with time.

This time series data shows the frequency of intentional attacks causing power outages by each year. When examine the trend, we can determine if intentional attacks are becoming more or less frequent over time. This is crucial for understanding the evolving nature of this threat to our power infrastructure.

Our analysis reveals that while intentional attacks may not always result in the longest outages or affect the most customers, they represent a significant threat across different regions. The frequency of these attacks appears to fluctuate, and spike dramatically, over time and geographic areas, indicating the need for prevention strategies and response plans.

## Assessment of Missingness

#### NMAR Analysis:

After examining the dataset and thinking about the process of generating this data, it is possible that the column CAUSE.CATEGORY.DETAIL might not be NMAR (Not Missing At Random). This column provides more specific information about the cause of each power outage and details about intentional attacks.

The missingness of this column could be NMAR because:
1. The information is highly sensitive and could lead to security related issues if all of the information was released to the public. The data itself could be the reason why it is not taken down.
2. There could be ongoing investigations into the case. If the attack was complex enough there would be details that were not immediately know at the time of taking down the data.
3. There could be an inherent reporting bias. Intentional attacks could be considered as security failures and there might be hesitation in providing all the details.

The missingness of the data is likely related to itself, which is what defines NMAR data.

The potentiality of this data being MAR could determined if we collected more data. Such as:
- If we had the proper security clearance to access the full details of each outage.
- If we knew the time that elapsed from the outage to the report submission.
- If we knew the reporting agency of each outage event.

With this additional information we might be able to explain the missingness based on other columns of data. For example we could find that a specific agency was more likely to not fill in additional details.

##### Missingness Dependency Analysis:

For our missingness dependency analysis, we focused on the CUSTOMERS.AFFECTED column, which shows non-trivial missingness in our dataset. We preformed permutation tests to an analyze the dependency of the missingness of this column on other variables.

We ran two permutation tests:

<iframe
  src="assets/permutation_test_outage_duration_vs_missingness_customers_affected.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>


Dependency on Outage Duration:
	- P-Value: 0.018
	- This missingness in CUSTOMERS.AFFECTED does depend on OUTAGE.DURATION. This low p-	value (< 0.05) suggest a relationship between outage duration and whether a customers 	data is missing.

<iframe
  src="assets/permutation_test_year_vs_missingness_customers_affected.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

Dependency on Year:
	- P-Value: 0.0
	- This strongly suggests that the missingness of the data is highly dependent on the 	year of the outage.

To go one step further we illustrate the dependency of missignness on outage duration, we made a box plot:

<iframe
  src="assets/boxplot_outage_duration_vs_customers_affected.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This box plot shows us comparison is distributions between not missing (0) and missing (1) customer data. 

We can see the median difference appears to be higher when customer data is not missing.

We see the spread of the data that there is more variability when the customer data is present.

And we can clearly see that outliers frequently appear in both groups, but more frequently in non-missing data.

##### Implications:

This analysis suggests that our data is MAR (Missing At random) rather than MCAR (Missing Completely At Random), as the missingness depends on observed variables in other columns.

## Hypothesis Testing

For our hypothesis test we will compare the duration of outages caused by intentional attacks to those caused by other factors. We will focus on severe weather as a point of comparison.

Null Hypothesis(H0): On average, the duration of outages caused by intentional attacks is the same as the duration of outages caused by severe weather.

Alternative Hypothesis(H1): On average, the duration of outages caused by intentional attacks is different from the duration of outages caused by severe weather.

- Test Statistic: The difference in means. (Mean Duration of severe weather outages - 	Mean duration of intentional attack outages)
- Method: Permutation test with 10,000 simulations.
- Significance level: 0.05.

The results of our permutation test give us an observed difference in mean outage of -3019.38 minutes and a p-value of 0.0. As you can see in the graph below, it is a VERY extreme value.

<iframe
  src="assets/hypothesis_test_graph.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

With a p-value so extremely small (<0.000) we can reject the null hypothesis at significance level of 0.05. This suggests strong evidence that there is a significant difference in the average duration of outages caused by intentional attacks compared to those caused by severe weather.

The observed difference of -3019 minutes indicated that on average, outages caused by intentional attacks are significantly shorter in duration than those caused by severe weather.








## Framing a Prediction Problem

My goal is to predict whether a power outage is caused by an intentional attack based on its characteristics and regional factors. This is framed as a binary classification problem, where I aim to determine between outages caused by intentional attacks and those caused by other factors, specifically severe weather.


## Baseline Model

##### Features

The baseline model uses the following features:

- Quantitative: outage_duration, customers_affected, pc_realgsp_state, popden_urban
- Categorical: nerc_region, month

These features are chosen to capture aspects that might influence the likelihood of an intentional attack, including the scale of the outage, and regional characteristics.

##### Model and Preprocessing

We implemented a logistic regression model with balanced class weights to account for potential class imbalance. We used StandardScaler to normalize numeric features, and we used OneHotEncoder for categorial features, using drop=first to avoid multicollinearity.

##### Performance

Our baseline model achieved and F1 score of 0.84, precision of 1.00 for non-attacks and 0.72 for attacks, and finally a recall of 0.85 for non-attacks with 1.00 for attacks.

##### Key Findings

- Our model shows high precision for non-attacks but lower precision for attacks. This indicates it may have some false positives when predicting for intentional attacks.
- We see high recall for attacks, which suggests that the model is good at identifying most of the actual intentional attacks.
- The most important features were:
	1. customers_affected (importance: 9.75)
	2. nerc_region_wecc (importance: 1.29)
	3. nerc_region_FRCC (importance: 1.09)
- There is a class imbalance in the dataset with more non-attacks than attacks.


## Final Model

For our final model, we aimed to improve upon the baseline by adding better feature engineering, using a better algorithm, and preforming hyperparamter tuning.

##### Model Selection and Preprocessing 

We used a random forest classifier for our final model due to its ability to capture non-linear relationships and feature interactions. Our preprocessing	pipeline was refined to include, QuantileTransformer for 'outage_duration' to handle its potentially skewed distribution, StandardScalar for other numeric features, and OneHotEncoder for categorial features.

##### Hyperparameter Tuning

We used GridSearchCV to optimize the following hyperparamters:
- n_estimators: [50,100,200]
- max_depth: [None,10,20,30]

### Performance Metrics

##### Best Parameters:
- `classifier__max_depth`: 10
- `classifier__n_estimators`: 50

##### Classification Report:

| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| 0     | 0.97      | 0.98   | 0.97     | 220     |
| 1     | 0.94      | 0.92   | 0.93     | 84      |

**Overall Accuracy**: 0.96

| Metric           | Macro Avg | Weighted Avg |
|------------------|-----------|--------------|
| Precision        | 0.95      | 0.96         |
| Recall           | 0.95      | 0.96         |
| F1-Score         | 0.95      | 0.96         |

### F1 Score: 
- **F1 Score (Class 1)**: 0.93
- **Improvement over Baseline**: 0.0877

##### Feature Importance:

1. Outage Duration (55.1%)
2. Urban Population Density (20.5%)
3. Customers Affected (8.48%)

Duration and scale appear to be the most important predictors of intentional attacks. Urban areas show distinct patterns in intentional attack likelihood. 

#### Comparison and Interpretation

1. The final random forest model showed significant improvement over our baseline model. The F1 score improved by 8.77%. Our feature engineering and model selection efforts paid off, resulting in more accurate classifiers.
2. Precision and recall: For non-attacks the model achieved 0.94 precision and 0.92 recall. These high and balanced precision and recall scores for both classes suggest that the model is effective at identifying both intentional attacks and non-attacks with only a slight biased towards correctly identifying non-attacks.
3. The best model used 50 tress with a max depth of 10. Simplicity caused the best results.
4. The model was correct 94% of the time when predicting an attack with a 92% chance of catching all intentional attacks. This balance is great for practical applications, as it minimizes false alarms and missed detections of attacks.


## Fairness Analysis

Group X: Power outages in the WECC region.
Group Y: Power outages in the non-WECC regions.

Evaluation Metric: We chose precision as our evaluation metric, focusing on the models ability to identify intentional attacks without false positives.

Hypothesis:
- Null: Our model is fair. It's precision for predicting intentional attacks in the WEVV region and non-WEC regions are roughly the same, and any differences are due to random chance.
- Alternative: Our model is unfair. Its precision for predicting intentional attacks in the WECC region is significantly different from its precision in non-WECC regions.

Test Statistic: The difference in precision between WECC and non-WECC regions.

Significance Level: 0.05

<iframe
  src="assets/fairness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After running our permutation test we obtained the following results:
- WECC Precision: 0.9688
- Non-WECC Precision: 0.9200
- Observed difference: 0.0487
- P-Value: 0.3740



The model shows slightly higher precision in predicting intentional attacks for the WECC region compared to non-WECC regions. This means that when the model predicts an intentional attack in WECC region, it is correct about 97% of the time, versus 92% If the time in non-WECC regions.

Our p-value is much larger than 0.05 which suggests that the observed difference in precision between WECC and non-WECC regions is not statistically significant.

We fail to reject the null hypothesis, so we cannot say there is a significant difference in the way our model predicts intentional attacks in WECC vs non-WECC regions.




