# D191 Database Report
JANUARY 11, 2023

## EER diagram for this project
![EER diagram for the project](./images/EER-diagram.png)

## My Environment for this project

- Operating system: Edition Windows 10 Education
- Installed RAM:  8.00 GB
- Processor:  Intel(R) Xeon(R) Gold 6230R CPU @ 2.10GHz   2.10 GHz
- System type:  64-bit operating system, x64-based processor
- Virtual machine located in New Port Richey, Florida

## Section A: Summarize one real-world business report that can be created from the attached Data Sets and Associated Dictionaries.

This report aims to analyze a DVD rental database to identify patterns in movie rental trends.

I am currently inquiring and examining the following business question: Which countries rent the highest number of movies by each category in the last 4 months? 

By using this report, the DVD business will be able to identify which countries have the highest demand for action movies. The report could potentially help the business allocate resources (DVDs) more efficiently.

### Question A1: Describe the data used for the report.
for my report i will use the following tables
- country of rental
- category of movie
- the number of times that a movie of that category was rented.

### Question A2: Identify two or more specific tables from the given dataset that will provide the data necessary for the detailed and the summary sections of the report.

I am going to include the following table in the detailed section of my report:
- rental
- inventory 
- film_category
- country
- film
- store
- category

I am going to include the following tables in the summary section of my report: 
- rental
- country
- category 

### Question A3: Identify the specific fields that will be included in the detailed and the summary sections of the report.

My detail table would include the following columns:  

1. inventory_id
2. Rental_date
3. Film_title
4. STORE_ID
5. COUNTRY_NAME. 

My summary table would include the following columns: 

1. Country 
2. Rental_count
3. Category_name

### Question A4: Identify one field in the detailed section that will require a custom transformation and explain why it should be transformed.

My transformation functions will transform the rental date in the detailed  tables to a more readable format e.g. `01-01-2023` to `1st Jan 23`.

### Question A5: Explain the different business uses of the detailed and the summary sections of the report.

The detailed table would be useful for analyzing the rental data in more depth and could be used to identify trends or patterns in the rental data. Using the detail table,  stakeholder can see exactly which movies are being rented in each country.

The summary table would allow the stakeholder to see the total number of movie rentals by country and category and would be useful for quickly identifying the countries that rent the highest number of movies by each category. A Stakeholder could use the summary table to see which countries have the highest number of movie rentals overall, and which categories are the most popular in each country.

### Question A6: Explain how frequently your report should be refreshed to remain relevant to stakeholders. 

To remain relevant to stakeholders, data in the report should be updated on a quarterly basis. This will provide insight on how well the company is managing its inventory and allow for informed decision-making on which movies to bring in the next quarter. For example, if data shows that there is a high demand for action movies in Canada and a high demand for classical movies in Australia in one quarter, the company can adjust its inventory accordingly by moving some action movies from Canada to Australia and classical movies from Australia to Canada.


### Based on the current dataset perform aggregation that can be utilized to address the business question mentioned in Question A1
In part A1, I was examining the following business question: Which countries rent the highest number of movies by each category in the last quarter? 

we can use this sql statement to sort the transaction by category in each country in 2005
```sql 

SELECT
    country.country AS Country,
    category.name AS Film_category,
    COUNT(DISTINCT film.film_id) AS Movies_rented
FROM
    rental
INNER JOIN
    inventory ON rental.inventory_id = inventory.inventory_id
INNER JOIN
    film ON inventory.film_id = film.film_id
INNER JOIN
    film_category ON film.film_id = film_category.film_id
INNER JOIN
    category ON film_category.category_id = category.category_id
INNER JOIN
    store ON inventory.store_id = store.store_id
INNER JOIN
    address ON store.address_id = address.address_id
INNER JOIN
    city ON address.city_id = city.city_id
INNER JOIN
    country ON city.country_id = country.country_id
WHERE
    EXTRACT(YEAR FROM rental.rental_date) = 2005
GROUP BY
    country.country, category.name
ORDER BY
   Movies_rented desc;

 ```


#### results:
| Film_category | Australia | Canada |
|---------------|-----------|--------|
| Documentary   | 600       | 450    |
| Sports        | 624       | 555    |
| Animation     | 598       | 568    |
| Drama         | 484       | 576    |
| Sci-Fi        | 580       | 521    |
| Family        | 539       | 557    |
| Foreign       | 509       | 524    |
| Action        | 516       | 596    |
| Games         | 514       | 455    |
| Children      | 492       | 453    |
| Classics      | 492       | 447    |
| Horror        | 460       | 386    |
| Travel        | 442       | 395    |
| Comedy        | 439       | 502    |
| New           | 438       | 502    |
| Music         | 394       | 436    |

![](./images/bar-graph.png)


#### Analysis of the data 
While the data provided from 2005 is too dated and too small to make precise future predictions, it can still offer valuable insights into what the business might have considered.

