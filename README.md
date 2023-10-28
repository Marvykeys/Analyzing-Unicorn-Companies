# :chart_with_upwards_trend: Analyzing-Unicorn-Companies :bar_chart: 
A Firm is interested in understanding which
industries are producing the highest valuations and the rate at which
new high-value companies are emerging. Providing them with this
information gives them a competitive insight as to industry trends and
how they should structure their portfolio looking forward.

The `unicorns` database, contains the following tables:

`dates`
| Column       | Description                                  |
|------------- |--------------------------------------------- |
| company_id   | A unique ID for the company.                 |
| date_joined  | The date that the company became a unicorn.  |
| year_founded | The year that the company was founded.       |

`funding`
| Column           | Description                                  |
|----------------- |--------------------------------------------- |
| company_id       | A unique ID for the company.                 |
| valuation        | Company value in US dollars.                 |
| funding          | The amount of funding raised in US dollars.  |
| select_investors | A list of key investors in the company.      |

`industries`
| Column       | Description                                  |
|------------- |--------------------------------------------- |
| company_id   | A unique ID for the company.                 |
| industry     | The industry that the company operates in.   |

`companies`
| Column       | Description                                       |
|------------- |-------------------------------------------------- |
| company_id   | A unique ID for the company.                      |
| company      | The name of the company.                          |
| city         | The city where the company is headquartered.      |
| country      | The country where the company is headquartered.   |
| continent    | The continent where the company is headquartered. |

Our final query should return the industry, the year, the number of companies in these industries that became unicorns each year in 2019, 2020, and 2021, along with the average valuation per industry per year, converted to billions of dollars and rounded to two decimal places!

As the firm is interested in trends for the top-performing industries, our results should be displayed by industry, then year in descending order.

#

 ### 1. Find the three (3) best performing industries based on the number of new unicorns created over three (3) years (2019,2020 and 2021) combined.

``` python
SELECT 
       i.industry, 
       COUNT(i.*) AS count_new_unicorns
FROM industries AS i
JOIN dates AS d
ON i.company_id = d.company_id
WHERE EXTRACT(YEAR FROM d.date_joined) IN(2019,2020,2021)
GROUP BY i.industry
ORDER BY count_new_unicorns DESC
LIMIT 3;
```
|       | industry | count_new_unicorns |
|-------|-------|-------|
| 0 | Fintech | 173 |
| 1 | Internet software & services | 152 |
| 2 | E-commerce & direct-to-consumer |   75 |

#

### 2. Calculate the number of unicorns and the average valuation, grouped by year and industry.

``` python
SELECT
      i.industry AS industry,
	  EXTRACT(YEAR FROM d.date_joined) AS year,
	  COUNT(i.*) AS num_unicorns,
	  AVG(f.valuation) AS average_valuation
FROM industries AS i
JOIN dates AS d
ON i.company_id = d.company_id
JOIN funding AS f
ON d.company_id = f.company_id
GROUP BY industry, year;
```

|          | industry | year  | num_unicorns | average_valuation | 
|-------|-------|-------|------|------|
| 0 | Mobile & telecommunications | 2017 | 5 | 2800000000 |
| 1 | Internet software & services | 2015 | 4 | 1250000000 |
| 2 | Fintech | 2018 | 10 | 8600000000 |
| 3 | Mobile & telecommunications | 2019 | 4 | 2000000000 |
| 4 |Artificial intelligence | 2012 | 1 | 2000000000 |
| 5 | Fintech | 2015 | 2 | 5500000000 |
| 6 | Hardware | 2020 | 1 | 2000000000 |
| 7 | Data Management & analaytics | 2019 | 4 | 11500000000 |
| 8 | Supply chain, logistics & delivery | 2018 | 11 | 3454545454.5454545 |
| 9 | Travel | 2017 | 2 | 2000000000 |

#

### 3. Create two (2) CTEs for the two (2) tables above, then run the final query. 

``` python
WITH top_industries AS (SELECT 
       i.industry, 
       COUNT(i.*) AS count_new_unicorns
FROM industries AS i
JOIN dates AS d
ON i.company_id = d.company_id
WHERE EXTRACT(YEAR FROM d.date_joined) IN(2019,2020,2021)
GROUP BY i.industry
ORDER BY count_new_unicorns DESC
LIMIT 3
	),
     yearly_ranks AS (SELECT
      i.industry AS industry,
	  EXTRACT(YEAR FROM d.date_joined) AS year,
	  COUNT(i.*) AS num_unicorns,
	  AVG(f.valuation) AS average_valuation
FROM industries AS i
JOIN dates AS d
ON i.company_id = d.company_id
JOIN funding AS f
ON d.company_id = f.company_id
GROUP BY i.industry, year
	)
SELECT
      industry,
	  year,
	  num_unicorns,
	  ROUND(AVG(average_valuation/1000000000),2) AS average_valuation_billions
FROM yearly_ranks
WHERE year IN (2019, 2020, 2021)
        AND industry IN (SELECT industry FROM top_industries)
GROUP BY industry, num_unicorns, year
ORDER BY industry, year DESC;
```
|          | industry | year  | num_unicorns | average_valuation | 
|-------|-------|-------|------|------|
| 0 | Mobile & telecommunications | 2017 | 5 | 2800000000 |
| 1 | Internet software & services | 2015 | 4 | 1250000000 |
| 2 | Fintech | 2018 | 10 | 8600000000 |
| 3 | Mobile & telecommunications | 2019 | 4 | 2000000000 |
| 4 |Artificial intelligence | 2012 | 1 | 2000000000 |
| 5 | Fintech | 2015 | 2 | 5500000000 |
| 6 | Hardware | 2020 | 1 | 2000000000 |
| 7 | Data Management & analaytics | 2019 | 4 | 11500000000 |
| 8 | Supply chain, logistics & delivery | 2018 | 11 | 3454545454.5454545 |
| 9 | Travel | 2017 | 2 | 2000000000 |

