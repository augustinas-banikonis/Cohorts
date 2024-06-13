# Weekly Subscription Churn Analysis Using Cohorts

## Situation

Subscription-based businesses often face challenges in retaining customers over time. Understanding the retention rates of subscribers who join in different weeks (cohorts) can provide valuable insights into user behavior and help in devising effective retention strategies.

## Goal of the Analysis

The goal of this analysis is to track the retention rates of subscribers across weekly cohorts. By analyzing how many subscribers continue their subscriptions from the initial week (week 0) to subsequent weeks (up to week 6), we aim to identify patterns in subscriber retention over time.

## SQL Query

```sql
SELECT 
  DATE_TRUNC(subscription_start, week) AS Cohort_date,
  COUNT(DISTINCT user_pseudo_id) AS cohort_size,
  COUNT(DISTINCT CASE WHEN DATE_ADD(DATE(subscription_start), INTERVAL 1 WEEK) < COALESCE(subscription_end, DATE('2021-02-07')) THEN user_pseudo_id END) AS we1,
  COUNT(DISTINCT CASE WHEN DATE_ADD(DATE(subscription_start), INTERVAL 2 WEEK) < COALESCE(subscription_end, DATE('2021-02-07')) THEN user_pseudo_id END) AS we2,
  COUNT(DISTINCT CASE WHEN DATE_ADD(DATE(subscription_start), INTERVAL 3 WEEK) < COALESCE(subscription_end, DATE('2021-02-07')) THEN user_pseudo_id END) AS we3,
  COUNT(DISTINCT CASE WHEN DATE_ADD(DATE(subscription_start), INTERVAL 4 WEEK) < COALESCE(subscription_end, DATE('2021-02-07')) THEN user_pseudo_id END) AS we4,
  COUNT(DISTINCT CASE WHEN DATE_ADD(DATE(subscription_start), INTERVAL 5 WEEK) < COALESCE(subscription_end, DATE('2021-02-07')) THEN user_pseudo_id END) AS we5,
  COUNT(DISTINCT CASE WHEN DATE_ADD(DATE(subscription_start), INTERVAL 6 WEEK) < COALESCE(subscription_end, DATE('2021-02-07')) THEN user_pseudo_id END) AS we6
FROM `tc-da-1.turing_data_analytics.subscriptions`
GROUP BY 1;
```
## Results

### Cohort Retention Rates
```
| Cohort Date | Cohort Size | Week 1 Retention | Week 2 Retention | Week 3 Retention | Week 4 Retention | Week 5 Retention | Week 6 Retention |
|-------------|-------------|------------------|------------------|------------------|------------------|------------------|------------------|
| 2020-11-01  | 20,078      | 18,312           | 17,855           | 17,530           | 17,302           | 17,096           | 16,952           |
| 2020-11-08  | 16,244      | 14,706           | 14,344           | 14,113           | 13,923           | 13,781           | 13,709           |
| 2020-11-15  | 17,924      | 16,339           | 15,928           | 15,675           | 15,481           | 15,384           | 15,349           |
| 2020-11-22  | 19,911      | 18,212           | 17,814           | 17,517           | 17,367           | 17,322           | 17,281           |
| 2020-11-29  | 22,278      | 20,369           | 19,918           | 19,709           | 19,647           | 19,590           | 19,532           |
| 2020-12-06  | 28,490      | 26,443           | 26,094           | 26,002           | 25,910           | 25,803           | 25,749           |
| 2020-12-13  | 25,533      | 23,893           | 23,766           | 23,673           | 23,561           | 23,490           | 23,421           |
| 2020-12-20  | 18,101      | 17,294           | 17,168           | 17,080           | 17,001           | 16,951           | 16,931           |
| 2020-12-27  | 17,059      | 16,229           | 16,032           | 15,923           | 15,812           | 15,770           | 0                |
| 2021-01-03  | 23,290      | 21,883           | 21,568           | 21,387           | 21,303           | 0                | 0                |
| 2021-01-10  | 21,793      | 20,416           | 20,092           | 19,975           | 0                | 0                | 0                |
| 2021-01-17  | 21,052      | 19,484           | 19,270           | 0                | 0                | 0                | 0                |
| 2021-01-24  | 19,977      | 18,762           | 0                | 0                | 0                | 0                | 0                |
| 2021-01-31  | 2,255       | 0                | 0                | 0                | 0                | 0                | 0                |
```

### Cohort Retention Rates as Percentages (Normalized to 100)

```
| Cohort Date | Week 1 Retention (%) | Week 2 Retention (%) | Week 3 Retention (%) | Week 4 Retention (%) | Week 5 Retention (%) | Week 6 Retention (%) |
|-------------|----------------------|----------------------|----------------------|----------------------|----------------------|----------------------|
| 2020-11-01  | 91.3                 | 88.9                 | 87.2                 | 86.2                 | 84.9                 | 84.3                 |
| 2020-11-08  | 90.8                 | 88.2                 | 87.0                 | 85.9                 | 84.8                 | 84.5                 |
| 2020-11-15  | 91.1                 | 88.9                 | 87.4                 | 86.2                 | 85.6                 | 85.5                 |
| 2020-11-22  | 91.5                 | 89.5                 | 87.9                 | 87.1                 | 86.9                 | 86.7                 |
| 2020-11-29  | 91.4                 | 89.7                 | 88.6                 | 88.2                 | 87.9                 | 87.7                 |
| 2020-12-06  | 92.8                 | 91.4                 | 91.2                 | 90.9                 | 90.7                 | 90.5                 |
| 2020-12-13  | 93.6                 | 93.2                 | 92.9                 | 92.5                 | 92.2                 | 91.7                 |
| 2020-12-20  | 95.3                 | 95.0                 | 94.5                 | 93.9                 | 93.6                 | 93.5                 |
| 2020-12-27  | 95.4                 | 94.1                 | 93.6                 | 93.0                 | 92.8                 | 0                    |
| 2021-01-03  | 93.9                 | 92.7                 | 91.8                 | 91.5                 | 0                    | 0                    |
| 2021-01-10  | 93.7                 | 92.3                 | 91.6                 | 0                    | 0                    | 0                    |
| 2021-01-17  | 92.7                 | 91.5                 | 0                    | 0                    | 0                    | 0                    |
| 2021-01-24  | 93.9                 | 0                    | 0                    | 0                    | 0                    | 0                    |
| 2021-01-31  | 0                    | 0                    | 0                    | 0                    | 0                    | 0                    |
``` 

## Key Observations

- **High Initial Retention**: The retention rates in the first few weeks (week 0 to week 3) are relatively high across most cohorts, indicating strong initial engagement with the subscription service.
  
- **Retention Decline Over Time**: There is a noticeable decline in retention rates as the weeks progress from week 3 onwards. This highlights the challenge of maintaining subscriber interest and engagement over an extended period.

- **Variability in Cohort Sizes**: Cohort sizes vary significantly from week to week, which impacts overall retention percentages. Smaller cohorts may experience more volatile retention rates compared to larger ones.

### Insights and Recommendations

- **Early Engagement Strategies**: Focus on optimizing engagement strategies during the initial weeks after subscription to maximize long-term retention. Implementing personalized onboarding experiences and targeted communications can help in establishing a strong connection with new subscribers.

- **Continuous Monitoring**: Given the variability in cohort sizes and retention rates, it's crucial to continuously monitor retention metrics over time. This allows for timely adjustments in marketing and retention strategies based on real-time data insights.

