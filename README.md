# Descriptive Analysis of Unmet Health Care Needs

## Table of Contents
  - [Objective](#objective)
  - [Dataset](#dataset)
  - [Tools](#tools)
  - [Methodology](#methodology)
  - [Key Achievements](#key-achievements)

# Objective

The primary goal of this project is to analyzing data on unmet health care needs to understand patterns and identify key demographics affected by access issues. 

# Dataset

The dataset from Statistics Canada: Table 13-10-0836-01  Unmet health care needs by sex and age group (DOI: https://doi.org/10.25318/1310083601-eng3) contains the following key features:

Year:Year
- GEO: Geography location of Country or Province such as British Columbia, Ontario, Nova Scotia, etc.
- Sex: Female, Male, or both sexes.
- Value: Quantity of people in thousands.

## Tools

- Excel for data cleaning, exploratory analysis, and visualization.

# Methodology

## 1. Data Collection and Preparation

The data cleaning includes:

Data loading and inspection
Handling missing and duplicate values
Standardizing the structure of the data
## 2. Descriptive Analysis
The analysis includes answers to questions such as:

What was the total laid off for each year and month?
WITH Rolling_Total AS
(
SELECT SUBSTRING(`date`, 1,7) AS `MONTH`, SUM(total_laid_off) AS total_off
FROM layoffs_staging2
WHERE SUBSTRING(`date`, 1,7) IS NOT NULL
GROUP BY `MONTH`
ORDER BY `MONTH` ASC
)
SELECT `MONTH`, total_off,
SUM(total_off) OVER(ORDER BY `MONTH`) AS rolling_total
FROM Rolling_Total;
images

What was the top 5 ranking of laid-off for each year?
WITH Company_Year (company, years, total_laid_off) AS
(
SELECT company, YEAR(`date`), SUM(total_laid_off) 
FROM layoffs_staging2
GROUP BY company, YEAR(`date`)
), Company_Year_Rank AS
(SELECT *, 
DENSE_RANK() OVER (PARTITION BY years ORDER BY total_laid_off DESC) AS Ranking
FROM Company_Year
WHERE years IS NOT NULL
)
SELECT *
FROM Company_Year_Rank
WHERE Ranking <= 5
;
images

SQL Data Exploration Analysis
## 3. Data Visualization
Dashboard Tableau

images

## 4. Insights and Findings
The highest peaks of total worldwide layoffs between March 2020 and March 2023 were in November 2022, with 53,451, and in January 2023, with 84,714.
The consumer industry was the hardest hit for the period under study, with 45,182 layoffs, followed by the retail sector with 43,613 layoffs.
Amazon, Google, and Meta were the top three companies with the most layoffs globally, with 18, 150, 12,000, and 11,000, respectively.
This explains why the United States has the highest number of layoffs in the world, with 256,059 total layoffs between March 2020 and 2023.
