## Project Overview
In recent years, rising living costs have become a global concern, especially in emerging economies. Factors such as inflation, commodity price fluctuations, and domestic policies drive this trend, impacting both households and broader economic health. In many African countries, high inflation, volatile commodity prices, and currency fluctuations exacerbate the situation, further strained by limited infrastructure, import reliance, and unstable economic growth. This case study examines how rising living expenses affect economic stability and growth, with more emphasis on consumer behavior, inflation, employment, and poverty.

## Objectives
1. Analyze the relationship between rising costs and inflation.
2. Assess the impact on employment and the job market.
3. Investigate business operations and investments.
4. Provide policy recommendations.

## Analytics tools
+ Excel
+ Google BigQuery
+ Power BI

## Key metrics analyzed
1. Cost of Living
2. GDP(USD)
3. Foreign direct investment
4. Unemployment rate
5. Inflation rate
6. Lending interest rate
7. GDP annual growth rate

## Questions answered
+ What is the average rate of inflation across the continent from 2016 to 2021?
+ Which countries have the highest inflation rates?
+ What is the relationship between inflation and purchasing power?
+ Does inflation affect unemployment?
+ What is the relationship between rising cost of living and inflation levels?
+ What is the trend for foreign direct investments across the years?
+ How do lending interest rates affect countries afflicted with high inflation?
+ What correlation is there between GDP annual growth rate and inflation?

## Important findings
1. Inflation levels peaked in 2020 with an average of 20% and were at the lowest in 2018 (below 10%).
2. Almost half of the nations in Africa had high inflation rates across the years with Zimbabawe recording the highest average of 153.50%.
3. Over the 6-year period, high inflation levels was linked to reduced purchasing power for Algeria, Tunisia, Egypt, South Africa, Kenya, and Morocco.
4. Djibouti had the highest unemployment rate of 43.6% followed closely by South Africa at 35.1%.
5. There is no strong correlation between inflation and unmployment across the continent.
6. Inflation has a direct impact on cost of living in Morocco and Kenya.
7. Countries with the highest inflation rates also have high lending interest rates.
8. Libya recorded the highest GDP annual growth rate and sucessfully reduced its inflation rate by 89%.

## Recommendations
1. African countries should adopt better monetary policies to keep inflation rates between 2 – 4%.
2. Policy makers should be proactive to prevent inflation from reaching unmanageable levels. This will prevent them from adopting high lending interest rates which stifles business growth as a solution.
3. Governments and private stakeholders should build more industries to stimulate production. Libya successfully reduced inflation rates by improving its annual GDP growth rate.
4. Since there is a correlation between an increase in the prices of essential goods and an increase in cost of living, effective regulatory measures should be introduced to control price levels.

## Code
```Sql
---------------------------------------------Joining columns across years-----------------------------------------------------------------------------------------------------

WITH CombinedData AS (
  SELECT 
    i.Country, 
    i.Year, 
    ROUND(AVG(i.Inflation_consumer_prices_annual_percent), 2) AS inflation_percent,
   ROUND(avg(CASE 
  WHEN i.Year = 2016 THEN CAST(c2016.`Local Purchasing Power Index` AS FLOAT64)
  WHEN i.Year = 2017 THEN CAST(c2017.`Local Purchasing Power Index` AS FLOAT64)
  WHEN i.Year = 2018 THEN CAST(c2018.`Local Purchasing Power Index` AS FLOAT64)
  WHEN i.Year = 2019 THEN CAST(c2019.`Local_Purchasing_Power_Index` AS FLOAT64)
  WHEN i.Year = 2020 THEN CAST(c2020.`Local Purchasing Power Index` AS FLOAT64)
  WHEN i.Year = 2021 THEN CAST(c2021.`Local Purchasing Power Index` AS FLOAT64)
  ELSE 0.0
END), 2) AS purchasing_power
  FROM 
    `rising-costs.inflation.inflation_` i
RIGHT JOIN 
    `rising-costs.inflation.col_16` c2016 ON i.Country = c2016.Country
RIGHT JOIN 
    `rising-costs.inflation.COL_17` c2017 ON i.Country = c2017.Country
RIGHT JOIN 
    `rising-costs.inflation.col_18` c2018 ON i.Country = c2018.Country
RIGHT JOIN 
    `rising-costs.inflation.col_19` c2019 ON i.Country = c2019.Country
RIGHT JOIN 
    `rising-costs.inflation.col_20` c2020 ON i.Country = c2020.Country
RIGHT JOIN 
    `rising-costs.inflation.col_21` c2021 ON i.Country = c2021.Country
  GROUP BY 
    i.Country, i.Year
)
SELECT 
  Country, 
  Year, 
  CombinedData.inflation_percent, 
  purchasing_power
FROM 
  CombinedData
WHERE 
  Year BETWEEN 2016 AND 2021
ORDER BY 
  Country, Year;