Based on the rental data, it's apparent that the DVD rental business needs to evaluate its approach to the Horror and Travel categories in Canada and the Music category in Australia, as their rental transactions stay below 400 transactions, indicating potentially declining demand. 

To adapt to market trends and customer preferences, the business should consider updating its offerings in these categories or exploring strategies to boost rentals like expanding Movie Selection, and Promotional Campaigns. Simultaneously, it's crucial to identify and focus on high-demand categories to maintain a competitive edge. 

To remain competitive in the market, the DVD rental business should prioritize aligning its inventory with customer preferences by emphasizing popular genres. This strategic move can enhance its relevance and appeal to a broader audience, fostering increased rentals and sustained market presence. In Australia, focusing on high-demand genres such as Sports and Documentary can help boost rental numbers. In Canada, prioritizing genres like Action and Drama is likely to maximize rental potential.

There is a wide gap between now and when data was captured. The current market for DVD rental had been experiencing a continuing decline in favor of digital streaming services. The rise of on-demand streaming platforms has led to a substantial decline in the demand for physical DVD rentals. This shift has resulted in the closure of numerous brick-and-mortar DVD rental stores, with some businesses transitioning their focus to digital platforms.

### Provide the code necessary to test the database and ensure that the information within it remains current to the analysis or inquiry you are currently undertaking?

```sql 
INSERT INTO rental(
        rental_date, inventory_id, customer_id, return_date, staff_id, last_update)
        VALUES ( CURRENT_DATE - INTERVAL '5 days', 25, 27, null , 1,now()),
              ( CURRENT_DATE - INTERVAL '2 days', 2, 10, null , 1,now()),
              ( CURRENT_DATE - INTERVAL '3 months', 15, 17, null , 1,now()),
              ( CURRENT_DATE - INTERVAL '2 months', 35, 37, null , 1,now()),
              ( CURRENT_DATE - INTERVAL '1 months', 55, 47, null , 1,now());
``` 

```sql
select * from rental  WHERE date_part('year', rental_date)>=2022
```

By introducing random values, I aim to make the data pertinent and reflective of the current date. This means that the information or statistics I'm incorporating will be up-to-date and applicable to the present time. It ensures that the data accurately represents the current conditions, trends, or variables that are relevant to the context of the analysis or inquiry I am conducting. The randomness of these values allows for a dynamic and realistic representation, which can be crucial when making decisions or drawing conclusions based on the data.





## Section B: Write SQL code that creates the tables to hold your report sections.
These tables will hold values that will be important for the stakeholder to efficiently manage and analyze rental data. The "summary" table provides a high-level overview, allowing the stakeholder to see rental counts categorized by country and film category while ensuring that the combination of country and category remains unique. On the other hand, the "detailed" table contains more granular data, including rental dates, inventory information, film titles, film categories, and countries. This detailed information can facilitate in-depth analysis and reporting, enabling the stakeholder to gain insights into specific rental transactions and trends.


```sql 

CREATE TABLE summary (
 Summary_id Serial Primary key, 
  Country VARCHAR(255),
  Rental_count INTEGER,
  Category_name VARCHAR(255)
);
-- make the country, and category name unique 
ALTER TABLE summary ADD UNIQUE (Country, Category_name);

-- SQL code for creating the detailed table
CREATE TABLE detailed (
  Detail_id SERIAL Primary key, 
  Rental_DATE DATE,
  Inventory_id INTEGER,
  film_title VARCHAR(45),
  Film_category VARCHAR(45),
  Country VARCHAR(60)
);
```

![Confirmation of sucessful query](./images/Section-B1.png)
![DATA](./images/Section-B2.png)

## Section C: Write a SQL query that will extract the raw data needed for the Detailed section of your report from the source database and verify the data’s accuracy.

```sql
INSERT INTO detailed (Rental_DATE, Inventory_id, film_title, Film_category, Country)
SELECT rental.rental_date,inventory.inventory_id, film.title ,category.name as Film_category, country.country as Country
FROM rental  
INNER JOIN inventory ON rental.inventory_id = inventory.inventory_id  
INNER JOIN film on inventory.film_id= film.film_id  
INNER JOIN film_category on film.film_id=film_category.film_id  
INNER JOIN category on film_category.category_id= category.category_id  
INNER JOIN store ON inventory.store_id = store.store_id  
INNER JOIN address ON store.address_id = address.address_id  
INNER JOIN city ON city.city_id = address.city_id  
INNER JOIN country ON country.country_id = city.country_id
WHERE rental.rental_date BETWEEN (now() - INTERVAL '4 months') AND now()
GROUP BY Country,  category.name,  rental.rental_id,  inventory.inventory_id,  film.film_id,  
film_category,  category.name,  country.country;
```

![Confirmation of sucessful query](./images/Section-C1.png)
![DATA](./images/Section-C2.png)


