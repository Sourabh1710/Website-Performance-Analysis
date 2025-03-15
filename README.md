# Website Performance Analysis

## Introduction

I am analyzing website performance using a dataset inspired by a problem found at Statso. This analysis involves examining key metrics such as user engagement, session trends, and marketing channel performance, along with forecasting future website traffic using time series modeling.

## Dataset Description

The [dataset](https://statso.io/website-performance-case-study/) contains the following columns:

- **Session primary channel group**: The marketing channel (e.g., Direct, Organic Social).
- **Date + hour (YYYYMMDDHH)**: The specific date and hour of the session.
- **Users**: Number of users in a given period.
- **Sessions**: Number of sessions in that period.
- **Engaged sessions**: Number of sessions with significant user engagement.
- **Average engagement time per session**: The average time a user is engaged per session.
- **Engaged sessions per user**: Ratio of engaged sessions to total sessions per user.
- **Events per session**: Average number of events (actions taken) per session.
- **Engagement rate**: The proportion of sessions that were engaged.
- **Event count**: Total number of events during the period.

## Data Preparation

I started by importing the necessary libraries and loading the dataset:

```python
import pandas as pd

data = pd.read_csv("data-export.csv")
print(data.head())
```

The dataset contained an issue where the first row was incorrectly treated as data instead of headers. I fixed this issue by:

```python
new_header = data.iloc[0]  # Grab the first row for the header
data = data[1:]  # Take the data less the header row
data.columns = new_header  # Set the header row as the df header
data.reset_index(drop=True, inplace=True)
print(data.head())
```

## Data Inspection

To understand the structure and summary statistics of the dataset:

```python
data.info()
print(data.describe())
```

## Data Type Conversion & Grouping

I converted the date column into an appropriate datetime format and grouped the data for further analysis:

```python
data['Date + hour (YYYYMMDDHH)'] = pd.to_datetime(data['Date + hour (YYYYMMDDHH)'], format='%Y%m%d%H')
data['Users'] = pd.to_numeric(data['Users'])
data['Sessions'] = pd.to_numeric(data['Sessions'])

# Group data by date and sum up the users and sessions
grouped_data = data.groupby(data['Date + hour (YYYYMMDDHH)']).agg({'Users': 'sum', 'Sessions': 'sum'})
```

This step prepared the dataset for time series analysis, helping in visualizing trends and applying forecasting models.

## Website Traffic Analysis

I plotted the total users and sessions over time to identify trends and patterns.

![Total Users and Sessions Over Time](https://github.com/Sourabh1710/Website-Performance-Analysis/blob/main/images/Total%20Users%20and%20Sessions%20Over%20Time.png)

Observations:
- Fluctuations in the number of users and sessions suggest daily cycles or specific high-traffic periods.
- Peaks may correspond to specific marketing activities, promotions, or events.

## User Engagement Analysis

Next, I analyzed key engagement metrics:

![User Engagement](https://github.com/Sourabh1710/Website-Performance-Analysis/blob/main/images/engagement%20time%20per%20session.png)

Observations:
- **Average Engagement Time per Session**: Shows fluctuations, with peaks indicating high engagement due to specific content or events.
- **Engaged Sessions per User**: Indicates the proportion of engaged sessions per user, with fluctuations suggesting variations in content effectiveness.
- **Events per Session**: Remains relatively consistent but varies based on interactive content.
- **Engagement Rate**: Shows ups and downs, reflecting how different content resonates with users.

## Correlation Analysis

I analyzed the relationships between different engagement metrics using scatter plots.

![Correlation](https://github.com/Sourabh1710/Website-Performance-Analysis/blob/main/images/comparisons.png)

Key Insights:
- **Average Engagement Time vs Events per Session**: As engagement time increases, events per session tend to cluster at lower values.
- **Average Engagement Time vs Engagement Rate**: Higher engagement time correlates with a higher engagement rate.
- **Engaged Sessions per User vs Events per Session**: Most data points cluster at lower values.
- **Engaged Sessions per User vs Engagement Rate**: Strong positive correlation.

## Channel Performance Analysis

I evaluated how different marketing channels contribute to traffic and engagement.

![Channel Performance](https://github.com/Sourabh1710/Website-Performance-Analysis/blob/main/images/Users%20and%20Sessions%20by%20Channel.png)

Findings:
- **Organic Search**: Drives the most traffic but has lower engagement metrics.
- **Referral & Organic Video**: While not the highest in traffic, these channels have high engagement, making them valuable for quality interactions.

## Website Traffic Forecasting

I used a SARIMA model to forecast website traffic for the next 24 hours.

To determine model parameters, I plotted the Autocorrelation (ACF) and Partial Autocorrelation (PACF) functions.

![Autocorrelation and Partial Correlation](https://github.com/Sourabh1710/Website-Performance-Analysis/blob/main/images/Autocorrelation%20and%20Partial%20Correlation.png)

Interpretation:
- **PACF**: Suggests an AR order of **p=1**.
- **ACF**: Suggests an MA order of **q=1**.
- **Differencing (d)**: Since seasonality exists, **d=1** is used.

I then built the SARIMA model for forecasting future traffic.

![Website Traffic Forecasting](https://github.com/Sourabh1710/Website-Performance-Analysis/blob/main/images/comparisons.png)

## Summary

In this project, I conducted a comprehensive analysis of website performance, covering:

- **Session Analysis**: Understanding traffic trends.
- **User Engagement Analysis**: Evaluating user interactions.
- **Channel Performance Analysis**: Assessing marketing channel effectiveness.
- **Website Traffic Forecasting**: Predicting future trends using time series models.

## Author
Sourabh Sonker <br>
Data Scientist
