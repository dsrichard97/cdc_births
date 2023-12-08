## Preliminary 
The objective is to analyze county - level births and be able to gain insights about the data with any prelimary work concerning socio-economic, racial, and health related factors.

Some preliminary questions...... 

**1.**  How do birth rates vary across different counties?

````sql
SELECT
    AVE_AGE_OF_MOTHER, AVE_NUMBER_OF_PRENATAL_WKS, COUNT(*) AS Morbidity_Count
FROM
    `bigquery-public-data.sdoh_cdc_wonder_natality.county_natality_by_maternal_morbidity`
GROUP BY
    AVE_AGE_OF_MOTHER, AVE_NUMBER_OF_PRENATAL_WKS
LIMIT 1000;

````

**Results:**

Total Reported Crimes|
---------------------|
1,189,780           |

**2.** What is the average age of mothers at the time of birth in each county?

````sql
SELECT 
	initcap(crime_type) AS crime_type,
	count(*) AS n_crimes
FROM 
	chicago.crimes
WHERE 
	crime_type IN ('homicide', 'battery', 'assault')
GROUP BY 
	crime_type
ORDER BY 
	n_crimes DESC;
````

**Results:**

crime_type|n_crimes|
----------|--------|
Battery   |  222214|
Assault   |  100411|
Homicide  |    3440|

**3.** Which counties report the highest incidence of births with abnormal conditions?

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

**4.** Are there significant differences in birth statistics (like average birth weight, gestational age) across different father or mother races?

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

**5.** Does the source of payment correlate with differences in birth outcomes like average birth weight or gestational age?

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

**6.** What month had the most crimes reported and what was the average and median temperature high in the last five years?

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

**7.** What month had the most homicides reported and what was the average and median temperature high in the last five years?

````sql
SELECT
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
WHERE
	t1.crime_type = 'homicide'
GROUP BY
	month
ORDER BY
	n_crimes DESC;
````

**Results:**

month    |n_crimes|avg_high_temp|median_high_temp|
---------|--------|-------------|----------------|
July     |     398|         85.3|            86.0|
June     |     360|         82.5|            82.0|
September|     356|         78.1|            79.0|
August   |     330|         84.6|            85.0|
May      |     326|         72.7|            73.0|
October  |     296|         63.8|            64.0|
April    |     275|         59.3|            58.0|
November |     263|         49.8|            48.0|
December |     249|         41.4|            42.0|
January  |     212|         32.9|            33.0|
February |     190|         35.0|            37.0|
March    |     185|         49.3|            48.0|

**8.** List the most violent year and the number of arrests with percentage.  Order by the number of crimes in decending order.  Determine the most violent year by the number of reported Homicides, Assaults and Battery for that year.

````sql
WITH get_arrest_percentage AS (
	SELECT
		EXTRACT('year' FROM t1.reported_crime_date) AS most_violent_year,
		count(*) AS reported_violent_crimes,
		sum(
			CASE
				WHEN arrest = TRUE THEN 1
				ELSE 0
			END 
		) AS number_of_arrests
	FROM
		chicago.crimes AS t1
	WHERE 
		crime_type IN ('homicide', 'battery', 'assault')
	GROUP BY
		most_violent_year
	ORDER BY
		reported_violent_crimes DESC
)
SELECT
	most_violent_year,
	reported_violent_crimes,
	number_of_arrests || ' (' || round(100 * number_of_arrests::NUMERIC / reported_violent_crimes, 2) || '%)' AS number_of_arrests
FROM
	get_arrest_percentage;
````

**Results:**

most_violent_year|reported_violent_crimes|number_of_arrests|
-----------------|-----------------------|-----------------|
2018|                  70835|13907 (19.63%)   |
2019|                  70645|14334 (20.29%)   |
2022|                  62412|8165 (13.08%)    |
2021|                  61611|7855 (12.75%)    |
2020|                  60562|9577 (15.81%)    |



To be continued....
