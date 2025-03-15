
# "Why So Serious?" - Outage Duration Level Classifier  
**Analyzing patterns in electricity disruption severity**

---
Site: https://tzheng846.github.io/Power_Outage_Analysis/

## Introduction
We aim to answer: **What are the characteristics of different levels of outage duration?**   

Almost everyone in the US has experianced some sort of power outages. It ranges from mildly inconvient of 2 minute all the way to devestating month long issue. We want to learn more about what makes these outages different and what factor influence their duration the most. In this project, we delve into a comprehensive dataset documenting major power outages across the continental United States. This dataset represents the cumulative data of most major recorded outages in the US. We hope to find our answers hidden in this rich source of information. Our goal is to uncover patterns and characteristics that differentiate short-term outages from prolonged ones. By analyzing variables such as climate conditions, population density, outage causes, and others we aim to answer the question: What are the key factors that influence the duration of power outages?  

To start we will be looking at the following columns:
```python 
['YEAR','MONTH','U.S._STATE','NERC.REGION', 'CLIMATE.REGION', 'ANOMALY.LEVEL', 'CLIMATE.CATEGORY','OUTAGE.START.DATE', 'OUTAGE.START.TIME', 'CAUSE.CATEGORY', 'CAUSE.CATEGORY.DETAIL','HURRICANE.NAMES', 'OUTAGE.DURATION', 'DEMAND.LOSS.MW','CUSTOMERS.AFFECTED', 'RES.PRICE', 'COM.PRICE', 'IND.PRICE','TOTAL.PRICE','TOTAL.CUSTOMERS', 'POPULATION', 'AREAPCT_URBAN', 'PCT_LAND']
```  
  
## Column Dictionary

| Variable Name          | Description |
|------------------------|-------------|
| **YEAR**               | Indicates the year when the outage event occurred |
| **MONTH**              | Indicates the month when the outage event occurred |
| **U.S._STATE**         | Represents all the states in the continental U.S. |
| **NERC.REGION**        | The North American Electric Reliability Corporation (NERC) regions involved in the outage event |
| **CLIMATE.REGION**     | U.S. Climate regions as specified by the National Centers for Environmental Information |
| **ANOMALY.LEVEL**      | Represents the oceanic El Niño/La Niña (ONI) index referring to climate episodes |
| **CLIMATE.CATEGORY**   | Climate categories: “Warm”, “Cold” or “Normal” episodes based on the Oceanic Niño Index (ONI) |
| **OUTAGE.START.DATE**  | The day of the year when the outage event started (as reported by the corresponding Utility) |
| **OUTAGE.START.TIME**  | The time of the day when the outage event started (as reported by the corresponding Utility) |
| **CAUSE.CATEGORY**     | Categories of all the events causing major power outages |
| **CAUSE.CATEGORY.DETAIL** | Detailed description of the events causing major power outages |
| **HURRICANE.NAMES**    | If the outage is due to a hurricane, this variable provides the hurricane name |
| **OUTAGE.DURATION**    | Duration of outage events (in minutes) |
| **DEMAND.LOSS.MW**     | Amount of peak demand lost during an outage event (in Megawatts) |
| **CUSTOMERS.AFFECTED** | Number of customers affected by the power outage event |
| **RES.PRICE**          | Monthly electricity price in the residential sector (cents/kilowatt-hour) |
| **COM.PRICE**          | Monthly electricity price in the commercial sector (cents/kilowatt-hour) |
| **IND.PRICE**          | Monthly electricity price in the industrial sector (cents/kilowatt-hour) |
| **TOTAL.PRICE**        | Average monthly electricity price in the U.S. state (cents/kilowatt-hour) |
| **TOTAL.CUSTOMERS**    | Annual number of total customers served in the U.S. state |
| **POPULATION**         | Population in the U.S. state in a year |
| **AREAPCT_URBAN**      | Percentage of the land area of the U.S. state represented by urban areas |
| **PCT_LAND**           | Percentage of land area in the U.S. state as compared to the overall land area in the continental U.S. |

---
 

# Data Cleaning and Exploratory Data Analysis

