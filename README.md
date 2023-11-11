# Exploring-London-s-Travel-Network
SQL to analyze a database containing information about Transport for London journeys over 12 years!


Snowflake
Use Snowflake to build a project that has a specific solution, with guided tasks and real-time automated code checks

TFL.JOURNEYS
Column	Definition	Data type
MONTH	Month in number format, e.g., 1 equals January	INTEGER
YEAR	Year	INTEGER
DAYS	Number of days in the given month	INTEGER
REPORT_DATE	Date that the data was reported	DATE
JOURNEY_TYPE	Method of transport used	VARCHAR
JOURNEYS_MILLIONS	Millions of journeys, measured in decimals	FLOAT
Note that in Snowflake all databases, tables, and columns are upper case by default.

1. What are the most popular transport types, measured by the total number of journeys?
   The output should contain two columns, 1) JOURNEY_TYPE and 2) TOTAL_JOURNEYS_MILLIONS, and be sorted by the second column in descending order.
   Save the query as most_popular_transport_types

   -- most_popular_transport_types
WITH most_popular_transport_types AS 
(
SELECT M.JOURNEY_TYPE, sum(M.JOURNEYS_MILLIONS) as TOTAL_JOURNEYS_MILLIONS
FROM TFL.JOURNEYS  M
GROUP BY M.JOURNEY_TYPE
ORDER BY 2 desc
)
SELECT * from most_popular_transport_types


2. Which five months and years were the most popular for the Emirates Airline?
   Return an output containing MONTH, YEAR, and JOURNEYS_MILLIONS, with the latter rounded to two decimal places and aliased as ROUNDED_JOURNEYS_MILLIONS.
   Exclude null values and save the result as emirates_airline_popularity

-- emirates_airline_popularity
WITH emirates_airline_popularity as
(
SELECT M.MONTH,M.YEAR , ROUND(M.JOURNEYS_MILLIONS,2) AS ROUNDED_JOURNEYS_MILLIONS
FROM TFL.JOURNEYS  M
WHERE JOURNEY_TYPE = 'Emirates Airline' AND ROUNDED_JOURNEYS_MILLIONS IS NOT NULL
ORDER BY ROUNDED_JOURNEYS_MILLIONS DESC
LIMIT 5
	)
	select * from emirates_airline_popularity

3. Find the five years with the lowest volume of Underground & DLR journeys, saving as least_popular_years_tube. The results should contain the columns YEAR, JOURNEY_TYPE, and TOTAL_JOURNEYS_MILLIONS.

-- least_popular_years_tube
with least_popular_years_tube as 
(
    SELECT M.YEAR, M.JOURNEY_TYPE, SUM(M.JOURNEYS_MILLIONS) AS TOTAL_JOURNEYS_MILLIONS
    FROM TFL.JOURNEYS M
	WHERE JOURNEY_TYPE LIKE '%Underground%'
    GROUP BY M.YEAR, M.JOURNEY_TYPE
    ORDER BY TOTAL_JOURNEYS_MILLIONS ASC
    LIMIT 5
)

SELECT * FROM least_popular_years_tube;


 



