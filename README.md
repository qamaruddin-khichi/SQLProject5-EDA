# Exploratory Data Analysis (EDA) using MySQL: Layoffs Dataset

## Table of Contents

1. [About](#about)
2. [Purpose of Project](#purpose-of-project)
3. [SQL Code](#sql-code)
4. [Conclusion](#conclusion)

## About

This project focuses on performing Exploratory Data Analysis (EDA) using MySQL on the layoffs dataset. The dataset contains information about layoffs from March 11, 2020, to January 11, 2024. Through exploratory analysis, the project aims to uncover insights and trends within the data, providing valuable information for further investigation and decision-making processes.

## Purpose of Project

The primary purpose of this project is to explore the layoffs dataset through various SQL queries and analyses. By conducting exploratory data analysis, the project aims to answer key questions, identify patterns, and gain insights into factors influencing layoffs, such as company size, location, industry, and time. The findings from this analysis can be used to understand the dynamics of layoffs and inform strategic decisions in various domains, including business, economics, and policy-making.

## SQL Code

```sql
-- EDA - Exploratory Data Analysis

-- Have a look at the complete data
SELECT * 
FROM layoffs_staging2;

-- This data will cover the layoffs from March 11, 2020, to January 11, 2024
-- You can say, as Covid-19 hit, some companies started laying_off and some companies
-- were raising millions of dollars and later they too laid_off.
SELECT MIN(`date`) AS Staring_date ,MAX(`date`) AS Till
FROM layoffs_staging2;

-- Maximum layoffs per day
SELECT MAX(total_laid_off)
FROM layoffs_staging2;

-- Which companies had 1 which is basically 100 percent of the company laid off
SELECT *
FROM layoffs_staging2
WHERE percentage_laid_off = 1;

-- If we order by funds_raised in millions we can see how big some of these companies were
SELECT company, funds_raised As Million_Dollars
FROM layoffs_staging2
WHERE percentage_laid_off = 1
ORDER BY funds_raised DESC;

-- Companies with the biggest single Layoff
SELECT company, total_laid_off
FROM layoffs_staging
ORDER BY total_laid_off DESC
LIMIT 5;

-- Companies with the most Total Layoffs
SELECT company, SUM(total_laid_off) AS Total_layoffs
FROM layoffs_staging2
GROUP BY company
ORDER BY Total_layoffs DESC
LIMIT 10;

-- Total layoffs by location
SELECT location, SUM(total_laid_off) AS Total_layoffs
FROM layoffs_staging2
GROUP BY location
ORDER BY Total_layoffs DESC
LIMIT 10;

-- Total layoffs by country
SELECT country, SUM(total_laid_off) AS Total_layoffs
FROM layoffs_staging2
GROUP BY country
ORDER BY Total_layoffs DESC;

-- Total layoffs by year
SELECT YEAR(date) AS Year, SUM(total_laid_off) AS Total_layoffs
FROM layoffs_staging2
GROUP BY Year
ORDER BY Year ASC;

-- -- Total layoffs by month
SELECT YEAR(date) AS Year, Month(date) AS Month, SUM(total_laid_off) AS Total_layoffs
FROM layoffs_staging2
GROUP BY Year, Month
ORDER BY Year ASC;

-- Total layoffs by industry
SELECT industry, SUM(total_laid_off) AS Total_layoffs
FROM layoffs_staging2
GROUP BY industry
ORDER BY Total_layoffs DESC;

-- Total layoffs by stage
SELECT stage, SUM(total_laid_off) AS Total_layoffs
FROM world_layoffs.layoffs_staging2
GROUP BY stage
ORDER BY Total_layoffs DESC;

-- Highest laid_offs by companies by year
With Company_Year (compnay, years, Total_layoffs) As
(
SELECT company, YEAR(date) AS Year, SUM(total_laid_off) AS Total_layoffs
FROM layoffs_staging2
GROUP BY company, Year
ORDER BY Year ASC
), company_year_rank AS
(
SELECT 	*,
		DENSE_RANK() OVER (PARTITION BY years ORDER BY Total_layoffs DESC) AS Ranking
FROM company_Year
WHERE years is NOT NULL
)
SELECT *
FROM company_year_rank
WHERE Ranking <= 5
ORDER BY years DESC, Ranking ASC;
```

## Conclusion

This project demonstrates the power of exploratory data analysis using SQL in uncovering insights and trends within the layoffs dataset. Through various analyses, we were able to identify patterns, understand the impact of layoffs on different companies, locations, industries, and time periods. The findings from this analysis can provide valuable insights for understanding the dynamics of layoffs and informing strategic decisions in various domains.
