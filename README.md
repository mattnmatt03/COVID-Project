# COVID-Project
-- A. Death Trends Over Time
SELECT location, date, new_deaths
FROM covid_deaths
WHERE location IN ('Afghanistan', 'Pakistan', 'Bangladesh')
ORDER BY location, date;


-- B. Top Countries by Total Deaths
SELECT location, MAX(total_deaths) AS max_total_deaths
FROM covid_deaths
GROUP BY location
ORDER BY max_total_deaths DESC
LIMIT 10;


-- C. Deaths Per Million (Per Capita)
SELECT location, population, total_deaths, total_deaths_per_million
FROM (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY location ORDER BY date DESC) AS rn
    FROM covid_deaths
) sub
WHERE rn = 1
ORDER BY total_deaths_per_million DESC
LIMIT 10;


-- D. Monthly Average Deaths
SELECT location, DATE_TRUNC('month', date) AS month, AVG(new_deaths) AS avg_monthly_deaths
FROM covid_deaths
GROUP BY location, month
ORDER BY location, month;


-- E. Death Intensity by Continent
SELECT continent, DATE_TRUNC('month', date) AS month, AVG(new_deaths_per_million) AS avg_deaths_per_million
FROM covid_deaths
GROUP BY continent, month
ORDER BY continent, month;
