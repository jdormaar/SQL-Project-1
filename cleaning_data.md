# Cleaning Data

### Lighthouse Labs, Final SQL Project.

##### November 21, 2022. Jamie Dormaar

---

## Data Cleaning: Intro

The primary goal of data cleaning is to learn everything we can about the information contained within the database. What Information does it have, How is it interconnected? Are there trends? Gaps? Where did it come from? Does it contain any innately meaningful information? What can be derived or inferred from this? How much of the content is junk?

### Issues that will be addressed by cleaning the data(6)?

- `Consistent naming `conventions for tables and columns
- Identify `irrelevant` and `duplicate` data for removal
- `Type conversion` for improper data types
- Syntax `errors` and `typos`
- `Find` missing values that should b filled in to complete the dataset
- `Identify` outliers for removal to prevent their interference during analysis
- Handle `missing data`
- Filter out data `outliers`
- `Fix` dates and times
- `Merge` and `split` columns
- Reconcile table data by `adding relationships`
- `Validate` data to ready it for data transformation and analysis
- Perform `transformations` and `analysis`

## Discussion:

(Did we win?) The primary goals of coming to understand the data were achieved. (answers to the rambling bungle of questions above can be breifly summarized here...)

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

## Methods and Procedure: Queries

.....(and this is how I did it)

### STEP 1: Getting Oriented with the Data.

#### Title and Naming consistency:

Consistent naming conventions were established first by changing the column names to match the format of the tables names. Snake case is the simplest format for working with PGAdmin, and so it was chosen when creating the table titles. An example query of how column names were changed is given for the full_visitor_id column (was previously fullVisitorId) for all_sessions below:

_SQL ALTER TABLE - RENAME COLUMN_:

```sql
-- Rename column headings for consistency to snake case.
ALTER TABLE public.all_sessions
RENAME COLUMN fullVisitorId TO full_visitor_id;
```

#### Number of unique/duplicate values:

Table shape was determined with simple select statements for total rows. The resulting values plus the known number of columns title Tables 1-5 in Discussion.

_SQL SELECT_:

```sql
SELECT COUNT(id)
FROM all_sessions;
```

Count() function aggregate statements were used sequentially for an initial broad overview of the quantity, type, and quality of the data in each column. The results of these were populated into Tables 1-5 in Discussion. An example query of this series on full_visitor_id column for all_sessions below:

_SQL DISTINCT and COUNT()_:

```sql
-- List of distinct value types, and their respective counts:
SELECT DISTINCT full_visitor_id, COUNT(full_visitor_id)
FROM all_sessions
GROUP BY 1
ORDER BY 2 DESC;

-- Evaluate NULL content:
SELECT COUNT(*)
FROM all_sessions
WHERE full_visitor_id IS NULL;
```

#### Create (Temp?) new table:

```sql

```

#### Query for full table row duplications:

Finding full row duplications in a table:
_SQL WITH_

```sql
-- Row duplications in all_sessions:
WITH dup_rows AS (
	SELECT full_visitor_id
		, visit_id
-- 		, product_sku
		, COUNT(*) AS num_rows
	FROM all_sessions
	GROUP BY 1,2
	-- ,3
	HAVING COUNT(*) > 1)

SELECT *
FROM all_sessions al
JOIN dup_rows d
ON al.full_visitor_id = d.full_visitor_id
AND al.visit_id = d.visit_id
-- AND al.product_sku = d.product_sku

-- Returns 459 rows.
-- Uncommenting the lines in the function Returns 0 row duplications
```

NOTE: `all_sessions` appeared to have 459 counted duplications when queried on the paired sets of `full_visitor_id`, `visit_id`.

Adding the third `product_sku` value to the matching sets however returned no duplications.

It is most likely this implies that there are 459 instances of user generated product relevant data, involving > 1 product per visit. Therefore the contents of this table should further broken down into more smaller relatable tables.

```sql
-- Row duplications in analytics
WITH dup_rows AS (
	SELECT visit_id
		, full_visitor_id
		, COUNT(*) AS num_rows
	FROM analytics an
	GROUP BY 1,2
	HAVING COUNT(*) > 1)

SELECT *
FROM analytics an
JOIN dup_rows d
ON an.full_visitor_id = d.full_visitor_id
AND an.visit_id = d.visit_id
-- Current parameters appear to show 4,298,949 of all 4,301,122 rows are duplicated.

WITH dup_rows AS (
	SELECT
		visit_number,
		visit_id,
		visit_start_time,
		date,
		full_visitor_id,
		user_id,
		channel_grouping,
		social_engagement_type,
		units_sold,
		page_views,
		time_on_site,
		bounces,
		revenue,
		unit_price,
	COUNT(*) AS num_rows -- Count # rows with ALL = columns:
	FROM analytics an
	GROUP BY 1,2,3,4,5,6,7,8,9,10,11,12,13,14
	HAVING COUNT(*) > 1
)
	AS num_unique
FROM analytics an
-- Returns 870577 values (~20%)

-- Further inquiry required.
```

