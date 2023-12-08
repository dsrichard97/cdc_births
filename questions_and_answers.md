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



## Summary
Average Number of Births Per Year Across All Counties:
The results indicated a decreasing trend in average births from 2016 to 2018.


Year with the Highest Average Number of Births:
This query identifies 2016 as the year with the highest average number of births. This insight can be important for understanding demographic trends or evaluating the impact of health policies or to further research economic factors during this period.

Year with the Lowest Average Number of Births:
The year 2018 is identified as having the lowest average number of births. Investigating the factors contributing to this decrease could be a critical area of analysis.


County with the Highest Average Age of Mothers:
Marin County, CA, has the highest average age of mothers. This might suggest demographic trends like later pregnancies in life, which could be associated with socio-economic factors. Further research is needed.

County with the Lowest Average Pre-Pregnancy BMI:
New York County, NY, shows the lowest average pre-pregnancy BMI. This could be indicative of health and lifestyle patterns within this population.


Comparison of Counties in Terms of Maternal Characteristics:
Comparing counties with the highest and lowest values in maternal characteristics (like average age of mothers and average pre-pregnancy BMI) can reveal significant disparities. This comparison shows a trend where urban counties like New York County, NY, and San Francisco County, CA, have lower pre-pregnancy BMI and higher maternal ages. Perhaps a socio-economic factors? Worth investigating trends or other information that can further develop these initial comparisions.


