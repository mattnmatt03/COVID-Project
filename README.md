
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





CREATE TABLE covid_deaths (
    iso_code VARCHAR(10),
    continent VARCHAR(50),
    location VARCHAR(100),
    date DATE,
    population BIGINT,
    total_cases INT,
    new_cases INT,
    new_cases_smoothed FLOAT,
    total_deaths INT,
    new_deaths INT,
    new_deaths_smoothed FLOAT,
    total_cases_per_million FLOAT,
    new_cases_per_million FLOAT,
    new_cases_smoothed_per_million FLOAT,
    total_deaths_per_million FLOAT,
    new_deaths_per_million FLOAT,
    new_deaths_smoothed_per_million FLOAT,
    reproduction_rate FLOAT,
    icu_patients INT,
    icu_patients_per_million FLOAT,
    hosp_patients INT,
    hosp_patients_per_million FLOAT,
    weekly_icu_admissions INT,
    weekly_icu_admissions_per_million FLOAT,
    weekly_hosp_admissions INT,
    weekly_hosp_admissions_per_million FLOAT,
    month DATE
);

