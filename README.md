# BigQuery-Analysis
This repository contains daily time series datasets related to COVID-19 in different countries. It includes demographic, economic, epidemiological, geographic, health, hospitalisation, mobility, government action and climate data.
Identify responses to COVID-19 pandemic-related queries. Getting the right answers will help the organisation to properly plan and target awareness programmes and health care initiatives.

## Case 1

U.S states with the most confrimed cases per month

```
select subregion1_name as State, count(cumulative_deceased) as total_confirmed_cases
from bigquery-public-data.covid19_open_data.covid19_open_data
where country_name = "United States of America"
group by subregion1_name
order by total_confirmed_cases desc;
```