## Section D: Write code for function(s) that perform the transformation(s) you identified in Seciton A
```sql 

CREATE OR REPLACE FUNCTION convert_date(timestamp)
RETURNS text AS
$$
SELECT to_char($1, 'FMDDth FMMonth YYYY');
$$ LANGUAGE SQL;
-- testing Function
SELECT convert_date(Rental_DATE) from detailed;

```
![Confirmation of sucessful query](./images/Section-D1.png)
![DATA](./images/Section-D2.png)

## Section E Trigger: Write a SQL code that creates a trigger on the detailed table of the report that will continually update the summary table as data is added to the detailed table. 
```sql 
CREATE OR REPLACE FUNCTION update_summary_table()
  RETURNS TRIGGER AS $$
  BEGIN
    INSERT INTO summary (Country, Rental_count, Category_name)
      SELECT  country,Count(rental_date) as rental_count, film_category
  FROM detailed
    WHERE NEW.rental_date IS NOT NULL
  GROUP BY country, film_category
    ON CONFLICT (Country, Category_name) DO UPDATE SET Rental_count =  EXCLUDED.Rental_count;
  RETURN NEW;
  END;
    $$ LANGUAGE PLPGSQL; 

 CREATE TRIGGER update_summary_trigger
 AFTER INSERT OR UPDATE OR DELETE ON detailed
 FOR EACH ROW
 EXECUTE PROCEDURE update_summary_table();
-- Testing Trigger: lets make a fictitious country called 'Qube' pronounced cube 
  DELETE from detailed where country='Qube'; 
  INSERT INTO detailed(rental_date, inventory_id, film_category, country) VALUES (now(), 5, 'action' , 'Qube'),(now(), 7, 'action' , 'Qube'),(now(), 8, 'film','Qube');
  SELECT * from summary where country='Qube';

-- Must return Qube  | 2 |action
--             Qube  | 1 | film 

```
 
![Confirmation of sucessful query](./images/Section-E1.png)

## Section F: Create a stored procedure that can be used to refresh the data in both your detailed and summary tables. The procedure should clear the contents of the detailed and summary tables and perform the ETL load process from part C and include comments that identify how often the stored procedure should be executed.

```sql
CREATE OR REPLACE PROCEDURE refresh_data()
LANGUAGE plpgsql
AS $$
BEGIN;
  TRUNCATE TABLE detailed;
  INSERT INTO detailed (Rental_DATE, Inventory_id, film_title, Film_category, Country)
    SELECT rental.rental_date,inventory.inventory_id, film.title ,category.name as Film_category, 
    country.country as Country
    FROM rental  
    INNER JOIN inventory ON rental.inventory_id = inventory.inventory_id  
    INNER JOIN film on inventory.film_id= film.film_id  
    INNER JOIN film_category on film.film_id=film_category.film_id  
    INNER JOIN category on film_category.category_id= category.category_id  
    INNER JOIN store ON inventory.store_id = store.store_id  
    INNER JOIN address ON store.address_id = address.address_id  
    INNER JOIN city ON city.city_id = address.city_id  
    INNER JOIN country ON country.country_id = city.country_id 
    WHERE rental.rental_date BETWEEN (now() - INTERVAL '4 month') AND now()
    GROUP BY Country,  category.name,  rental.rental_id,  inventory.inventory_id,  film.film_id,  film_category,  category.name,  country.country;  
 -- Truncate the summary tables
  TRUNCATE TABLE summary;
-- Load data into the summary table
  INSERT INTO summary (Country, Rental_count, Category_name)
  SELECT country, COUNT(rental_date), film_category
  FROM detailed
  GROUP BY country, film_category
ON CONFLICT DO NOTHING;

END;
$$;


```

![Confirmation of sucessful query](./images/Section-F1.png)
![CALL](./images/Section-F2.png)

### F1. Running automatically: Explain how the stored procedure can be run on a schedule to ensure data freshness.
In order to automatically execute the stored procedure once every 4 months we can use a tool called pg-agent.

1. Install and configure pgAgent using the command 
```sql 
CREATE EXTENSION pgagent
```


1. Create a new job in pgAgent by connecting to the database and running the command

```sql 
INSERT INTO pgagent.pga_job (jobname, jagent) VALUES 
('refresh_data','refresh_data();');

```

1. Define a schedule: To run the job on a schedule, we need to define a schedule for it. In this case, we want the job to run every four months.


```sql 

INSERT INTO pgagent.pga_schedule (sname, sjobno, sinterval, snexttime, sstatus)
VALUES ('four_months', (SELECT jobid FROM pgagent.pga_job WHERE jobname = 'refresh_data'), '4 month', NOW(), 'r');

```


1. Use the following command to activate the pgAgent, so it can perform the scheduled task. 

```sql
SELECT pgagent.pga_start();
 ```




