
# Analysis of Power Outages in the U.S.

**By: Arthur Brown and Eli Nalebuff**

##  Introduction

We chose to explore the Power outage dataset from Purdue University. The dataset includes a breakdown of the geographic, climatic, economic, cause and time specific information associated with a given outage. After exploring the data set, we began to wonder if one could predict the length of an outage based on information regarding the outage. We thought it would be interesting to be able to provide someone experiencing an outage a prediction on what time they could expect their power to come back on.

### Breakdown of dataset:

- **Entries:** 1533
- **Relevant Columns:**
  - YEAR (year the outage occurred)
  - MONTH (month the outage occurred)
  - U.S._STATE (state the outage occurred)
  - CLIMATE.REGION (region of the U.S the outage occurred, i.e., North East)
  - CLIMATE.CATEGORY (cold, warm or hot)
  - OUTAGE.START.DATE (date outage began)
  - OUTAGE.START.TIME (time outage began)
  - OUTAGE.RESTORATION.DATE (date outage was restored)
  - OUTAGE.RESTORATION.TIME (time outage was restored)
  - OUTAGE.DURATION (length of duration in minutes)
  - CAUSE.CATEGORY (cause of outage)
 
##  Data Cleaning and Exploratory Data Analysis

The first precaution we took with the data was to ensure all numeric columns were represented as either integers or floats. We also combined the date and time columns into a consolidated `pd.Datetime` column. Additionally, we removed rows that contained empty start days, as we found there were very few of them, and accurately imputing these values would be challenging. Lastly, we removed columns we did not feel would be useful, such as economic statistics related to each state.


Next, we began our exploratory data analysis. We first plotted the relationship between power outages and month using a bar chart. We observed that summer months (June, July, and August) on average had a greater number of outages, while the fall months (September, October, and November) appeared to have the fewest number of outages.

<iframe
  src="assets/plot1.html"
  width="1200"
  height="500"
  frameborder="0"
></iframe>

We then used a line plot to represent the relationship between the number of outages and the hour of the day. There was a clear relationship showing that outages were more likely to begin in the late afternoon (2pm - 4pm) and rarely occurred in the late night/early morning.
<iframe
  src="assets/plot2.html"
  width="1200"
  height="500"
  frameborder="0"
></iframe>
Next, we explored the duration of outages. We plotted a bar chart showing the average duration of outages by month. The average outage duration was significantly larger in September and October, which led us to investigate a possible inverse relationship between the number of outages and average duration. After further analysis, we concluded that there was no significant relationship.
<iframe
  src="assets/plot3.html"
  width="1200"
  height="500"
  frameborder="0"
></iframe>
We also explored whether certain climate regions had different distributions of outages with regard to the season. We created a categorical bar chart with climate region on the X-axis and normalized the number of outages on the Y-axis. We noticed that the central regions had a higher percentage of outages occurring in the summer. We also observed trends that aligned with regional weather patterns, such as the northwest having a higher share of winter outages.
<iframe
  src="assets/plot4.html" width="1200" height="500" frameborder="0"></iframe>
Additionally, we examined the average duration of outages in relation to their causes. We found that fuel supply emergencies had by far the largest average duration.
<iframe
  src="assets/plot5.html"
  width="1200"
  height="500"
  frameborder="0"
></iframe>

Lastly, we created a scatter plot showing the relationship between the time of day and outage duration, categorized by cause. This allowed us to identify some cause-specific patterns, such as fuel supply emergencies tending to have longer durations when they began in the late morning or early afternoon.
<iframe
  src="assets/plot6.html"
  width="1200"
  height="500"
  frameborder="0"
></iframe>
## Assessment of Missingness

Most of the columns in our dataset contained no missing values, but one of the columns we noticed had missing values that are likely NMAR (Not Missing At Random) is the customers affected column. This column was collected by aggregating data from a variety of sources, and certain companies may have chosen not to report this number if it reflected badly on them.