---

ERD for end of Part 1

![]()

---

### STEP 2: Cleaning the Issues Found in Orientation.

#### Drop irrelevant or empty columns:

There were four empty columns identified in all_sessions

_SQL ALTER TABLE - DROP COLUMN_

```sql
ALTER TABLE all_sessions
  DROP COLUMN product_refund_amount;

ALTER TABLE all_sessions
  DROP COLUMN item_quantity;

ALTER TABLE all_sessions
  DROP COLUMN item_revenue;

ALTER TABLE all_sessions
  DROP COLUMN search_keyword;

-- Confirm columns dropped successfully.
SELECT * FROM all_sessions;
```

---

#### Breaking up all_sessions into smaller relatable tables:

```sql
SELECT CAST((id +900000 )AS INT) AS id
	, CAST(id AS INT) AS alls_id
	, page_title
	, page_path_level1
FROM all_sessions
```

```sql
-- Create new table for page analytic data:
CREATE TABLE page_analytics(
    id INT
	, alls_id INT
    , page_title VARCHAR(1000)
    , page_path_level1 VARCHAR(50)
    , PRIMARY KEY (id)
    , FOREIGN KEY (alls_id)
    REFERENCES all_sessions (id)
);

COPY page_analytics(
    id
	, alls_id
    , page_title
    , page_path_level1
)
FROM '/Users/Shared/page_analytics.csv'
DELIMITER ','
CSV HEADER;
```

Confirm data safely secured elsewhere:

```sql
-- Call new table page_analytics
SELECT *
FROM page_analytics

-- Remove displaced columns from all_sessions table
ALTER TABLE all_sessions
  DROP COLUMN page_title;

ALTER TABLE all_sessions
  DROP COLUMN page_path_level1;
```

---

```sql
-- New table: geo_vistor.  Export .csv
SELECT CAST((id +800000 )AS INT) AS geo_id
	, city
	, country
	, CAST(id AS INT) AS alls_id
FROM all_sessions

-- Create new table:
CREATE TABLE geo_vistor(
    geo_id INT
    , city VARCHAR(100)
    , country VARCHAR(50)
    , alls_id INT
    , PRIMARY KEY (geo_id)
    , FOREIGN KEY (alls_id)
    REFERENCES all_sessions (id)
);

COPY geo_visitor(
  geo_id
  , city
  , country
  , alls_id
)
FROM '/Users/Shared/geo_visitor.csv'
DELIMITER ','
CSV HEADER;
```

```sql
-- Call new table geo_visitor
SELECT *
FROM geo_visitor

-- Remove displaced columns from all_sessions table
Confirm data is secured:
ALTER TABLE all_sessions
  DROP COLUMN city;

ALTER TABLE all_sessions
  DROP COLUMN country;
```

---

```SQL
-- New table: page_analytics.  Export .csv
SELECT
    CAST((id + 700000)AS INT) AS ecommerce_id
	, ecommerce_action_type
	, ecommerce_action_step
    , ecommerce_action_option
	, CAST(id AS INT) AS alls_id
    , CAST((id + 900000)AS INT) AS page_id
FROM all_sessions;


-- Create new table:
CREATE TABLE ecommerce_hits(
    ecommerce_id INT
    , ecommerce_action_type SMALLINT
    , ecommerce_action_step SMALLINT
    , ecommerce_action_option VARCHAR(50)
    , alls_id INT
    , page_id INT
    , PRIMARY KEY (ecommerce_id)
    , FOREIGN KEY (alls_id)
    REFERENCES all_sessions (id)
    , FOREIGN KEY (page_id)
    REFERENCES page_analytics (id)
);

COPY ecommerce_hits(
  ecommerce_id
  , ecommerce_action_type
  , ecommerce_action_step
  , ecommerce_action_option
  , alls_id
  , page_id
)
FROM '/Users/Shared/ecommerce_hits.csv'
DELIMITER ','
CSV HEADER;

-- Call new table ecommerce_hits
SELECT * FROM ecommerce_hits;

-- Drop columns from all_sessions
ALTER TABLE all_sessions
  DROP COLUMN ecommerce_action_type;

ALTER TABLE all_sessions
  DROP COLUMN ecommerce_action_step;

ALTER TABLE all_sessions
  DROP COLUMN ecommerce_action_option;
```

