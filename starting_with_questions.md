## Question 1

Which cities and countries have the highest level of transaction revenues on the site?

### SQL QUERIES:

#### Countries with the top five highest total transaction revenues:

```sql
-- Sum transaction levels by country
SELECT
    g.country
  , SUM(ROUND(total_transaction_revenue)) AS ttr
FROM product_hits
JOIN transaction_hits tr USING (alls_id)
JOIN geo_visitor g USING (alls_id)
WHERE tr.total_transaction_revenue IS NOT NULL
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

output:

```
| country       | total_transaction_revenue |
| ------------- | ------------------------- |
| United States | 13153                     |
| Israel        | 602                       |
| Australia     | 358                       |
| Canada        | 150                       |
| Switzerland   | 17                        |
```

#### Cities with the top five highest total transaction revenues:

```sql
-- Sum transaction levels by city
SELECT
    g.city
  ,	g.country
  , SUM(ROUND(total_transaction_revenue)) AS ttr
FROM product_hits
JOIN transaction_hits tr USING (alls_id)
JOIN geo_visitor g USING (alls_id)
WHERE tr.total_transaction_revenue IS NOT NULL
AND city IS NOT NULL
GROUP BY 1,2
ORDER BY 3 DESC
LIMIT 5;
```

output:

```
| city          | country       | total_transaction_revenue |
| ------------- | ------------- | ------------------------- |
| San Francisco | United States | 1564                      |
| Sunnyvale     | United States | 992                       |
| Atlanta       | United States | 854                       |
| Palo Alto     | United States | 608                       |
| Tel Aviv-Yafo | Israel        | 602                       |
```

## Question 2

What is the average number of products ordered from visitors in each city and country?

### SQL QUERIES:

#### Average number of products ordered, for each country:

```sql
-- Average number of products ordered by country
SELECT
    g.country
  , ROUND(AVG(product_quantity)) AS country_quantity
FROM product_hits
JOIN geo_visitor as g USING (alls_id)
WHERE product_quantity IS NOT NULL
GROUP BY g.country
ORDER BY country_quantity DESC;
```

Average number of products ordered by country:

| country       | country_quantity |
| ------------- | ---------------- |
| Spain         | 10               |
| United States | 4                |
| Colombia      | 1                |
| Finland       | 1                |
| France        | 1                |
| Argentina     | 1                |
| Ireland       | 1                |
| Mexico        | 1                |
| India         | 1                |
| Canada        | 1                |

#### Average number of products ordered, for each city:

```sql
-- Average number of products ordered by city

SELECT
    city
  , g.country
  , ROUND(AVG(product_quantity)) AS city_quantity
FROM product_hits
JOIN geo_visitor as g USING (alls_id)
WHERE product_quantity IS NOT NULL
  AND city IS NOT NULL
GROUP BY g.city, g.country
ORDER BY city_quantity DESC;
```

Average number of products ordered by city:

| city          | country       | city_quantity |
| ------------- | ------------- | ------------- |
| Madrid        | Spain         | 10            |
| Salem         | United States | 8             |
| Atlanta       | United States | 4             |
| Houston       | United States | 2             |
| Dallas        | United States | 1             |
| Detroit       | United States | 1             |
| Dublin        | Ireland       | 1             |
| Los Angeles   | United States | 1             |
| Mountain View | United States | 1             |
| New York      | United States | 1             |
| Palo Alto     | United States | 1             |
| San Francisco | United States | 1             |
| San Jose      | United States | 1             |
| Seattle       | United States | 1             |
| Ann Arbor     | United States | 1             |
| Sunnyvale     | United States | 1             |
| Bengaluru     | India         | 1             |
| Chicago       | United States | 1             |
| Columbus      | United States | 1             |

## Question 3

Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

### SQL Queries:

```sql
SELECT
    g.city
  , g.country
  , v2_product_category
  , v2_product_name
  , sub_cat_3
  , sub_cat_1
  , sub_cat_2
FROM products_category
JOIN geo_visitor as g USING (alls_id)
WHERE product_quantity IS NOT NULL
  AND g.city IS NOT NULL
GROUP BY g.city, g.country, v2_product_category, v2_product_name, sub_cat_1 , sub_cat_2 , sub_cat_3
ORDER BY g.city DESC;
```
