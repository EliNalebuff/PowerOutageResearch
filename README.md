
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
  - OUTAGE.START.DATE/TIME
  - OUTAGE.RESTORATION.DATE/TIME
  - CAUSE.CATEGORY
 
##  Data Cleaning and Exploratory Data Analysis

The first precaution we took with the data was to ensure all numeric columns were represented as either integers or floats. We also combined the date and time columns into a consolidated `pd.Datetime` column. Additionally, we removed rows that contained empty start days, as we found there were very few of them, and accurately imputing these values would be challenging. Lastly, we removed columns we did not feel would be useful, such as economic statistics related to each state.


Next, we began our exploratory data analysis. We first plotted the relationship between power outages and month using a bar chart. We observed that summer months (June, July, and August) on average had a greater number of outages, while the fall months (September, October, and November) appeared to have the fewest number of outages.

<iframe
  src="assets/plot1.html"
  width="1200"
  height="800"
  frameborder="0"
></iframe>

We then used a line plot to represent the relationship between the number of outages and the hour of the day. There was a clear relationship showing that outages were more likely to begin in the late afternoon (2pm - 4pm) and rarely occurred in the late night/early morning.
<iframe
  src="assets/plot2.html"
  width="1200"
  height="800"
  frameborder="0"
></iframe>
Next, we explored the duration of outages. We plotted a bar chart showing the average duration of outages by month. The average outage duration was significantly larger in September and October, which led us to investigate a possible inverse relationship between the number of outages and average duration. After further analysis, we concluded that there was no significant relationship.
<iframe
  src="assets/plot3.html"
  width="1200"
  height="800"
  frameborder="0"
></iframe>
We also explored whether certain climate regions had different distributions of outages with regard to the season. We created a categorical bar chart with climate region on the X-axis and normalized the number of outages on the Y-axis. We noticed that the central regions had a higher percentage of outages occurring in the summer. We also observed trends that aligned with regional weather patterns, such as the northwest having a higher share of winter outages.
<iframe
  src="assets/plot4.html"
  width="1200"
  height="800"
  frameborder="0"
></iframe>

Additionally, we examined the average duration of outages in relation to their causes. We found that fuel supply emergencies had by far the largest average duration.
<iframe
  src="assets/plot5.html"
  width="1200"
  height="800"
  frameborder="0"
></iframe>

Lastly, we created a scatter plot showing the relationship between the time of day and outage duration, categorized by cause. This allowed us to identify some cause-specific patterns, such as fuel supply emergencies tending to have longer durations when they began in the late morning or early afternoon.
<iframe
  src="assets/plot6.html"
  width="1200"
  height="800"
  frameborder="0"
></iframe>