---

```SQL
SELECT
    CAST((id + 600000)AS INT) AS category_id
    , product_price
    , product_sku
    , v2_product_name
    , v2_product_category
    , product_variant
    , CAST(id AS INT) AS alls_id
    , CAST((id + 700000)AS INT) AS ecommerce_id
FROM all_sessions;


-- Create new table:
CREATE TABLE category(
    category_id INT
    , product_price BIGINT
    , product_sku VARCHAR(50)
    , v2_product_name VARCHAR(100)
    , v2_product_category VARCHAR(100)
    , product_variant VARCHAR(50)
	, alls_id INT
	, ecommerce_id INT
    , PRIMARY KEY (category_id)
    , FOREIGN KEY (alls_id)
	REFERENCES all_sessions (id)
	, FOREIGN KEY (ecommerce_id)
    REFERENCES ecommerce_hits (ecommerce_id)
);

COPY category(
  category_id
  , product_price
  , product_sku
  , v2_product_name
  , v2_product_category
  , product_variant
  , alls_id
  , ecommerce_id
)
FROM '/Users/Shared/category.csv'
DELIMITER ','
CSV HEADER;

-- Call new table ecommerce_hits
SELECT * FROM category;

-- Drop columns from all_sessions
ALTER TABLE all_sessions
  DROP COLUMN v2_product_name;

ALTER TABLE all_sessions
  DROP COLUMN v2_product_category;

ALTER TABLE all_sessions
  DROP COLUMN product_variant;
```

### RESULTING LIST OF RELATABLE TABLES:

#### TABLE 6: all_sessions: [15,134 rows, 7 original columns]

| Column Name      | Distinct | NULLs | Definition of contained values:                          |
| ---------------- | -------- | ----- | -------------------------------------------------------- |
| id               | 15,134   | 0     | Unique serial id generated as PK with create table query |
| full_visitor_id  | 14223    | 0     | The unique visitor id                                    |
| channel_grouping | 7        | 0     | Default Channel Group per session View for the user      |
|                  |          |       | Values near: 100:'close' 1:'far' 0:'not calculated'      |
| date,            | 366      | 0     | Date of session record                                   |
| visit_id,        | 14556    | 0     | Session id unique only to the user                       |

#### TABLE 7: product_hits: [15,134 rows, 4 original columns]

| Column Name       | Distinct | NULLs | Definition of contained values:                     |
| ----------------- | -------- | ----- | --------------------------------------------------- |
| product_quantity, | 8        | 15081 | (hits.prod.) Purchased product quantity             |
| product_price,    | 141      | 0     | (hits.prod.) Product price **multiplied by 10^6**   |
| product_revenue,  | 4        | 15130 | (hits.prod.) Product revenue **multiplied by 10^6** |
| product_sku,      | 536      | 0     | (hits.prod.) ProductSKU                             |

#### TABLE 8: category: [15,134 rows, 5 original columns]

| Column Name          | Distinct | NULLs | Definition of contained values: |
| -------------------- | -------- | ----- | ------------------------------- |
| product_sku,         | 536      | 0     | (hits.prod.) ProductSKU         |
| v2_product_name,     | 471      | 0     | (hits.prod.) Product Name       |
| v2_product_category, | 74       | 0     | (hits.prod.) Product Category   |
| product_variant,     | 11       | 0     | (hits.prod.) Product Variant    |

#### TABLE 9: ecommerce_hits: [15,134 rows, 3 original columns]

| Column Name             | Distinct | NULLs | Definition of contained values:                    |
| ----------------------- | -------- | ----- | -------------------------------------------------- |
| ecommerce_action_type,  | 7        | 0     | (hits.) _see ecommerce_action_type list below:_    |
| ecommerce_action_step,  | 3        | 0     | (hits.) Indicates step at checkout specific to hit |
| ecommerce_action_option | 3        | 15103 | (hits.) Option selected at checkout (ex fedex)     |

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

#### TABLE 10: transaction_hits: [15,134 rows, 4 original columns]

