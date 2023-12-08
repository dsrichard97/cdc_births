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

year|averaged_births|
----------|--------|
2016   |  6911.056|


**3.** Identify the year with the lowest average number of births:

```sql
WITH get_top_crime AS (
	SELECT 
		initcap(crime_type) AS crime_type,
		count(*) AS n_crimes
	FROM 
		chicago.crimes
	GROUP BY 
		crime_type
	ORDER BY 
		n_crimes DESC
)
SELECT
	crime_type,
	n_crimes,
	round(100 * n_crimes::NUMERIC / sum(n_crimes) OVER (), 2) AS total_percentage
FROM
	get_top_crime
LIMIT 3;
```

**Results:**

crime_type     |n_crimes|total_percentage|
---------------|--------|----------------|
Theft          |  264701|           22.25|
Battery        |  222214|           18.68|
Criminal Damage|  131716|           11.07|

**4.** Find the county with the highest average age of mothers:

````sql
SELECT 
	initcap(t2.community_name) AS community,
	t2.population,
	t2.density,
	count(*) AS reported_crimes
FROM 
	chicago.crimes AS t1
JOIN
	chicago.community AS t2
ON 
	t2.community_id = t1.community_id
GROUP BY 
	t2.community_name,
	t2.population,
	t2.density
ORDER BY 
	reported_crimes DESC
LIMIT 10;
````

**Results:**

community      |population|density |reported_crimes|
---------------|----------|--------|---------------|
Austin         |     96557|13504.48|          66662|
Near North Side|    105481|38496.72|          51977|
Near West Side |     67881|11929.88|          41773|
South Shore    |     53971|18420.14|          40984|
Loop           |     42298|25635.15|          40245|
North Lawndale |     34794|10839.25|          39115|
Humboldt Park  |     54165|15045.83|          34992|
Auburn Gresham |     44878|11903.98|          33680|
West Town      |     87781|19166.16|          32812|
Roseland       |     38816| 8053.11|          30836|

**5.** Find the county with the lowest average pre-pregnancy BMI:

````sql
SELECT 
	initcap(t2.community_name) AS community,
	t2.population,
	t2.density,
	count(*) AS reported_crimes
FROM 
	chicago.crimes AS t1
JOIN
	chicago.community AS t2
ON 
	t2.community_id = t1.community_id
GROUP BY 
	t2.community_name,
	t2.population,
	t2.density
ORDER BY 
	reported_crimes
LIMIT 10;
````

**Results:**

community      |population|density |reported_crimes|
---------------|----------|--------|---------------|
Edison Park    |     11525|10199.12|           1336|
Burnside       |      2527| 4142.62|           1787|
Forest Glen    |     19596| 6123.75|           2601|
Mount Greenwood|     18628|  6873.8|           2609|
Hegewisch      |     10027| 1913.55|           2861|
Montclare      |     14401|14546.46|           2905|
Oakland        |      6799|11722.41|           3289|
Fuller Park    |      2567| 3615.49|           3616|
Archer Heights |     14196| 7062.69|           4011|
Mckinley Park  |     15923|11292.91|           4081|

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
