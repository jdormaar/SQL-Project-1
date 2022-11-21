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

# Question 2: For each column within the database, determine the number of unique values:

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

| Column Name                | Distinct | NULLs | Definition of contained values:                                           |
| -------------------------- | -------- | ----- | ------------------------------------------------------------------------- |
| id                         | 15,134   | 0     | Unique serial id generated as PK with create table query                  |
| full_visitor_id            | 14223    | 0     | The unique visitor id                                                     |
| channel_grouping           | 7        | 0     | Default Channel Group per session View for the user                       |
| time,                      | 9600     | 0     | (hits.) From visit\*start*time to hit registered \_in ms*                 |
| country,                   | 136      | 0     | (geoNetwork.) Visitor's country by IP                                     |
| city,                      | 266      | 0     | (geoNetwork.) Visitor's city by IP or Geographical ID                     |
| total_transaction_revenue, | 72       | 15053 | (totals.) Total transaction revenue _multiplied by 10^6_                  |
| transactions,              | 1        | 15053 | (totals.) Number of ecommerce transactions from session record            |
| time_on_site,              | 1267     | 3300  | (totals.) Time of session _in seconds_                                    |
| page_views,                | 29       | 0     | (totals.) Number of page views from within the session                    |
| session_quality_dim,       | 45       | 13906 | (totals.) Quality estimate if 'close' or 'far' to transaction             |
|                            |          |       | Values near: 100:'close' 1:'far' 0:'not calculated'                       |
| date,                      | 366      | 0     | Date of session record                                                    |
| visit_id,                  | 14556    | 0     | Session id unique only to the user                                        |
| type,                      | 2        | 0     | (hits.) one of PAGE, TRANSACTION, ITEM, EVENT, SOCIAL, APPVIEW, EXCEPTION |
| product_refund_amount,     | NULL     | all   | _This column will be deleted_                                             |
| product_quantity,          | 8        | 15081 | (hits.prod.) Purchased product quantity                                   |
| product_price,             | 141      | 0     | (hits.prod.) Product price _multiplied by 10^6_                           |
| product_revenue,           | 4        | 15130 | (hits.prod.) Product revenue _multiplied by 10^6_                         |
| product_sku,               | 536      | 0     | (hits.prod.) ProductSKU                                                   |
| v2_product_name,           | 471      | 0     | (hits.prod.) Product Name                                                 |
| v2_product_category,       | 74       | 0     | (hits.prod.) Product Category                                             |
| product_variant,           | 11       | 0     | (hits.prod.) Product Variant                                              |
| currency_code,             | 1        | 272   | (hits.tr.) and (hits.item.) Currency code for transaction                 |
| item_quantity,             | NULL     | all   | _This column will be deleted_                                             |
| item_revenue               | NULL     | all   | _This column will be deleted_                                             |
| transaction_revenue,       | 4        | 15125 | (hits.tr.) Transaction revenue _multiplied by 10^6_                       |
| transaction_id,            | 9        | 15125 | (hits.tr.) Transaction id of the ecommerce transaction                    |
| page_title,                | 269      | 1     | (hits.page.)Title of Page                                                 |
| search_keyword,            | NULL     | all   | _Would have contained search word keywords - This column will be deleted_ |
| page_path_level1,          | 11       | 0     | (hits.page.) All the page paths rolled into the 1st hierarchical level    |
| ecommerce_action_type,     | 7        | 0     | (hits.) _see ecommerce_action_type list below:_                           |
| ecommerce_action_step,     | 3        | 0     | (hits.) Indicates step at checkout specific to hit                        |
| ecommerce_action_option    | 3        | 15103 | (hits.) Option selected at checkout (ex fedex)                            |

| ecommerce_action_type |
| --------------------- |

- 0 = Unknown

1.  = Click through of product lists
1.  = Product detail views
1.  = Add product(s) to cart
1.  = Remove product(s) from cart
1.  = Check out
1.  = Completed purchase
1.  = Refund of purchase
1.  = Checkout options

### TABLE 2: analytics: [4,301,122 rows, 15 columns]

| Column Name            | Distinct  | NULLs     | Definition of contained values:                          |
| ---------------------- | --------- | --------- | -------------------------------------------------------- |
| id                     | 4,301,122 | 0         | Unique serial id generated as PK with create table query |
| visit_number           | 222       | 0         | The session number                                       |
| visit_id               | 148,642   | 0         | The name of each product item.                           |
| visit_start_time       | 148,853   | 0         | Timestamp _as POSIX time format_                         |
| date                   | 93        | 0         | Date of the visit record                                 |
| full_visitor_id        | 120,018   | 0         | The unique visitor id                                    |
| user_id                | NULL      | all       | This column will be deleted                              |
| channel_grouping       | 8         | 0         | Default Channel Group per session View for the user      |
| social_engagement_type | 1         | 0         | This column will be deleted                              |
| units_sold             | 135       | 95,147    | The store's record of Units sold                         |
| page_views             | 129       | 72        | (totals.) Number of page views from within the session   |
| time_on_site           | 3270      | 477,465   | (totals.) _in seconds_                                   |
| bounces                | 1         | 3,826,283 | (totals.) A bounced session = 1, otherwise is null       |
| revenue                | 5270      | 4,285,767 | The store's record of revenue                            |
| unit_price             | 1442      | 0         | the store's record of unit price                         |

### TABLE 3: products: [1092 rows, 8 columns]

| Column Name          | Distinct | NULLs | Definition of contained values:                              |
| -------------------- | -------- | ----- | ------------------------------------------------------------ |
| id                   | 1092     | 0     | Unique serial id generated as PK with create table query     |
| sku                  | 1092     | 0     | Unique product id. _Auto-generated id above will be dropped_ |
| name                 | 313      | 0     | Product name and description                                 |
| ordered_quantity     | 224      | 0     | Ordered quantity                                             |
| stock_level          | 262      | 0     | Product stock level                                          |
| restocking_lead_time | 27       | 0     | Restock timing indicator _in days_                           |
| sentiment_score      | 17       | 1     |                                                              |
| sentiment_magnitude  | 20       | 1     |                                                              |

### TABLE 4: sales_by_sku: [462 rows, 3 columns]

| Column Name   | Distinct | NULLs | Definition of contained values:                              |
| ------------- | -------- | ----- | ------------------------------------------------------------ |
| id            | 462      | 0     | Unique serial id generated as PK with create table query     |
| product_sku   | 462      | 0     | Unique product id. _Auto-generated id above will be dropped_ |
| total_ordered | 60       | 0     | Total quantity of product ordered                            |

### TABLE 5: sales_report: [454 rows, 9 columns]

| Column Name          | Distinct | NULLs | Definition of contained values:                                |
| -------------------- | -------- | ----- | -------------------------------------------------------------- |
| id                   | 454      | 0     | Unique serial id generated as PK with create table query       |
| product_sku          | 454      | 0     | Unique product id. _Auto-generated id above might be dropped?_ |
| total_ordered        | 60       | 0     | Ordered quantity                                               |
| name                 | 237      | 0     | Product name and description                                   |
| stock_level          | 219      | 0     | Product stock level                                            |
| restocking_lead_time | 26       | 0     | Restock timing indicator _in days_                             |
| sentiment_score      | 16       | 0     |                                                                |
| sentiment_magnitude  | 20       | 0     |                                                                |
| ratio                | 245      | 78    |                                                                |

# Question 3: How many cities purchased orders within each country

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