| Column Name                | Distinct | NULLs | Definition of contained values:                              |
| -------------------------- | -------- | ----- | ------------------------------------------------------------ |
| currency_code,             | 1        | 272   | (hits.tr.) and (hits.item.) Currency code for transaction    |
| transaction_revenue,       | 4        | 15125 | (hits.tr.) Transaction revenue **multiplied by 10^6**        |
| total_transaction_revenue, | 72       | 15053 | (totals.) Total transaction revenue _**multiplied by 10^6**_ |
| transaction_id,            | 9        | 15125 | (hits.tr.) Transaction id of the ecommerce transaction       |

#### TABLE 11: totals: [15,134 rows, 5 original columns]

| Column Name                | Distinct | NULLs | Definition of contained values:                                |
| -------------------------- | -------- | ----- | -------------------------------------------------------------- |
| total_transaction_revenue, | 72       | 15053 | (totals.) Total transaction revenue _**multiplied by 10^6**_   |
| transactions,              | 1        | 15053 | (totals.) Number of ecommerce transactions from session record |
| time_on_site,              | 1267     | 3300  | (totals.) Time of session _**in seconds**_                     |
| page_views,                | 29       | 0     | (totals.) Number of page views from within the session         |
| session_quality_dim,       | 45       | 13906 | (totals.) Quality estimate if 'close' or 'far' to transaction  |

#### TABLE 12: page_analytics: [15,134 rows, 2 original columns]

| Column Name       | Distinct | NULLs | Definition of contained values:                                        |
| ----------------- | -------- | ----- | ---------------------------------------------------------------------- |
| page_title,       | 269      | 1     | (hits.page.)Title of Page                                              |
| page_path_level1, | 11       | 0     | (hits.page.) All the page paths rolled into the 1st hierarchical level |

#### TABLE 13: geo_visitor: [15,134 rows, 2 original columns]

| Column Name | Distinct | NULLs | Definition of contained values:                       |
| ----------- | -------- | ----- | ----------------------------------------------------- |
| city,       | 266      | 0     | (geoNetwork.) Visitor's city by IP or Geographical ID |
| country,    | 136      | 0     | (geoNetwork.) Visitor's country by IP                 |

#### TABLE 14: hits: [15,134 rows, 2 original columns]

| Column Name | Distinct | NULLs | Definition of contained values:                                           |
| ----------- | -------- | ----- | ------------------------------------------------------------------------- |
| type,       | 2        | 0     | (hits.) one of PAGE, TRANSACTION, ITEM, EVENT, SOCIAL, APPVIEW, EXCEPTION |
| time,       | 9600     | 0     | (hits.) From visit\*start*time to hit registered \_in ms*                 |

### DATA TRANSFORMATIONS: transaction_hits TABLE

Saving query results to csv formats null values as string 'NULL' which were cleaned by the following:

##### COLUMN transaction_revenue:

_SQL UPDATE - SET_

_SQL ALTER TABLE - COLUMN_

```SQL
-- Remove string format 'NULL' values:
UPDATE transaction_hits
SET transaction_revenue = 0
WHERE transaction_revenue LIKE upper('%NULL%')

-- Cast column type to integer again:
ALTER TABLE transaction_hits
ALTER COLUMN transaction_revenue TYPE NUMERIC
USING transaction_revenue::numeric;
```

Since the BigQuery Export schema produces financial values with a 10^6 multiplier, all financial values were normalized by dividing this value back out again.

```SQL
UPDATE transaction_hits
SET transaction_revenue = transaction_revenue * 0.000001;

-- Confirm results:
SELECT *
FROM transaction_hits
```

##### COLUMN transaction_id:

```sql
UPDATE transaction_hits
SET transaction_id = null
WHERE transaction_id LIKE upper('%NULL%')

-- Confirm results:
SELECT *
FROM transaction_hits
```

##### COLUMN total_transaction_revenue:

```sql
-- Fix string formated 'NULL' values:
UPDATE transaction_hits
SET total_transaction_revenue = null
WHERE total_transaction_revenue LIKE upper('%NULL%')

-- Cast column type to integer again:
ALTER TABLE transaction_hits
ALTER COLUMN total_transaction_revenue TYPE NUMERIC
USING total_transaction_revenue::numeric;

-- Adjust for the Data Results financial multiplier of 10^6:
UPDATE transaction_hits
SET total_transaction_revenue = total_transaction_revenue * 0.000001

-- Confirm results:
SELECT *
FROM transaction_hits
```

