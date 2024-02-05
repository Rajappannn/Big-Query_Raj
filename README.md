1. Find the total number of bank branches:
```SQL
SELECT
  COUNT(*) AS total_branches
FROM
  `bigquery-public-data.fdic_banks.locations`;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/84237ed7-4c1e-445f-bb19-943d56a70568)

2.List the top 10 states with the most bank branches:
```SQL
SELECT
  state,
  COUNT(*) AS num_branches
FROM
  `bigquery-public-data.fdic_banks.locations`
GROUP BY
  state
ORDER BY
  num_branches DESC
LIMIT 10;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/ec24a4d6-e614-4be3-81a1-625d43aec735)

3. Find the most frequent crime types:
```SQL
SELECT
primary_type,
COUNT(*) AS crime_count
FROM `bigquery-public-data.austin_crime.crime`
GROUP BY primary_type
ORDER BY crime_count DESC
LIMIT 10;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/f2803be8-67dc-412a-aecd-1bccf7407cde)

4. Identify the days of the week with the highest crime rates:
```SQL
SELECT
CAST(EXTRACT(DAYOFWEEK FROM clearance_date) AS STRING) AS day_of_week,
COUNT(*) AS crime_count
FROM `bigquery-public-data.austin_crime.crime`
GROUP BY day_of_week
ORDER BY crime_count DESC;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/5830904c-7495-4c40-b361-dc0628167059)

5. Compare crime rates across different city districts:
```SQL
SELECT
  CAST(district AS STRING) AS district,
  COUNT(*) AS crime_count
FROM `bigquery-public-data.austin_crime.crime`
GROUP BY district
ORDER BY crime_count DESC;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/c76ee39e-f581-4bc9-ba0f-280719b9f10e)

6. Identify hotspots for specific crime types:
```SQL
SELECT
CAST(latitude AS FLOAT64) AS latitude,
CAST(longitude AS FLOAT64) AS longitude,
primary_type,
COUNT(*) AS crime_count
FROM `bigquery-public-data.austin_crime.crime`
GROUP BY latitude, longitude, primary_type
HAVING COUNT(*) > 10  -- Adjust threshold as needed
ORDER BY crime_count DESC;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/470578d2-815a-4a72-b5f3-f372fcfef70b)

7. Find the top 10 agencies with the highest total charges:
```SQL
SELECT
  provider_id,
  agency_name,
  SUM(total_hha_charge_amount_non_lupa) AS total_charges
FROM `bigquery-public-data.cms_medicare.home_health_agencies_2013`
GROUP BY provider_id,   agency_name
ORDER BY total_charges DESC
LIMIT 10;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/bd411de7-9908-437f-a949-aa108736e850)

8. Identify states with the highest number of home health agencies:
```SQL
SELECT
  state,
  COUNT(DISTINCT provider_id) AS num_agencies
FROM `bigquery-public-data.cms_medicare.home_health_agencies_2013`
GROUP BY state
ORDER BY num_agencies DESC
LIMIT 10;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/21aaacc0-475a-4798-893f-e36b311b73a5)

9. Identify banks with branches in multiple states: 
```SQL
SELECT
locations.institution_name,
  COUNT(DISTINCT locations.state_name) AS num_states
FROM
  `bigquery-public-data.fdic_banks.locations` AS locations
JOIN
  `bigquery-public-data.fdic_banks.institutions` AS institutions
ON
  locations.fdic_certificate_number = institutions.fdic_certificate_number
GROUP BY
  institution_name
HAVING
  COUNT(DISTINCT institutions.state_name) > 1;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/dd3182fc-30cd-45a6-b4e5-f9c41965d4ab)

10. Track changes in bank branch locations over time:  
```SQL
SELECT
  locations.institution_name,
  locations.state_name,
  established_date,
  end_effective_date,
  FROM
  `bigquery-public-data.fdic_banks.locations` AS locations
JOIN
  `bigquery-public-data.fdic_banks.institutions` AS institutions
ON
  locations.fdic_certificate_number = institutions.fdic_certificate_number
WHERE
  established_date IS NOT NULL  -- Filter for opened branches
ORDER BY
  institution_name,
  established_date DESC;
```
![image](https://github.com/Rajappannn/Big-Query_Raj/assets/158993153/7870ffa2-3bc3-453f-9ac3-08ded926c9ec)


