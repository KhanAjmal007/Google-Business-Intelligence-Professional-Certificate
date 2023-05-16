# Google Fiber
## Scenario
The Google Fiber customer service team’s goal is to understand how often customers are calling customer support after their first inquiry; this will help leadership understand how effectively the team is able to answer customer questions the first time. The dashboard you create should demonstrate an understanding of this goal and provide your stakeholders with insights about repeat caller volumes in different markets and the types of problems they represent. As part of the interview process, they have asked you to create a dashboard that will: 

<ul>
 <li> Help them understand how often customers are calling customer support after their first inquiry; this will help leadership understand how effectively the team is able to answer customer questions the first time </li>
 <li> Provide insights into the types of customer issues that seem to generate more repeat calls </li>
 <li> Explore repeat caller trends in the three different market cities </li>
 <li> Design charts so that stakeholders can view trends by week, month, quarter, and year. </li>
</ul>
 
You met with stakeholders to complete project planning documents and uploaded the necessary tables into your BigQuery project space. 

## Datasets
[Market 1](https://docs.google.com/spreadsheets/d/1a9IKjkvOvYHRx84SyRdp4Sq81EzgeOZPufcRtrUcAIc/template/preview#gid=775366698)

[Market 2](https://docs.google.com/spreadsheets/d/19CINdvAwp-2RF5pphkLywZLQJyJu66EOjX6CgrW32nA/template/preview#gid=2065220237)

[Market 3](https://docs.google.com/spreadsheets/d/1K6X9ZhjWtbneBss7PQH7IobGCzQ5NzG1hxs1D-hbsZM/template/preview?resourcekey=0-q90E-1XwD8nkNSjs0Ws3-w)

## Resources
 [BigQuery](https://console.cloud.google.com/bigquery?_ga=2.139941043.1230943722.1684190616-1879709139.1684190616&project=noted-processor-386822&ws=!1m0/)
 
 [Dataflow](/https://console.cloud.google.com/dataflow/jobs?project=noted-processor-386822/)

### Example Query
```
SELECT*
FROM `your project table location for market_1`
UNION ALL
SELECT*
FROM `your project table location for market_2`
```
*you could run the following SQL query to create a single combined table that merged all three of the datasets your were given
```
SELECT
  date_created,
  contacts_n,
  contacts_n_1,
  contacts_n_2,
  contacts_n_3,
  contacts_n_4,
  contacts_n_5,
  contacts_n_6,
  contacts_n_7,
  new_type,
  new_market
FROM `your project.fiber.market_1`
UNION ALL
SELECT
  date_created,
  contacts_n,
  contacts_n_1,
  contacts_n_2,
  contacts_n_3,
  contacts_n_4,
  contacts_n_5,
  contacts_n_6,
  contacts_n_7,
  new_type,
  new_market
FROM `your project.fiber.market_2`
UNION ALL
SELECT
  date_created,
  contacts_n,
  contacts_n_1,
  contacts_n_2,
  contacts_n_3,
  contacts_n_4,
  contacts_n_5,
  contacts_n_6,
  contacts_n_7,
  new_type,
  new_market
FROM `your project.market_3`
```

### My Query
```
SELECT *
FROM `noted-processor-386822.fiber.market1`
UNION ALL
SELECT *
FROM `noted-processor-386822.fiber.market2`
UNION ALL
SELECT *
FROM `noted-processor-386822.fiber.market3`;
```
