# Question 1: Missing values

## For each column within the database, assess the type and significance of missing values:

SQL Queries:

An example query of this series on full_visitor_id column for all_sessions below:

_SQL COUNT()_:

```sql
-- Evaluate NULL content:
SELECT COUNT(*)
FROM all_sessions
WHERE full_visitor_id IS NULL;
```

Sometimes the missing values were not formatted as null values. Below includes the string formatted versions of missing values from "city":

```sql
-- Missing values as 'not available in demo dataset' in city:
SELECT * FROM geo_visitor
WHERE city LIKE lower('%not available in demo dataset%')

-- Replace string alternates of missing values with null:
UPDATE geo_visitor
SET city = null
WHERE city LIKE lower('%not available in demo dataset%')

-- Assess the content for the city missing values as "not set":
SELECT * FROM geo_visitor
WHERE city LIKE lower('%not set%')

-- Replace string alternates of missing values with null:
UPDATE geo_visitor
SET city = null
WHERE city LIKE lower('%not set%')

-- Recheck city to confirm addressed missing values:
SELECT DISTINCT city, COUNT(city)
FROM geo_visitor
GROUP BY 1
ORDER BY 1 DESC
```

Similarly; below includes the string formatted missing values from "country"

```sql
-- Missing values 'not available in demo dataset' in country:
SELECT * FROM geo_visitor
WHERE country LIKE lower('%not set%')

-- Replace string alternates of missing values to null:
UPDATE geo_visitor
SET country = null
WHERE country LIKE lower('%not set%')

-- Recheck country to confirm addressed missing values
SELECT country, COUNT(country)
FROM geo_visitor
GROUP BY 1
ORDER BY 2 DESC
```

Answer:

- the query result answers are included in the tables below in question 2.
- The more qualitative observations however are more broad, and can be found throughout the cleaning_data.md document

# Question 2: Type and distribution of unique values:

## Determining the number of unique values for each column per table:

SQL Queries:

**Table shape** was determined with simple select statements for total rows.

_SQL SELECT_:

```sql
SELECT COUNT(id)
FROM all_sessions;
```

Count() function aggregate statements were used sequentially for an initial broad overview of the quantity, type, and quality of the data in each column. The results of these were populated into Tables 1-5 below, and also in discussion of cleaning_data.md.

An example query of this series on full_visitor_id column for all_sessions below:

_SQL DISTINCT and COUNT()_:

```sql
-- List of distinct value types, and their respective counts:
SELECT DISTINCT full_visitor_id, COUNT(full_visitor_id)
FROM all_sessions
GROUP BY 1
ORDER BY 2 DESC;
```

Answer:

### TABLE 1: all_sessions: [15,134 rows, 33 columns]

| Column Name                | Distinct | NULLs | Notes about missing values:              |
| -------------------------- | -------- | ----- | ---------------------------------------- |
| full_visitor_id            | 14223    | 0     |                                          |
| channel_grouping           | 7        | 0     |                                          |
| time,                      | 9600     | 0     | in ms                                    |
| country,                   | 136      | 0     | by IP                                    |
| city,                      | 266      | 0     | by IP or Geographical ID                 |
| total_transaction_revenue, | 72       | 15053 | _multiplied by 10^6_ null = 10^6?        |
| transactions,              | 1        | 15053 |                                          |
| time_on_site,              | 1267     | 3300  | _in seconds_ null = no session data?     |
| page_views,                | 29       | 0     |                                          |
| session_quality_dim,       | 45       | 13906 | 0:'not calculated'?                      |
| date,                      | 366      | 0     |                                          |
| visit_id,                  | 14556    | 0     | _Session id unique only to the user_     |
| type,                      | 2        | 0     |                                          |
| product_refund_amount,     | NULL     | all   | _This column was deleted_                |
| product_quantity,          | 8        | 15081 | nulls should = 0?                        |
| product_price,             | 141      | 0     | _multiplied by 10^6_ null = 10^6?        |
| product_revenue,           | 4        | 15130 | _multiplied by 10^6_ null = 10^6?        |
| product_sku,               | 536      | 0     |                                          |
| v2_product_name,           | 471      | 0     |                                          |
| v2_product_category,       | 74       | 0     |                                          |
| product_variant,           | 11       | 0     |                                          |
| currency_code,             | 1        | 272   | null = no purchase or "hit" transaction? |
| item_quantity,             | NULL     | all   | _This column was deleted_                |
| item_revenue               | NULL     | all   | _This column was deleted_                |
| transaction_revenue,       | 4        | 15125 | _multiplied by 10^6_ null = 10^6?        |
| transaction_id,            | 9        | 15125 |                                          |
| page_title,                | 269      | 1     |                                          |
| search_keyword,            | NULL     | all   | _This column was deleted_                |
| page_path_level1,          | 11       | 0     |                                          |
| ecommerce_action_type,     | 7        | 0     | 0 = unknown?                             |
| ecommerce_action_step,     | 3        | 0     |                                          |
| ecommerce_action_option    | 3        | 15103 |                                          |

### TABLE 2: analytics: [4,301,122 rows, 15 columns]