## Data Cleaning

In this section, we detail the data cleaning steps taken to prepare the dataset for analysis. The dataset, which contains information about power outages, required several cleaning steps to ensure the data was suitable for analysis.

1. **Handling Missing Values**: We dropped rows where the `OUTAGE.DURATION` was missing, as this was our primary variable of interest. Additionally, we replaced zeros in `DEMAND.LOSS.MW` and `CUSTOMERS.AFFECTED` with `NaN` values, as zeros in these columns were unlikely to represent actual data and were more likely to indicate missing values.

2. **Data Type Conversion**: We converted `OUTAGE.START.DATE` to a datetime object and `OUTAGE.START.TIME` to a time object. This allowed us to perform time-based analysis more effectively.

3. **Creating New Columns**: We created a new column, `DAY.OR.NIGHT`, to indicate whether the outage started during the day or night. This was determined by checking if the outage start time was between 9 AM and 5 PM. We also created a `HAS.HURRICANE` column to indicate whether the outage was associated with a hurricane, based on the presence of a hurricane name in the `HURRICANE.NAMES` column.

4. **Dropping Unnecessary Columns**: We dropped the `HURRICANE.NAMES` column after extracting the necessary information into the `HAS.HURRICANE` column.

These cleaning steps ensured that our dataset was free of missing values in critical columns, had appropriate data types, and included relevant derived features for our analysis.

## Exploratory analysis
### Univariate Analysis
population vs count:  
The following shows the number of outages for each population range. We see that it's multimodal with concentration at 5M, 20M, and 35-40M. This hints that most outages occurs in areas with low population. This would make sense since smaller citites might not have the proper outage protection. 

<iframe
  src="project04/asset/population-count.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

climate vs count:  
The following shows the three catagory of climate and the number of occurance of outages. We see that most outages occurs in normal, with cold climate being second. This makes sense since most of US is catagorized as 'normal' climate, therefore will have more instances of outages. 

<iframe
  src="project04/asset/climate-count.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Duration level vs count:  
The following chart shows the number of occurance of outages for each duration level. Note that the catagories are not in order of duration. We see that most outages are momentary and short termed. 

<iframe
  src="project04/asset/duration-count.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Year vs Outage Duration:  
The following chart shows that there is a general downward trend since 2002 to 2016 of outage duration, despite each year varying wildly. 

<iframe
  src="project04/asset/year-duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

Outage Duration vs Population:  
The following chart shows the distribution of population in comaparison to outage duration. Similar to the previous outage count chart, we see high concentration in three areas: namely 0-13M, 17-27M, and 35-40M range. 

<iframe
  src="project04/asset/population-duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
 
Anomoly level vs climate category:  
The following chart comares the number of occurance of outages for each climate category and anomoly level

<iframe
  src="project04/asset/level-count.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Heatmap:  
The following chart compares climate categroy with cause category and the difference in number of occurances. We see severe weather has the most effect to outage, as well as intentional attacks. The others have not much effects. 

<iframe
  src="project04/asset/cause-climate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

State vs anomaly level:  
The following chart shows the max anomaly level of each state. Note all states are not shown on bottom. Hover over bars to see more details. We see that Kansas and South Dakota are significantly higher than the rest. 

<iframe
  src="project04/asset/state-level.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Grouping and Aggregates

| NERC.REGION   |   cold |   normal |   warm |
|:--------------|-------:|---------:|-------:|
| ECAR          |      2 |       22 |      8 |
| FRCC          |     10 |       19 |     14 |
| FRCC, SERC    |    nan |      nan |      1 |
| HECO          |    nan |        1 |      2 |
| HI            |      1 |      nan |    nan |
| MRO           |     13 |       25 |      6 |
| NPCC          |     46 |       83 |     18 |
| PR            |    nan |        1 |    nan |
| RFC           |    149 |      219 |     48 |
| SERC          |     63 |       96 |     35 |
| SPP           |     15 |       27 |     20 |
| TRE           |     30 |       51 |     27 |
| WECC          |    134 |      186 |    104 |

