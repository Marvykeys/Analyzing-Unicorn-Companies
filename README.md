# Analyzing-Unicorn-Companies
Did you know that the average return from investing in stocks is 10% per
year! But who wants to be average?!

Support an investment firm by analyzing trends in
high-growth companies. The Firm is interested in understanding which
industries are producing the highest valuations and the rate at which
new high-value companies are emerging. Providing them with this
information gives them a competitive insight as to industry trends and
how they should structure their portfolio looking forward.

The `unicorns` database, contains
the following tables:

`dates` \| Column \| Description \| \|\-\-\-\-\-\-\-\-\-\-\-\--
\|\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
\| \| company_id \| A unique ID for the company. \| \| date_joined \|
The date that the company became a unicorn. \| \| year_founded \| The
year that the company was founded. \|

`funding` \| Column \| Description \|
\|\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
\|\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
\| \| company_id \| A unique ID for the company. \| \| valuation \|
Company value in US dollars. \| \| funding \| The amount of funding
raised in US dollars. \| \| select_investors \| A list of key investors
in the company. \|

`industries` \| Column \| Description \| \|\-\-\-\-\-\-\-\-\-\-\-\--
\|\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
\| \| company_id \| A unique ID for the company. \| \| industry \| The
industry that the company operates in. \|

`companies` \| Column \| Description \| \|\-\-\-\-\-\-\-\-\-\-\-\--
\|\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
\| \| company_id \| A unique ID for the company. \| \| company \| The
name of the company. \| \| city \| The city where the company is
headquartered. \| \| country \| The country where the company is
headquartered. \| \| continent \| The continent where the company is
headquartered. \|
:::

