## Preliminary 
The objective is to analyze county - level births and be able to gain insights about the data with any prelimary work concerning socio-economic, racial, and health related factors.

Some preliminary questions...... 

**1.**  Calculate the average number of births for each year across all counties:

````sql
SELECT
    a.Year,
    AVG(a.Births) AS average_births
FROM
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality` AS a
INNER JOIN
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality_by_maternal_morbidity` AS b
ON
    a.County_of_Residence_FIPS = b.County_of_Residence_FIPS
GROUP BY
        Year
ORDER BY 
        Year;

````

**Results:**
year |average_births|
----------|--------|
2016   |  6911.056|
2017   |  6752.397|
2018  |    6635.648|



**2.** Identify the year with the highest average number of births:

````sql
SELECT
    a.Year,
    AVG(a.Births) AS average_births
FROM
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality` AS a
INNER JOIN
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality_by_maternal_morbidity` AS b
ON
    a.County_of_Residence_FIPS = b.County_of_Residence_FIPS
GROUP BY
        Year
ORDER BY 
-- we want the highest average number of births by descending order
        average_births DESC
LIMIT 1; -- limit to the first entry
````
**Results:**
year|average_births|
----------|--------|
2016   |  6911.056|


**3.** Identify the year with the lowest average number of births:

```sql
SELECT
    a.Year,
    AVG(a.Births) AS average_births
FROM
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality` AS a
INNER JOIN
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality_by_maternal_morbidity` AS b
ON
    a.County_of_Residence_FIPS = b.County_of_Residence_FIPS
GROUP BY
        Year
ORDER BY 
-- we want the highest average number of births by descending order
        average_births ASC
LIMIT 1; -- limit to the first entry
```

**Results:**
year|average_births|
----------|--------|
2018   |  6635.648|

**4.** Find the county with the highest average age of mothers:

````sql
SELECT
    a.County_of_Residence,
    AVG(a.Ave_Age_of_Mother) AS avg_age_of_mothers
FROM
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality` AS a
INNER JOIN
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality_by_maternal_morbidity` AS b
ON
    a.County_of_Residence_FIPS = b.County_of_Residence_FIPS
GROUP BY
        County_of_Residence
ORDER BY 
-- we want to order by average age of mothers
        avg_age_of_mothers DESC
````

**Results:**

County_of_Residence|avg_age_of_mothers|
----------|--------|
Marin County,CA   |  33.16|

**5.** Find the county with the lowest average pre-pregnancy BMI:

````sql
SELECT
    a.County_of_Residence,
    AVG(a.Ave_Pre_pregnancy_BMI) AS avg_pre_pregnancy_BMI
FROM
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality` AS a
INNER JOIN
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality_by_maternal_morbidity` AS b
ON
    a.County_of_Residence_FIPS = b.County_of_Residence_FIPS
GROUP BY
        County_of_Residence
ORDER BY 
-- we want the lowest average pre-pregrancy BMI by ascending order
        avg_pre_pregnancy_BMI ASC
LIMIT 1; -- limit to the first entry
````

**Results:**

County_of_Residence|avg_pre_pregrancy_BMI|
----------|--------|
New York County,NY   |  23.89|

**6.** Compare the counties with the highest and lowest values in terms of maternal characteristics:

````sql
ELECT
	to_char(t1.reported_crime_date, 'Month') AS month,
	COUNT(*) AS n_crimes,
	round(avg(t2.temp_high), 1) avg_high_temp,
	PERCENTILE_CONT(0.5) WITHIN GROUP(ORDER BY t2.temp_high) AS median_high_temp
FROM
	chicago.crimes AS t1
JOIN 
	chicago.weather AS t2
ON 
	t1.reported_crime_date = t2.weather_date
GROUP BY
	month
ORDER BY
	n_crimes DESC;
````

**Results:**

month    |n_crimes|avg_high_temp|median_high_temp|
---------|--------|-------------|----------------|
July     |  111328|         85.2|            86.0|
August   |  110659|         84.3|            85.0|
October  |  105563|         62.5|            62.0|
June     |  105163|         81.5|            81.0|
September|  105075|         77.2|            78.0|
May      |  103985|         71.8|            72.0|
December |   96505|         40.6|            41.0|
November |   95501|         47.6|            47.0|
March    |   92947|         48.0|            47.0|
January  |   92018|         32.3|            34.0|
April    |   88707|         56.7|            55.0|
February |   82329|         35.3|            35.0|




To be continued....