# Assessment of Missingness

## NMAR Analysis

Among all columns with missing values, 'DEMAND.LOSS.MW' and 'CUSTOMERS.AFFECTED' has a high chance of being NMAR. Because these values could be missing due to the data collection method. As these values could bring direct and crucial negative effects to the power companies, they might not report the data to the institute collecting the data. 

With addition information about the companies such as their name, their size, or their annual revenue, it is possible to determine whether the missing values are dependent on the companies or their performances and thus making them MAR. 

## Missingness Dependency

We focus on the distribution of the column 'TOTAL.PRICE' and test it again the columns 'U.S._STATE' and 'CLIMATE.CATEGORY'.

1. 'U.S._STATE' vs.'TOTAL.PRICE' 
- Null Hypothesis: The distribution of 'U.S._STATE' is the same when 'TOTAL.PRICE' is missing vs not missing.
- Alternate Hypothesis: The distribution of 'U.S._STATE' is not the same when 'TOTAL.PRICE' is missing vs not missing.

<iframe
  src="project04/asset/prop-STATE.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="project04/asset/tvd-prob.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The observed TVD is around 0.4099 with the p-values of 0.256, which is higher than 0.05. As a result, we fail to reject the null hypothesis and conclude that the distribution of 'U.S._STATE' where 'TOTAL.PRICE' is missing has no significant difference from the distribution where 'TOTAL.PRICE' is not missing. Therefore, we can so far conclued that the missness of 'TOTAL.PRICE' does not depend on 'U.S._STATE'. 

2. 'CLIMATE.CATEGORY' vs. 'TOTAL.PRICE'
- Null Hypothesis: The distribution of 'CLIMATE.CATEGORY' is the same when 'TOTAL.PRICE' is missing vs not missing.
- Alternate Hypothesis: The distribution of ''CLIMATE.CATEGORY' is not the same when 'TOTAL.PRICE' is missing vs not missing.
<iframe
  src="project04/asset/prop-climCatagory.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="project04/asset/tvd-prob2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The observed TVD is around 0.0164 with the p-values of 0.001, which is less than 0.05. As a result, we reject the null hypothesis and conclude that the distribution of 'CLIMATE.CATEGORY' where 'TOTAL.PRICE' is missing has a significant difference from the distribution where 'TOTAL.PRICE' is not missing. Therefore, we can say that the missing mechanism of 'TOTAL.PRICE' is MAR as it does depends on 'CLIMATE.CATEGORY'. 

## Hypothesis Testing
We will be testing whether Duration is longer when it begins at night time. The columns that are use for testing are 'OUTAGE.DURATION' and 'DAY.OR.NIGHT'.

- Null Hypothesis: The average duration when it begins during night is the same as the duration that begins during day time. 
- Alternative Hypothesis: The average duration when it begins during night is longer than the duration that begins during day time.
- Test Statistic: Difference in mean (Mean duration begins during night - Mean duration begins during day time).

<iframe
  src="project04/asset/hypothesisTest.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The above graph show the observed difference vs. the differences' empirical distribution. 
After performing a permutation test with 10000 simulations assuming that the null hypothesis was correct, we got the p-value of 0.0. Obviously, 0.0 < 0.05, implying a statistical significance, so we reject the null hypothesis and conclude that the average duration when it begins during night time is longer than the duration that begins during day time.

## Framing a Prediction Problem

We want to classified the level of outage duration into 'Momentary', 'Short-term', 'Extended', 'Extended', and 'Chronic', so it would be a clssification problem. 
The metric we'll be using would be the acuracy because that's one the easist metric to be interpreted and we want the user to quickly understand our model's performance. 
During the prediction, user would have information to the following values: 'NERC.REGION', 'CLIMATE.REGION', 'CAUSE.CATEGORY', 'CAUSE.CATEGORY.DETAIL', 'DEMAND.LOSS.MW', 'CUSTOMERS.AFFECTED', 'ANOMALY.LEVEL', 'HAS.HURRICANE'. These information would be the input of our classifying prediction. 

## Baseline Model

