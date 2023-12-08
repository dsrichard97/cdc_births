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
LIMIT 5; -- limit to 5 entry



-- lowest average pre-pregrnancy BMI by county of residence
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
LIMIT 5; -- limit to 5 entry
````

**Results:**
County_of_Residence|avg_age_of_mothers|
----------|--------|
Marin County,CA   |  33.16|
San Francisco County,CA   |  33.08|
Arlignton County,VA   |  32.53|
New York County,NY  |  32.36|
Norfolk County,MA   |  32.28|

**Results:**
County_of_Residence|avg_pre_pregrancy_BMI|
----------|--------|
New York County,NY   |  23.89|
San Francisco County,CA   |  24.06|
Arlignton County,VA   |  24.36|
Williamson County,TN   |  24.68|
Marin County,CA   |  24.69|


