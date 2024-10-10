# Exploratory Data Analysis of Company Layoffs using MySQL 

## Project Overview
This project aims to analyze company layoffs using SQL to derive key insights on trends, patterns, and metrics such as the number of layoffs, percentage of employees laid off, and year-over-year fluctuations. The analysis focuses on understanding layoffs across different companies, industries, and time periods, identifying the contributing factors behind workforce reductions.

## Key Objectives
Analyze Layoff Trends: Explore historical data to identify companies with the highest and lowest layoffs over the years.

## Calculate Key Metrics:
Total number of employees laid off per company.\
Percentage of layoffs relative to total employees.\
Minimum and maximum layoffs per company and year.\
Yearly Analysis: Investigate layoffs on an annual basis.\
Industry-Specific Insights: Identify which industries were most affected.\
Company Size & Layoffs: Explore the correlation between company size and the number of layoffs.

## Dataset:
The dataset contains layoff information from various companies and includes the following key fields:

Company Name: Name of the company.\
Industry: Sector in which the company operates.\
Employees Laid Off: Number of employees laid off.\
Total Employees: Total number of employees before layoffs.\
Percentage Laid Off: Proportion of employees laid off.\
Date: Year in which layoffs occurred.\
Country: Geographic location of the company.\
Funds Raised (Millions): Funds raised by the company to date.

## Methodology

*Step 1: Exploratory Data Analysis (EDA)*
Using SQL queries to explore and summarize the dataset

### Total Layoffs Per Company:
sql\
select company, sum(total_laid_off)\
from layoffs_staging2\
group by company\
order by sum(total_laid_off) desc;

### Calculate Percentage of Layoffs Per Company:
sql\
select company, percentage_laid_off, total_laid_off\
from layoffs_staging2\
where percentage_laid_off = 1\
order by total_laid_off desc;

### Annual Trends:
sql\
select year(`date`),sum(total_laid_off)\
from layoffs_staging2\
group by year(`date`)\
order by 2 desc;

### Industry-Specific Layoffs:
sql\
select industry, sum(total_laid_off)\
from layoffs_staging2\
group by industry\
order by 2 desc;

*Step 2: Ranking and Metrics*
### Rolling Total for Layoffs per Month:
sql\
with rolling_total as\
(\
select substring(`date`, 1,7)as`month`,sum(total_laid_off) as total_off\
from layoffs_staging2\
where  substring(`date`, 1,7) is not null\
group by `month`\
order by 1 asc\
)\
select `month`,total_off, sum(total_off) over(order by `month`) as rolling_total\
from rolling_total;

### Dense Rank for Companies:
sql\
with company_years(company, years, total_laid_off) as\
(\
select company, year(`date`), sum(total_laid_off)\
from layoffs_staging2\
group by company, year(`date`)\
order by 3 desc\
),company_year_rank as\
(\
select *, dense_rank() over(partition by years order by total_laid_off desc) as ranking\
from company_years\
where years is not null\
)\
select *\
from company_year_rank\
where ranking <=5;\

### Min/Max Layoffs:
sql\
select max(total_laid_off), min(total_laid_off)\
from layoffs_staging2;

## Expected Outcomes:
A detailed report on the companies most affected by layoffs, both in total number and percentage of employees laid off.
Insights into years with the highest layoff rates and potential causes (e.g., economic downturns, global events).
Industry-specific analysis to determine if certain sectors were disproportionately affected.
Minimum and maximum layoffs to identify outliers in the dataset.
Expand analysis to include geographic trends (layoffs by country).
Investigate any correlation between funds raised and the likelihood or magnitude of layoffs.

## Future Work:
Enhance visualizations for interactive data exploration in Tableau.

