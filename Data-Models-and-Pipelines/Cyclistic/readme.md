# Cyclystic: Create a Target Table
## Scenario
The product development team at Cyclistic has begun developing their business plan for next year. In order to build a better Cyclistic, the team needs to understand how customers are currently using the bikes, how location and other factors impact demand, and what stations get the most traffic. The Cyclistic team has a few goals:

<ul>
  <li> Understand current customers needs, what makes a successful product, and how new stations might alleviate demand in different geographical areas </li>
  <li> Understand current usage of bikes at different locations </li>
  <li> Apply customer usage insights to inform new station growth </li>
  <li> Understand how different users (subscribers and non-subscribers) use the bikes </li>
</ul>

You met with stakeholders to complete project planning documents and uploaded the necessary tables into your BigQuery project space. 

## Instructions

<ol type="I">
  <li> Access either [BigQuery](https://console.cloud.google.com/bigquery?_ga=2.139941043.1230943722.1684190616-1879709139.1684190616&project=noted-processor-386822&ws=!1m0/) or [Dataflow](/https://console.cloud.google.com/dataflow/jobs?project=noted-processor-386822/), two google cloud platforms that can execute SQL code; however, both require you to have a Google Cloud account. </li>
   [BigQuery](https://console.cloud.google.com/bigquery?_ga=2.139941043.1230943722.1684190616-1879709139.1684190616&project=noted-processor-386822&ws=!1m0/)
   [Dataflow](/https://console.cloud.google.com/dataflow/jobs?project=noted-processor-386822/)
  <li> Query the data. For this step, consider all the key metrics the stakeholders have identified, address their business questions, and collect the data you'll need to develop the final dashboard. For the final dashboard, you will create two target tables: a table to capture the entire year and a table to focus on summer trends. </li>
</ol>

### Example Query
```
SELECT
  TRI.usertype,
  ZIPSTART.zip_code AS zip_code_start,
  ZIPSTARTNAME.borough borough_start,
  ZIPSTARTNAME.neighborhood AS neighborhood_start,
  ZIPEND.zip_code AS zip_code_end,
  ZIPENDNAME.borough borough_end,
  ZIPENDNAME.neighborhood AS neighborhood_end,
  -- Since this is a fictional dashboard, you can add 5 years to make it look recent
  DATE_ADD(DATE(TRI.starttime), INTERVAL 5 YEAR) AS start_day,
  DATE_ADD(DATE(TRI.stoptime), INTERVAL 5 YEAR) AS stop_day,
  WEA.temp AS day_mean_temperature, -- Mean temp
  WEA.wdsp AS day_mean_wind_speed, -- Mean wind speed
  WEA.prcp day_total_precipitation, -- Total precipitation
  -- Group trips into 10 minute intervals to reduces the number of rows
  ROUND(CAST(TRI.tripduration / 60 AS INT64), -1) AS trip_minutes,
  COUNT(TRI.bikeid) AS trip_count
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips` AS TRI
INNER JOIN
  `bigquery-public-data.geo_us_boundaries.zip_codes` ZIPSTART
  ON ST_WITHIN(
    ST_GEOGPOINT(TRI.start_station_longitude, TRI.start_station_latitude),
    ZIPSTART.zip_code_geom)
INNER JOIN
  `bigquery-public-data.geo_us_boundaries.zip_codes` ZIPEND
  ON ST_WITHIN(
    ST_GEOGPOINT(TRI.end_station_longitude, TRI.end_station_latitude),
    ZIPEND.zip_code_geom)
INNER JOIN
  `bigquery-public-data.noaa_gsod.gsod20*` AS WEA
  ON PARSE_DATE("%Y%m%d", CONCAT(WEA.year, WEA.mo, WEA.da)) = DATE(TRI.starttime)
INNER JOIN
  -- Note! Add your zip code table name, enclosed in backticks: `example_table`
  `(insert your table name) zipcodes` AS ZIPSTARTNAME
  ON ZIPSTART.zip_code = CAST(ZIPSTARTNAME.zip AS STRING)
INNER JOIN
  -- Note! Add your zipcode table name, enclosed in backticks: `example_table`
  `(insert your table name) zipcodes` AS ZIPENDNAME
  ON ZIPEND.zip_code = CAST(ZIPENDNAME.zip AS STRING)
WHERE
  -- This takes the weather data from one weather station
  WEA.wban = '94728' -- NEW YORK CENTRAL PARK
  -- Use data from 2014 and 2015
  AND EXTRACT(YEAR FROM DATE(TRI.starttime)) BETWEEN 2014 AND 2015
GROUP BY
  1,
  2,
  3,
  4,
  5,
  6,
  7,
  8,
  9,
  10,
  11,
  12,
  13
```