Our model is a DecisionTreeClassifier that uses two nominol features: 'NERC.REGION' and 'CLIMATE.REGION' to predict the duration level of a given outage. The information give by the model is useful for the govenment to determine whether this outage is serious or not.

Our model is a DecisionTreeClassifier that uses two nominol features: 'NERC.REGION' and 'CLIMATE.REGION' to predict the duration level of a given outage. The information give by the model is useful for the govenment to determine whether this outage is serious or not.

The model's performance in terms of accuracy is around 0.35 when tested on the test set, which is not a good value and means that the model requires critical inprovement during the next step. To do so, we plan to change the classify model, add more features for training, and use grid search to tune the hyperparameters.

## Final Model
In our final model, we've decided to use a RandomForestClassifier to classify the duration level using the following features: 'NERC.REGION', 'CLIMATE.REGION', 'CAUSE.CATEGORY', 'CAUSE.CATEGORY.DETAIL', 'DEMAND.LOSS.MW', 'CUSTOMERS.AFFECTED', 'ANOMALY.LEVEL', 'HAS.HURRICANE'.  

Base on our baseline model, we added 6 new features. 
1. 'CAUSE.CATEGORY' (nominal): We add it because the way how outage happened can significantly impact its duration. We use OneHotEncoding to deal with it as it's a nominal feature.
2. 'CAUSE.CATEGORY.DETAIL' (nominal): We add it for the similar reason as 'CAUSE.CATEGORY', as it could help split when two levels have the same 'CAUSE.CATEGORY'. Similary, we use OneHotEncoding to deal with it because it's a nominal feature. 
3. 'DEMAND.LOSS.MW' (quantative): We added it because a outage with longer duration cause a larger loss in demand. We use IterativeImputer to impute the missing value because quite a lot of the values are missing but it provides important informations to the duration and we don't want to loss it. After imputing we standardized it to let the the values comparable. 
4. 'CUSTOMERS.AFFECTED' (quantative): We added it because ast he outage last longer, more people will be affected, so there'sa positive correlation between them. We use KNNImputer to impute the missing values because it does not have that much missing values compare to 'DEMAND.LOSS.MW' and we don't want to loss its important informations. 
5. 'ANOMALY.LEVEL' (quantative): We added 'ANOMALY.LEVEL' because it refleact to the climate status, specifically temperature, of the period of time and we believe temperature can affect people's electricity usage habits and further impact the outage duration. Since it's a quantative feature without missing values, we leave it as it was. 
6. 'HAS.HURRICANE' (nominal): We added it because hurricane causes serious damage, which can led to the postpone of outage restoration work. We use OneHotEncoding to deal with this nominal feature.  

We use GridSearchCV to find the best hyperparameters for our RandomForestClassifier.
The parameters are: 
- 'max_depth': 10
- 'min_samples_split': 20
- 'n_estimators': 50  

Finally, our final model achieved the accuracy score of 0.54. Compare to the accuracy of 0.35 from the baseline model, our final model has a huge improvement on its accuracy, meaning that it's a way better model compare to the baseline model.

## Fairness Analysis

Our groups for the fairness analysis are whether the outage happened with hurricane or not. We choose to use hurricane becuase it has a strong correlation between outage duration and we want to make sure that out model is predicing correctly. We use recall as our metric because we want to make sure that the model predicted correctly everytime there is a hurricane.

- Null Hypothesis: The model is fair. Its recall is the same whether there's hurricane or not, any difference should be due to random chance.
- Alternative Hypothesis: The model is unfair. Its recall is significantly different whether there's hurricane or not.

We use the recall_score of (hurricame - no hurricane) to record the result of the experiment 1000 times with the standard of 0.05.

<iframe
  src="project04/asset/recall-probabaility.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We get the p-value of 0.0 after the test, which is less than the standard 0.05. Therefore, we reject the null hypothesis and conclude that our model is unfair as the significant different in recall from the two groups. 

References:
- https://docs.lib.purdue.edu/cgi/viewcontent.cgi?params=/context/civeng/article/1035/&path_info=10.1016_j.dib.2018.06.067.pdf