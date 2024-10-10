# Exploratory Data Analysis of Company Layoffs using MySQL 
##Project Overview
This project aims to analyze company layoffs using SQL to derive key insights on trends, patterns, and metrics such as the number of layoffs, percentage of employees laid off, and year-over-year fluctuations. The analysis focuses on understanding layoffs across different companies, industries, and time periods, identifying the contributing factors behind workforce reductions.

##Key Objectives
Analyze Layoff Trends: Explore historical data to identify companies with the highest and lowest layoffs over the years.
##Calculate Key Metrics:
Total number of employees laid off per company.
Percentage of layoffs relative to total employees.
Minimum and maximum layoffs per company and year.
Yearly Analysis: Investigate layoffs on an annual basis.
Industry-Specific Insights: Identify which industries were most affected.
Company Size & Layoffs: Explore the correlation between company size and the number of layoffs.

##Dataset
The dataset contains layoff information from various companies and includes the following key fields:
Company Name: Name of the company.
Industry: Sector in which the company operates.
Employees Laid Off: Number of employees laid off.
Total Employees: Total number of employees before layoffs.
Percentage Laid Off: Proportion of employees laid off ((Employees Laid Off / Total Employees) * 100).
Date: Year in which layoffs occurred.
Country: Geographic location of the company.
Funds Raised (Millions): Funds raised by the company to date.
Methodology
Step 1: Data Cleaning & Preprocessing
Before performing exploratory analysis, the dataset is cleaned and prepared:

Check for Missing Data:
sql
Copy code
SELECT * 
FROM layoffs
WHERE total_employees IS NULL OR employees_laid_off IS NULL;
Handle Missing or Invalid Data: Decide whether to remove or impute missing values, and handle outliers like negative layoffs.
Step 2: Exploratory Data Analysis (EDA)
Using SQL queries to explore and summarize the dataset:

Total Layoffs Per Company:
sql
Copy code
SELECT company_name, SUM(employees_laid_off) AS total_laid_off
FROM layoffs
GROUP BY company_name
ORDER BY total_laid_off DESC;
Calculate Percentage of Layoffs Per Company:
sql
Copy code
SELECT company_name, (SUM(employees_laid_off) / SUM(total_employees)) * 100 AS percent_laid_off
FROM layoffs
GROUP BY company_name
ORDER BY percent_laid_off DESC;
Annual Trends:
sql
Copy code
SELECT YEAR(date) AS year, SUM(employees_laid_off) AS total_laid_off
FROM layoffs
GROUP BY YEAR(date)
ORDER BY year;
Industry-Specific Layoffs:
sql
Copy code
SELECT industry, SUM(employees_laid_off) AS total_laid_off
FROM layoffs
GROUP BY industry
ORDER BY total_laid_off DESC;
Step 3: Ranking and Metrics
Dense Rank for Layoff Rankings:
sql
Copy code
SELECT company_name, year, employees_laid_off, 
       DENSE_RANK() OVER (PARTITION BY year ORDER BY employees_laid_off DESC) AS rank
FROM layoffs;
Min/Max Layoffs:
sql
Copy code
SELECT MIN(employees_laid_off) AS min_layoffs, MAX(employees_laid_off) AS max_layoffs
FROM layoffs;
Step 4: Visualization
While the analysis is performed in MySQL, results are visualized using Tableau or Excel for better insights:

Bar Charts: To show total layoffs by company.
Line Charts: For visualizing yearly trends in layoffs.
Pie Charts: For industry distribution of layoffs.
Expected Outcomes
A detailed report on the companies most affected by layoffs, both in total number and percentage of employees laid off.
Insights into years with the highest layoff rates and potential causes (e.g., economic downturns, global events).
Industry-specific analysis to determine if certain sectors were disproportionately affected.
Minimum and maximum layoffs to identify outliers in the dataset.
Expand analysis to include geographic trends (layoffs by country).
Investigate any correlation between funds raised and the likelihood or magnitude of layoffs.
Future Work:
Enhance visualizations for interactive data exploration in Tableau.