Because this analysis is primarily focused on the duration of outages, we decided it was crucial to investigate the missingness of the duration column. We conducted a series of permutation tests comparing the distribution of relevant categorical variables when duration was missing vs. not missing. After a series of tests, we determined that the missingness of duration was most associated with the year and climate category columns. We then imputed the missing duration values using conditional mean imputation. Lastly, we used the newly created durations to also impute the outage restoration date/time column.
<iframe
  src="assets/plot7.html"
  width="1200"
  height="500"
  frameborder="0"
></iframe>
## Hypothesis Testing

We conducted two primary hypothesis tests to explore the effects of the time of year on power outages.

### Test 1: Effect of Time of Year on Length of Outages

- **Null Hypothesis (H0):** The time of year (month) does not affect the length of outages.
- **Alternative Hypothesis (H1):** The time of year (month) does affect the length of outages.
- **Observed Total Variation Distance (TVD):** 4350.77
- **Significance Level:** 0.01

**Results:**
- **P-Value:** 0.0006
- **Conclusion:** There is a high likelihood that the time of year does affect the length of outages.

### Test 2: Effect of Time of Year on Frequency of Outages

- **Null Hypothesis (H0):** The frequency of outages does not depend on the month.
- **Alternative Hypothesis (H1):** The frequency of outages does depend on the month.
- **Observed Total Variation Distance (TVD):** 0.10840

**Results:**
- **P-Value:** 0.0
- **Conclusion:** There is a high likelihood that the time of year (month) affects the frequency of outages.

## Framing a Prediction Problem

We aim to predict whether power outage duration will be greater than or less than 1 hour. For this task, we have chosen a binary classification model. This model could be particularly useful for individuals experiencing power outages who want to know if they are likely to regain power within an hour of the outage starting.

### Model Evaluation

We will use the F1 score to evaluate the model's performance. The F1 score balances precision and recall, making it a suitable metric for our needs as it prioritizes good overall predictions without favoring either recall or precision excessively.

### Features

All features used in the model are those that can be determined at the start of the outage. This includes:
- Geographic information
- Climatic conditions
- Cause of the outage
- Time-specific information

### Model Flexibility

Our model is designed to allow predictions even with incomplete information, ensuring that it can still provide useful insights despite potential gaps in the data.

## Baseline Model

Our baseline model used month, cause category, NERC region, and climate category to predict whether an outage would last under an hour. All these features are ordinal, and therefore we utilized one-hot encoding.

The baseline model tended to overpredict that the outage duration would be greater than an hour. It had low precision and recall scores when predicting durations under an hour. While the model is a good starting point, it has significant shortcomings.




![Alt text](assets/base)




## Final Model

In our final model, we added day_of_week and time_of_day, which we also one-hot encoded. We chose these parameters because our earlier analysis indicated a distinguishable relationship between time of day, day of week, and outage duration. This relationship did not appear to be linear, so we believed that one-hot encoding these features in a classifier would be more effective.

Although the overall accuracy of the final model decreased, we observed a much higher recall and F1-score for predicting outages under an hour. The hyperparameters were optimized by running multiple GridSearches with various ranges. The optimal values were a max depth of 14 and an estimator count of 240.

The original model had higher accuracy and higher precision for predicting longer outages because it predicted longer durations more frequently, aligning with the dataset's distribution that contained more instances of longer outages.

We believe this final model is better because it now correctly predicts outages lasting less than an hour much more frequently.


![Alt text](assets/improved)



### Fairness Test

To ensure fairness, we examined whether outages in cold regions were being predicted fairly. This was important because a lack of power in a cold environment can be detrimental to safety. Our null hypothesis was that prediction accuracy is the same for cold climates and other climates. Our alternative hypothesis was that cold climates are predicted less accurately than other climates. 

Permutation tests revealed no significant difference in prediction accuracy. The p-value was 0.676, indicating that the model does not unfairly disadvantage outages in cold climates.