##### COLUMN currency_code:

```sql
-- Fix string formated 'NULL' values:
UPDATE transaction_hits
SET currency_code = null
WHERE currency_code LIKE upper('%NULL%')

-- Confirm results:
SELECT COUNT(*)
FROM transaction_hits
WHERE currency_code IS NULL
-- Returns 272 null value count again.
```

#### DATA TYPE CONVERSIONS: product_hits TABLE

As per the Table 7 for the following cleaning requirements:

##### COLUMN product_quantity:

```sql
-- Remove string converted "NULL" values:
UPDATE product_hits
SET product_quantity = null
WHERE product_quantity LIKE upper('%NULL%')

-- Cast column type to numeric:
ALTER TABLE product_hits
ALTER COLUMN product_quantity TYPE NUMERIC
USING product_quantity::numeric;

-- Confirm results:
SELECT *
FROM product_hits
```

##### COLUMN product_price:

```sql
-- Cast column type to numeric:
ALTER TABLE product_hits
ALTER COLUMN product_price TYPE NUMERIC
USING product_price::numeric;

-- Adjust for financial multiplier of 10^6:
UPDATE category
SET product_price = product_price * 0.000001

-- Remove 0 values after stripping multiplier:
UPDATE product_hits
SET product_price = NULL
WHERE product_price = 0

-- Confirm results:
SELECT *
FROM product_hits
```

##### COLUMN product_revenue:

```sql
-- Remove string converted "NULL" values:
UPDATE product_hits
SET product_revenue = null
WHERE product_revenue LIKE upper('%NULL%')

-- Cast column type to numeric:
ALTER TABLE product_hits
ALTER COLUMN product_revenue TYPE NUMERIC
USING product_revenue::numeric;

-- Adjust for financial multiplier of 10^6:
UPDATE product_hits
SET product_revenue = product_revenue * 0.000001

-- Confirm results:
SELECT *
FROM product_hits
```

##### COLUMN product_sku:

#### DATA TRANSFORMATIONS: category TABLE:

ALTER TABLE category
ALTER COLUMN product_price TYPE NUMERIC
USING product_price::numeric;

```SQL
CREATE TABLE IF NOT EXISTS public.products_category
(
category_id INT NOT NULL
, product_price DECIMAL(15,2)
, product_sku VARCHAR(50) COLLATE pg_catalog."default"
, v2_product_name VARCHAR(100) COLLATE pg_catalog."default"
, v2_product_category VARCHAR(100) COLLATE pg_catalog."default"
, main_category VARCHAR(50)
, sub_cat_1 VARCHAR(50)
, sub_cat_2 VARCHAR(50)
, sub_cat_3 VARCHAR(50)
, product_variant VARCHAR(50) COLLATE pg_catalog."default"
, alls_id INT
, ecommerce_id INT
, CONSTRAINT category_pkey PRIMARY KEY (category_id)
, CONSTRAINT category_alls_id_fkey FOREIGN KEY (alls_id)
REFERENCES public.all_sessions (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION,
CONSTRAINT category_ecommerce_id_fkey FOREIGN KEY (ecommerce_id)
REFERENCES public.ecommerce_hits (ecommerce_id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.products_category
OWNER to postgres;
-- Create a Temporary table to story new values in new column order:
WITH split_cat AS (
	SELECT
		category_id
		, product_price
		, product_sku
		, v2_product_name
		, v2_product_category
		, SPLIT_PART (v2_product_category,'/',1) main_category
		, SPLIT_PART (v2_product_category,'/',2) sub_cat_1
		, SPLIT_PART (v2_product_category,'/',3) sub_cat_2
		, SPLIT_PART (v2_product_category,'/',4) sub_cat_3
		, product_variant
		, alls_id
		, ecommerce_id
	FROM category
)
-- Insert statement for the new table:
INSERT INTO products_category(
	category_id
	, product_price
	, product_sku
	, v2_product_name
	, v2_product_category
	, main_category
	, sub_cat_1
	, sub_cat_2
	, sub_cat_3
	, product_variant
	, alls_id
	, ecommerce_id
)
-- From the Temp table
SELECT
	category_id
	, product_price
	, product_sku
	, v2_product_name
	, v2_product_category
	, main_category
	, sub_cat_1
	, sub_cat_2
	, sub_cat_3
	, product_variant
	, alls_id
	, ecommerce_id
FROM split_cat

-- Confirm new table values:
SELECT * FROM public.products_category
```

