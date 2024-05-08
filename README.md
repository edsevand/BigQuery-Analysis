# BigQuery-Analysis
This repository contains daily time series datasets related to COVID-19 in different countries. It includes demographic, economic, epidemiological, geographic, health, hospitalisation, mobility, government action and climate data.
Identify responses to COVID-19 pandemic-related queries. Getting the right answers will help the organisation to properly plan and target awareness programmes and health care initiatives.

### Case 1

U.S states with the most confrimed cases per month

```
select
  subregion1_name as State,
  count(cumulative_deceased) as total_confirmed_cases
from
  bigquery-public-data.covid19_open_data.covid19_open_data
where
  country_name = "United States of America"
group by
  subregion1_name
order by
  total_confirmed_cases desc;
```

### Case 2 

Fatality rate in Italy by mnth 2020

```
select
  extract(month from `date`) as month,
  sum(cumulative_deceased) as total_confirmed_cases,
  sum(cumulative_confirmed) as total_deaths,
  avg(cumulative_deceased/cumulative_confirmed) * 100 case_fatality_ratio
from
  bigquery-public-data.covid19_open_data.covid19_open_data
where
  country_name = "Italy" and
  cumulative_confirmed is not null and
  cumulative_confirmed > 0 and
  extract(year from `date`) = 2020
group by
  month
order by
  month asc;
```

### Case 3

Identifies days with a zero rate in the net new caseload

```
WITH india_cases_by_date AS (
  SELECT
    date,
    SUM(cumulative_confirmed) AS cases
  FROM
    `bigquery-public-data.covid19_open_data.covid19_open_data`
  WHERE
    country_name="India"
    AND date between '2020-01-01' and '2022-09-17'
  GROUP BY
    date
  ORDER BY
    date ASC)

#india_previous_day_comparison AS
SELECT
  date,
  cases,
  LAG(cases) OVER(ORDER BY date) AS previous_day,
  cases - LAG(cases) OVER(ORDER BY date) AS net_new_cases
FROM india_cases_by_date
ORDER BY
  net_new_cases DESC
LIMIT 10
```