::: {#c188c634-1227-47a0-832d-39f69f064b67 .cell .markdown}
### 1. Find the three (3) best performing industries based on the number of new unicorns created over three (3) years (2019,2020 and 2021) combined. {#1-find-the-three-3-best-performing-industries-based-on-the-number-of-new-unicorns-created-over-three-3-years-20192020-and-2021-combined}
:::

::: {#7bda904b-0b3b-4b1e-9a3d-4c4aa9b3d3d3 .cell .code execution_count="4" chartConfig="{\"bar\":{\"hasRoundedCorners\":true,\"stacked\":false},\"type\":\"bar\",\"version\":\"v1\"}" customType="sql" dataFrameVariableName="df" executionTime="1576" initial="false" integrationId="89e17161-a224-4a8a-846b-0adc0fe7a4b1" lastSuccessfullyExecutedCode="SELECT 
       i.industry, 
       COUNT(i.*) AS count_new_unicorns
FROM industries AS i
JOIN dates AS d
ON i.company_id = d.company_id
WHERE EXTRACT(YEAR FROM d.date_joined) IN(2019,2020,2021)
GROUP BY i.industry
ORDER BY count_new_unicorns DESC
LIMIT 3;" visualizeDataframe="false"}
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

::: {.output .execute_result execution_count="4"}
``` json
{"table":{"data":[{"count_new_unicorns":173,"index":0,"industry":"Fintech"},{"count_new_unicorns":152,"index":1,"industry":"Internet software & services"},{"count_new_unicorns":75,"index":2,"industry":"E-commerce & direct-to-consumer"}],"schema":{"fields":[{"name":"index","type":"integer"},{"name":"industry","type":"string"},{"name":"count_new_unicorns","type":"integer"}],"pandas_version":"1.4.0","primaryKey":["index"]}},"total_rows":3,"truncation_type":null}
```
:::
:::

::: {#beedad9b-f403-4d23-9216-f1d9d3e401e8 .cell .markdown}
### 2. Calculate the number of unicorns and the average valuation, grouped by year and industry. {#2-calculate-the-number-of-unicorns-and-the-average-valuation-grouped-by-year-and-industry}
:::

::: {#6a229a59-1381-4adb-80fd-5ea52d76b905 .cell .code execution_count="5" customType="sql" dataFrameVariableName="df1" executionTime="1806" lastSuccessfullyExecutedCode="SELECT
      i.industry AS industry,
	  EXTRACT(YEAR FROM d.date_joined) AS year,
	  COUNT(i.*) AS num_unicorns,
	  AVG(f.valuation) AS average_valuation
FROM industries AS i
JOIN dates AS d
ON i.company_id = d.company_id
JOIN funding AS f
ON d.company_id = f.company_id
GROUP BY industry, year;" sqlCellMode="dataFrame" sqlSource="{\"integrationId\":\"89e17161-a224-4a8a-846b-0adc0fe7a4b1\",\"type\":\"integration\"}"}
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

::: {.output .execute_result execution_count="5"}
``` json
{"table":{"data":[{"average_valuation":2800000000,"index":0,"industry":"Mobile & telecommunications","num_unicorns":5,"year":2017},{"average_valuation":1250000000,"index":1,"industry":"Internet software & services","num_unicorns":4,"year":2015},{"average_valuation":8600000000,"index":2,"industry":"Fintech","num_unicorns":10,"year":2018},{"average_valuation":2000000000,"index":3,"industry":"Mobile & telecommunications","num_unicorns":4,"year":2019},{"average_valuation":2000000000,"index":4,"industry":"Artificial intelligence","num_unicorns":1,"year":2012},{"average_valuation":5500000000,"index":5,"industry":"Fintech","num_unicorns":2,"year":2015},{"average_valuation":2000000000,"index":6,"industry":"Hardware","num_unicorns":1,"year":2020},{"average_valuation":11500000000,"index":7,"industry":"Data management & analytics","num_unicorns":4,"year":2019},{"average_valuation":3.4545454545454545e9,"index":8,"industry":"Supply chain, logistics, & delivery","num_unicorns":11,"year":2018},{"average_valuation":2000000000,"index":9,"industry":"Travel","num_unicorns":2,"year":2017},{"average_valuation":3000000000,"index":10,"industry":"Supply chain, logistics, & delivery","num_unicorns":8,"year":2019},{"average_valuation":4250000000,"index":11,"industry":"Hardware","num_unicorns":4,"year":2015},{"average_valuation":1000000000,"index":12,"industry":"Artificial intelligence","num_unicorns":1,"year":2016},{"average_valuation":3000000000,"index":13,"industry":"Artificial intelligence","num_unicorns":13,"year":2018},{"average_valuation":7000000000,"index":14,"industry":"Auto & transportation","num_unicorns":1,"year":2014},{"average_valuation":2.333333333333333e9,"index":15,"industry":"Consumer & retail","num_unicorns":6,"year":2018},{"average_valuation":3000000000,"index":16,"industry":"Auto & transportation","num_unicorns":2,"year":2022},{"average_valuation":1.1666666666666666e10,"index":17,"industry":"Other","num_unicorns":3,"year":2018},{"average_valuation":2000000000,"index":18,"industry":"Other","num_unicorns":2,"year":2016},{"average_valuation":1.8571428571428568e9,"index":19,"industry":"Health","num_unicorns":7,"year":2022},{"average_valuation":2.6666666666666665e9,"index":20,"industry":"Travel","num_unicorns":3,"year":2021},{"average_valuation":4000000000,"index":21,"industry":"Artificial intelligence","num_unicorns":3,"year":2020},{"average_valuation":2000000000,"index":22,"industry":"Data management & analytics","num_unicorns":1,"year":2013},{"average_valuation":2750000000,"index":23,"industry":"Edtech","num_unicorns":4,"year":2020},{"average_valuation":100000000000,"index":24,"industry":"Other","num_unicorns":1,"year":2012},{"average_valuation":47500000000,"index":25,"industry":"Artificial intelligence","num_unicorns":4,"year":2017},{"average_valuation":1875000000,"index":26,"industry":"Supply chain, logistics, & delivery","num_unicorns":8,"year":2022},{"average_valuation":6000000000,"index":27,"industry":"Cybersecurity","num_unicorns":2,"year":2015},{"average_valuation":6000000000,"index":28,"industry":"Internet software & services","num_unicorns":1,"year":2011},{"average_valuation":8000000000,"index":29,"industry":"Hardware","num_unicorns":2,"year":2016},{"average_valuation":2.5185185185185184e9,"index":30,"industry":"Cybersecurity","num_unicorns":27,"year":2021},{"average_valuation":2000000000,"index":31,"industry":"Edtech","num_unicorns":12,"year":2021},{"average_valuation":6.142857142857143e9,"index":32,"industry":"Health","num_unicorns":7,"year":2018},{"average_valuation":1000000000,"index":33,"industry":"Internet software & services","num_unicorns":1,"year":2016},{"average_valuation":27000000000,"index":34,"industry":"E-commerce & direct-to-consumer","num_unicorns":1,"year":2012},{"average_valuation":27250000000,"index":35,"industry":"E-commerce & direct-to-consumer","num_unicorns":4,"year":2018},{"average_valuation":46000000000,"index":36,"industry":"Fintech","num_unicorns":1,"year":2011},{"average_valuation":4000000000,"index":37,"industry":"E-commerce & direct-to-consumer","num_unicorns":16,"year":2020},{"average_valuation":2.1512605042016807e9,"index":38,"industry":"Internet software & services","num_unicorns":119,"year":2021},{"average_valuation":2000000000,"index":39,"industry":"Travel","num_unicorns":1,"year":2015},{"average_valuation":3.333333333333333e9,"index":40,"industry":"Health","num_unicorns":3,"year":2019},{"average_valuation":3125000000,"index":41,"industry":"Mobile & telecommunications","num_unicorns":8,"year":2020},{"average_valuation":3.6666666666666665e9,"index":42,"industry":"Consumer & retail","num_unicorns":3,"year":2019},{"average_valuation":2.888888888888889e9,"index":43,"industry":"Other","num_unicorns":9,"year":2019},{"average_valuation":1.8333333333333333e9,"index":44,"industry":"Mobile & telecommunications","num_unicorns":6,"year":2021},{"average_valuation":7000000000,"index":45,"industry":"Mobile & telecommunications","num_unicorns":1,"year":2016},{"average_valuation":1000000000,"index":46,"industry":"Supply chain, logistics, & delivery","num_unicorns":1,"year":2017},{"average_valuation":1.3333333333333333e9,"index":47,"industry":"Consumer & retail","num_unicorns":3,"year":2016},{"average_valuation":1000000000,"index":48,"industry":"Edtech","num_unicorns":2,"year":2015},{"average_valuation":2000000000,"index":49,"industry":"Hardware","num_unicorns":14,"year":2021},{"average_valuation":3750000000,"index":50,"industry":"Internet software & services","num_unicorns":4,"year":2017},{"average_valuation":2000000000,"index":51,"industry":"Health","num_unicorns":1,"year":2016},{"average_valuation":1.7142857142857141e9,"index":52,"industry":"Other","num_unicorns":21,"year":2021},{"average_valuation":1800000000,"index":53,"industry":"Other","num_unicorns":5,"year":2022},{"average_valuation":1750000000,"index":54,"industry":"E-commerce & direct-to-consumer","num_unicorns":4,"year":2014},{"average_valuation":1000000000,"index":55,"industry":"Data management & analytics","num_unicorns":4,"year":2022},{"average_valuation":7.928571428571427e9,"index":56,"industry":"Internet software & services","num_unicorns":14,"year":2018},{"average_valuation":2.468085106382979e9,"index":57,"industry":"E-commerce & direct-to-consumer","num_unicorns":47,"year":2021},{"average_valuation":6.666666666666666e9,"index":58,"industry":"E-commerce & direct-to-consumer","num_unicorns":6,"year":2016},{"average_valuation":1500000000,"index":59,"industry":"Artificial intelligence","num_unicorns":2,"year":2015},{"average_valuation":4750000000,"index":60,"industry":"Travel","num_unicorns":4,"year":2018},{"average_valuation":3000000000,"index":61,"industry":"Other","num_unicorns":11,"year":2020},{"average_valuation":1000000000,"index":62,"industry":"Edtech","num_unicorns":1,"year":2019},{"average_valuation":4000000000,"index":63,"industry":"Health","num_unicorns":1,"year":2014},{"average_valuation":2.4545454545454545e9,"index":64,"industry":"Auto & transportation","num_unicorns":11,"year":2018},{"average_valuation":95000000000,"index":65,"industry":"Fintech","num_unicorns":1,"year":2014},{"average_valuation":1000000000,"index":66,"industry":"Cybersecurity","num_unicorns":8,"year":2022},{"average_valuation":4000000000,"index":67,"industry":"Edtech","num_unicorns":2,"year":2016},{"average_valuation":1.5555555555555556e9,"index":68,"industry":"E-commerce & direct-to-consumer","num_unicorns":9,"year":2015},{"average_valuation":3000000000,"index":69,"industry":"Auto & transportation","num_unicorns":5,"year":2020},{"average_valuation":3.111111111111111e9,"index":70,"industry":"Health","num_unicorns":9,"year":2020},{"average_valuation":4.1666666666666665e9,"index":71,"industry":"Auto & transportation","num_unicorns":6,"year":2019},{"average_valuation":8000000000,"index":72,"industry":"Cybersecurity","num_unicorns":1,"year":2018},{"average_valuation":1800000000,"index":73,"industry":"Mobile & telecommunications","num_unicorns":5,"year":2018},{"average_valuation":1.7096774193548388e9,"index":74,"industry":"Fintech","num_unicorns":31,"year":2022},{"average_valuation":2.583333333333333e9,"index":75,"industry":"E-commerce & direct-to-consumer","num_unicorns":12,"year":2019},{"average_valuation":5500000000,"index":76,"industry":"Edtech","num_unicorns":2,"year":2018},{"average_valuation":2.333333333333333e9,"index":77,"industry":"Mobile & telecommunications","num_unicorns":3,"year":2014},{"average_valuation":4.333333333333333e9,"index":78,"industry":"Fintech","num_unicorns":15,"year":2020},{"average_valuation":4500000000,"index":79,"industry":"Artificial intelligence","num_unicorns":14,"year":2019},{"average_valuation":1500000000,"index":80,"industry":"Mobile & telecommunications","num_unicorns":4,"year":2015},{"average_valuation":39000000000,"index":81,"industry":"Supply chain, logistics, & delivery","num_unicorns":1,"year":2014},{"average_valuation":1000000000,"index":82,"industry":"Mobile & telecommunications","num_unicorns":2,"year":2022},{"average_valuation":2500000000,"index":83,"industry":"Hardware","num_unicorns":2,"year":2022},{"average_valuation":4000000000,"index":84,"industry":"Health","num_unicorns":2,"year":2015},{"average_valuation":2000000000,"index":85,"industry":"Internet software & services","num_unicorns":28,"year":2022},{"average_valuation":2000000000,"index":86,"industry":"Auto & transportation","num_unicorns":1,"year":2015},{"average_valuation":1600000000,"index":87,"industry":"Artificial intelligence","num_unicorns":10,"year":2022},{"average_valuation":1750000000,"index":88,"industry":"Other","num_unicorns":4,"year":2017},{"average_valuation":3.2857142857142854e9,"index":89,"industry":"Cybersecurity","num_unicorns":7,"year":2020},{"average_valuation":2000000000,"index":90,"industry":"Auto & transportation","num_unicorns":1,"year":2016},{"average_valuation":2.1428571428571427e9,"index":91,"industry":"Data management & analytics","num_unicorns":21,"year":2021},{"average_valuation":2.5714285714285717e9,"index":92,"industry":"Consumer & retail","num_unicorns":7,"year":2021},{"average_valuation":4350000000,"index":93,"industry":"Internet software & services","num_unicorns":20,"year":2020},{"average_valuation":2.6666666666666665e9,"index":94,"industry":"Hardware","num_unicorns":9,"year":2018},{"average_valuation":2000000000,"index":95,"industry":"Consumer & retail","num_unicorns":1,"year":2012},{"average_valuation":2.753623188405797e9,"index":96,"industry":"Fintech","num_unicorns":138,"year":2021},{"average_valuation":3000000000,"index":97,"industry":"Data management & analytics","num_unicorns":6,"year":2020},{"average_valuation":1.6666666666666665e9,"index":98,"industry":"Fintech","num_unicorns":6,"year":2017},{"average_valuation":5.333333333333333e9,"index":99,"industry":"Data management & analytics","num_unicorns":3,"year":2018},{"average_valuation":4.2307692307692313e9,"index":100,"industry":"Internet software & services","num_unicorns":13,"year":2019},{"average_valuation":3500000000,"index":101,"industry":"Hardware","num_unicorns":2,"year":2014},{"average_valuation":4000000000,"index":102,"industry":"Travel","num_unicorns":3,"year":2019},{"average_valuation":10500000000,"index":103,"industry":"Edtech","num_unicorns":4,"year":2017},{"average_valuation":1500000000,"index":104,"industry":"E-commerce & direct-to-consumer","num_unicorns":4,"year":2017},{"average_valuation":2200000000,"index":105,"industry":"Supply chain, logistics, & delivery","num_unicorns":25,"year":2021},{"average_valuation":3750000000,"index":106,"industry":"Auto & transportation","num_unicorns":4,"year":2021},{"average_valuation":3000000000,"index":107,"industry":"Health","num_unicorns":4,"year":2017},{"average_valuation":1950000000,"index":108,"industry":"Health","num_unicorns":40,"year":2021},{"average_valuation":2000000000,"index":109,"industry":"Supply chain, logistics, & delivery","num_unicorns":2,"year":2020},{"average_valuation":2500000000,"index":110,"industry":"Data management & analytics","num_unicorns":2,"year":2017},{"average_valuation":1000000000,"index":111,"industry":"Cybersecurity","num_unicorns":1,"year":2013},{"average_valuation":1000000000,"index":112,"industry":"Edtech","num_unicorns":1,"year":2022},{"average_valuation":1.4166666666666665e9,"index":113,"industry":"Artificial intelligence","num_unicorns":36,"year":2021},{"average_valuation":1000000000,"index":114,"industry":"Supply chain, logistics, & delivery","num_unicorns":1,"year":2016},{"average_valuation":1000000000,"index":115,"industry":"E-commerce & direct-to-consumer","num_unicorns":1,"year":2007},{"average_valuation":3000000000,"index":116,"industry":"Internet software & services","num_unicorns":1,"year":2013},{"average_valuation":15000000000,"index":117,"industry":"Consumer & retail","num_unicorns":1,"year":2020},{"average_valuation":2250000000,"index":118,"industry":"Cybersecurity","num_unicorns":4,"year":2019},{"average_valuation":1.5714285714285715e9,"index":119,"industry":"E-commerce & direct-to-consumer","num_unicorns":7,"year":2022},{"average_valuation":1000000000,"index":120,"industry":"Other","num_unicorns":2,"year":2015},{"average_valuation":10500000000,"index":121,"industry":"Consumer & retail","num_unicorns":4,"year":2017},{"average_valuation":6800000000,"index":122,"industry":"Fintech","num_unicorns":20,"year":2019},{"average_valuation":1000000000,"index":123,"industry":"Travel","num_unicorns":1,"year":2022}],"schema":{"fields":[{"name":"index","type":"integer"},{"name":"industry","type":"string"},{"name":"year","type":"integer"},{"name":"num_unicorns","type":"integer"},{"name":"average_valuation","type":"number"}],"pandas_version":"1.4.0","primaryKey":["index"]}},"total_rows":124,"truncation_type":null}
```
:::
:::

::: {#25c63b29-3e08-4d5c-8f7d-63e45bc3c3d9 .cell .markdown}
### 3. Create two (2) CTEs for the two (2) tables above, then run the final query. {#3-create-two-2-ctes-for-the-two-2-tables-above-then-run-the-final-query}
:::

::: {#efd74d9c-f03e-4d4b-8678-a413656e77b9 .cell .code execution_count="6" customType="sql" dataFrameVariableName="df2" executionTime="1544" lastSuccessfullyExecutedCode="WITH top_industries AS (SELECT 
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
ORDER BY industry, year DESC;" sqlCellMode="dataFrame" sqlSource="{\"integrationId\":\"89e17161-a224-4a8a-846b-0adc0fe7a4b1\",\"type\":\"integration\"}"}
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

::: {.output .execute_result execution_count="6"}
``` json
{"table":{"data":[{"average_valuation_billions":2.47,"index":0,"industry":"E-commerce & direct-to-consumer","num_unicorns":47,"year":2021},{"average_valuation_billions":4,"index":1,"industry":"E-commerce & direct-to-consumer","num_unicorns":16,"year":2020},{"average_valuation_billions":2.58,"index":2,"industry":"E-commerce & direct-to-consumer","num_unicorns":12,"year":2019},{"average_valuation_billions":2.75,"index":3,"industry":"Fintech","num_unicorns":138,"year":2021},{"average_valuation_billions":4.33,"index":4,"industry":"Fintech","num_unicorns":15,"year":2020},{"average_valuation_billions":6.8,"index":5,"industry":"Fintech","num_unicorns":20,"year":2019},{"average_valuation_billions":2.15,"index":6,"industry":"Internet software & services","num_unicorns":119,"year":2021},{"average_valuation_billions":4.35,"index":7,"industry":"Internet software & services","num_unicorns":20,"year":2020},{"average_valuation_billions":4.23,"index":8,"industry":"Internet software & services","num_unicorns":13,"year":2019}],"schema":{"fields":[{"name":"index","type":"integer"},{"name":"industry","type":"string"},{"name":"year","type":"integer"},{"name":"num_unicorns","type":"integer"},{"name":"average_valuation_billions","type":"number"}],"pandas_version":"1.4.0","primaryKey":["index"]}},"total_rows":9,"truncation_type":null}
```
:::
:::