#### DATA TRANSFORMATIONS: geo_visitor TABLE:

```sql
-- Fix string formated 'NULL' values:
UPDATE transaction_hits
SET total_transaction_revenue = null
WHERE total_transaction_revenue LIKE upper('%NULL%')
```

#### DATA TRANSFORMATIONS: category TABLE:

#### DATA TRANSFORMATIONS: category TABLE:

#### DATA TRANSFORMATIONS: category TABLE:

#### DATA TRANSFORMATIONS: category TABLE:

#### DATA TRANSFORMATIONS: category TABLE:

# `WIP`-----------------------------------------------------------------------`WIP`

#### Group values: Consistency?

_SQL LIKE_

```sql
-- find values by string values to find typos etc
-- This ex: Find all facilities whose name BEGINS with 'Tennis'
SELECT * FROM cd.facilities
WHERE UPPER(name) LIKE 'TENNIS%'
-- This ex: Find all facilities whose name CONTAINS  'Tennis'
SELECT * FROM cd.facilities
WHERE UPPER(name) LIKE '%TENNIS%'
-- Strings can be searched using the regex  ~ operator too. this ex looks for '()'
SELECT memid, telephone FROM cd.members
WHERE telephone ~ '[()]';

```

_SQL LPAD_

```sql
-- This example pads member zipcodes with zeros
SELECT LPAD(CAST(zipcode AS CHAR(5)),5,'0') zip
FROM cd.members
ORDER BY zip;
```

_SQL TRANSLATE_

```sql
-- This ex takes the '-() ' symbols out of the phonenumbers list and replaces them with the empty string (TRANSLATE(input string, CHARs to remove, new chars))
SELECT memid, TRANSLATE(telephone, '-() ', '') AS telephone
FROM cd.members
ORDER BY memid;
```

#### Useful Merges?:

_SQL CONCATENATION_:

```sql
-- Some systems (like SQL Server) use +, but || is the SQL standard.
SELECT surname||', '||firstname
FROM cd.members;
-- REF: https://pgexercises.com/questions/string/concat.html
```

_SQL COALESCE_

```SQL
-- COALESCE takes a list of input values and returns the first value that IS NOT NULL
WITH class_count AS (
SELECT student_id, COUNT(*) AS num_of_class
FROM class_history
GROUP BY student_id
)
SELECT
COALESCE(c.student_id, s.student_id) AS student_id,
s.student_name,
COALESCE(c.num_of_class, 0) AS num_of_class
FROM class_count c
_____ student s ON c.student_id = s.student_id
```

_SQL EXTRACT_

```SQL
-- Extract time components from timestamps to summarize data:
SELECT
	  facid
	, EXTRACT(MONTH FROM starttime) AS month
	, SUM(slots) AS total
FROM cd.bookings
WHERE EXTRACT(YEAR FROM starttime) = 2012
GROUP BY 1, 2
ORDER BY 1, 2;
```

#### Useful splits?:

```sql

```

```sql

```

#### Renaming a table:

```sql

```

#### Statistical summary of the categorical values:

```sql

```

#### Statistical summary of the numerical values: Are there ouliers?

```sql

```

### STEP 3: Transforming the Data

> "The first phase of data transformations should include things like data type conversion and flattening of hierarchical data...

> Parsing fields out of comma-delimited log data for loading to a relational database is an example of this type of data transformation....

> Data transformation is often concerned with whittling data down and making it more manageable. Data may be consolidated by filtering out unnecessary fields, columns, and records. Omitted data might include numerical indexes in data intended for graphs and dashboards or records from business regions that aren’t of interest in a particular study...

> Data might also be aggregated or summarized. by, for instance, transforming a time series of customer transactions to hourly or daily sales counts.

- A customer’s transactions can be rolled up into a grand total and added into a customer information table for quicker reference or for use by customer analytics systems.
- Long or freeform fields may be split into multiple columns,
- and missing values can be imputed or corrupted data replaced as a result of these kinds of transformations."

REF: https://www.stitchdata.com/resources/data-transformation/

### STEP 4: Load Data Back into the DB

---

_Other notes?_

_Definitions?_

_Abreviations?_

---

## References

1. Surname, F. (2021) Title title of titleynesses of article. _Name of Page._ https://urly-ish.ness.business/goods.live.here

1. https://pgexercises.com/questions/string/concat.html
1. https://www.stitchdata.com/resources/data-transformation/
1. https://support.google.com/analytics/answer/3437719?hl=en