| Column Name            | Distinct | NULLs     | Notes about missing values: |
| ---------------------- | -------- | --------- | --------------------------- |
| visit_number           | 222      | 0         |                             |
| visit_id               | 148,642  | 0         |                             |
| visit_start_time       | 148,853  | 0         | _POSIX Timestamp_           |
| date                   | 93       | 0         |                             |
| full_visitor_id        | 120,018  | 0         |                             |
| user_id                | NULL     | all       | _This column was deleted_   |
| channel_grouping       | 8        | 0         |                             |
| social_engagement_type | 1        | 0         |                             |
| units_sold             | 135      | 95,147    |                             |
| page_views             | 129      | 72        |                             |
| time_on_site           | 3270     | 477,465   | _in seconds_                |
| bounces                | 1        | 3,826,283 | not bounced = null          |
| revenue                | 5270     | 4,285,767 |                             |
| unit_price             | 1442     | 0         |                             |

### TABLE 3: products: [1092 rows, 8 columns]

| Column Name          | Distinct | NULLs | Notes about missing values:         |
| -------------------- | -------- | ----- | ----------------------------------- |
| sku                  | 1092     | 0     | _Auto-generated id will be dropped_ |
| name                 | 313      | 0     |                                     |
| ordered_quantity     | 224      | 0     |                                     |
| stock_level          | 262      | 0     |                                     |
| restocking_lead_time | 27       | 0     |                                     |
| sentiment_score      | 17       | 1     |                                     |
| sentiment_magnitude  | 20       | 1     |                                     |

### TABLE 4: sales_by_sku: [462 rows, 3 columns]

| Column Name   | Distinct | NULLs | Notes about missing values: |
| ------------- | -------- | ----- | --------------------------- |
| product_sku   | 462      | 0     |                             |
| total_ordered | 60       | 0     |                             |

### TABLE 5: sales_report: [454 rows, 9 columns]

| Column Name          | Distinct | NULLs | Notes about missing values: |
| -------------------- | -------- | ----- | --------------------------- |
| product_sku          | 454      | 0     |                             |
| total_ordered        | 60       | 0     |                             |
| name                 | 237      | 0     |                             |
| stock_level          | 219      | 0     |                             |
| restocking_lead_time | 26       | 0     |                             |
| sentiment_score      | 16       | 0     |                             |
| sentiment_magnitude  | 20       | 0     |                             |
| ratio                | 245      | 78    |                             |

# Question 3: City representation per country:

SQL Queries:

```sql
-- Count then number of cities within givin country
SELECT country, COUNT(city) AS cities
FROM geo_visitor
WHERE city IS NOT NULL
GROUP BY 1
ORDER BY 2 DESC, 1;
```

Answer:
Ordered to display the countries with the highest number of cities for originating purchase orders.

| country                | cities |
| ---------------------- | ------ |
| "United States"        | 4513   |
| "India"                | 387    |
| "Canada"               | 218    |
| "United Kingdom"       | 208    |
| "Australia"            | 151    |
| "Japan"                | 72     |
| "France"               | 64     |
| "Ireland"              | 55     |
| "Hong Kong"            | 53     |
| "Israel"               | 47     |
| "Singapore"            | 46     |
| "Germany"              | 44     |
| "Spain"                | 44     |
| "Brazil"               | 42     |
| "Thailand"             | 36     |
| "Mexico"               | 35     |
| "Poland"               | 34     |
| "Turkey"               | 30     |
| "Indonesia"            | 28     |
| "Russia"               | 25     |
| "South Korea"          | 25     |
| "Switzerland"          | 24     |
| "Ukraine"              | 23     |
| "Colombia"             | 22     |
| "Philippines"          | 21     |
| "Sweden"               | 21     |
| "Malaysia"             | 20     |
| "Italy"                | 19     |
| "Romania"              | 18     |
| "Vietnam"              | 18     |
| "Argentina"            | 13     |
| "Netherlands"          | 13     |
| "United Arab Emirates" | 9      |
| "Greece"               | 8      |
| "Peru"                 | 8      |
| "Chile"                | 7      |
| "Czechia"              | 7      |
| "Hungary"              | 7      |
| "Sri Lanka"            | 6      |
| "Taiwan"               | 6      |
| "Austria"              | 5      |
| "Pakistan"             | 5      |
| "Belgium"              | 4      |
| "Croatia"              | 4      |
| "Saudi Arabia"         | 4      |
| "Venezuela"            | 4      |
| "Denmark"              | 3      |
| "South Africa"         | 3      |
| "Kenya"                | 2      |
| "New Zealand"          | 2      |
| "Uruguay"              | 2      |
| "China"                | 1      |
| "Egypt"                | 1      |
| "El Salvador"          | 1      |
| "Finland"              | 1      |
| "Jordan"               | 1      |
| "Lithuania"            | 1      |
| "Macedonia (FYROM)"    | 1      |
| "Norway"               | 1      |
| "Paraguay"             | 1      |
| "Portugal"             | 1      |
| "Qatar"                | 1      |
| "Serbia"               | 1      |
| "Slovakia"             | 1      |
