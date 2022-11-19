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

| Column Name            | Distinct | NULLs     | Definition of contained values:                        |
| ---------------------- | -------- | --------- | ------------------------------------------------------ |
| visit_number           | 222      | 0         | The session number                                     |
| visit_id               | 148,642  | 0         | The name of each product item.                         |
| visit_start_time       | 148,853  | 0         | Timestamp _as POSIX time format_                       |
| date                   | 93       | 0         | Date of the visit record                               |
| full_visitor_id        | 120,018  | 0         | The unique visitor id                                  |
| user_id                | NULL     | all       | This column will be deleted                            |
| channel_grouping       | 8        | 0         | Default Channel Group per session View for the user    |
| social_engagement_type | 1        | 0         | This column will be deleted                            |
| units_sold             | 135      | 95,147    |                                                        |
| page_views             | 129      | 72        | (totals.) Number of page views from within the session |
| time_on_site           | 3270     | 477,465   | (totals.) _in seconds_                                 |
| bounces                | 1        | 3,826,283 |                                                        |
| revenue                | 5270     | 4,285,767 |                                                        |
| unit_price             | 1442     | 0         |                                                        |

### TABLE 3: products: [ rows, 8 columns]

| Column Name            | Distinct | NULLs     | Definition of contained values:                        |
| ---------------------- | -------- | --------- | ------------------------------------------------------ |
| visit_number           | 222      | 0         | The session number                                     |
| visit_id               | 148,642  | 0         | The name of each product item.                         |
| visit_start_time       | 148,853  | 0         | Timestamp _as POSIX time format_                       |
| date                   | 93       | 0         | Date of the visit record                               |
| full_visitor_id        | 120,018  | 0         | The unique visitor id                                  |
| user_id                | NULL     | all       | This column will be deleted                            |
| channel_grouping       | 8        | 0         | Default Channel Group per session View for the user    |
| social_engagement_type | 1        | 0         | This column will be deleted                            |
| units_sold             | 135      | 95,147    |                                                        |
| page_views             | 129      | 72        | (totals.) Number of page views from within the session |
| time_on_site           | 3270     | 477,465   | (totals.) _in seconds_                                 |
| bounces                | 1        | 3,826,283 |                                                        |
| revenue                | 5270     | 4,285,767 |                                                        |
| unit_price             | 1442     | 0         |                                                        |

### TABLE 4: sales_by_sku: [ rows, 3 columns]

| Column Name            | Distinct | NULLs     | Definition of contained values:                        |
| ---------------------- | -------- | --------- | ------------------------------------------------------ |
| visit_number           | 222      | 0         | The session number                                     |
| visit_id               | 148,642  | 0         | The name of each product item.                         |
| visit_start_time       | 148,853  | 0         | Timestamp _as POSIX time format_                       |
| date                   | 93       | 0         | Date of the visit record                               |
| full_visitor_id        | 120,018  | 0         | The unique visitor id                                  |
| user_id                | NULL     | all       | This column will be deleted                            |
| channel_grouping       | 8        | 0         | Default Channel Group per session View for the user    |
| social_engagement_type | 1        | 0         | This column will be deleted                            |
| units_sold             | 135      | 95,147    |                                                        |
| page_views             | 129      | 72        | (totals.) Number of page views from within the session |
| time_on_site           | 3270     | 477,465   | (totals.) _in seconds_                                 |
| bounces                | 1        | 3,826,283 |                                                        |
| revenue                | 5270     | 4,285,767 |                                                        |
| unit_price             | 1442     | 0         |                                                        |

### TABLE 5: sales_report: [ rows, 9 columns]

| Column Name            | Distinct | NULLs     | Definition of contained values:                        |
| ---------------------- | -------- | --------- | ------------------------------------------------------ |
| visit_number           | 222      | 0         | The session number                                     |
| visit_id               | 148,642  | 0         | The name of each product item.                         |
| visit_start_time       | 148,853  | 0         | Timestamp _as POSIX time format_                       |
| date                   | 93       | 0         | Date of the visit record                               |
| full_visitor_id        | 120,018  | 0         | The unique visitor id                                  |
| user_id                | NULL     | all       | This column will be deleted                            |
| channel_grouping       | 8        | 0         | Default Channel Group per session View for the user    |
| social_engagement_type | 1        | 0         | This column will be deleted                            |
| units_sold             | 135      | 95,147    |                                                        |
| page_views             | 129      | 72        | (totals.) Number of page views from within the session |
| time_on_site           | 3270     | 477,465   | (totals.) _in seconds_                                 |
| bounces                | 1        | 3,826,283 |                                                        |
| revenue                | 5270     | 4,285,767 |                                                        |
| unit_price             | 1442     | 0         |                                                        |

## Methods and Procedure: Queries

.....(and this is how I did it)

### STEP 1: Getting Oriented with the Data.

Consistent naming conventions were first established with the column and table names for simpler interactions with the data. Tables were titled with snake case, which is the simplest format for PGAdmin as well. An example query of this series on full_visitor_id column for all_sessions below:

_ALTER TABLE to RENAME COLUMN HEADING_:

```sql
-- Rename column headings for consistency to snake case.
ALTER TABLE public.all_sessions
RENAME COLUMN fullVisitorId TO full_visitor_id;
```

Table shape was determined with simple select statements

_SQL SELECT_:

Count aggregate statements were used sequentially for an initial broad overview of the qantity, type, and quality of the data in each column. The results of these were populated into Tables 1-5 in Discussion. An example query of this series on full_visitor_id column for all_sessions below:

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

#### Create (Temp?) new table:

```sql

```

#### Create a table to query using WITH:

_SQL WITH_

```sql
-- This ex is of a SQL query which finds: total # of classes taken, for all students (past and present) who've taken >= 1 class.
-- The result set has the student ID, name, and the # of classes taken.

-- The temp table
WITH class_count AS (
SELECT student_id, COUNT(*) AS num_of_class
FROM class_history
GROUP BY student_id
)
-- Subsequent query
SELECT
COALESCE(c.student_id, s.student_id) AS student_id,
s.student_name,
COALESCE(c.num_of_class, 0) AS num_of_class
FROM class_count c
_____ student s ON c.student_id = s.student_id
```

### STEP 2: Cleaning the Issues Found in Orientation.

Get to know the table.

#### Renaming a table:

```sql

```

#### Determine the size of the table:

```sql

```

#### Check for column duplications:

Finding full row duplications in a table:

```sql
-- Row duplications in all_sessions:
WITH dup_rows AS (
	SELECT fullvisitorid
		, channelgrouping
		, time
		, country
		, city
		, visitid
-- 		, productsku
		, COUNT(*) AS num_rows
	FROM all_sessions
	GROUP BY 1,2,3,4,5,6
	HAVING COUNT(*) > 1)

SELECT *
FROM all_sessions al
JOIN dup_rows d
ON al.fullvisitorid = d.fullvisitorid
AND al.time = d.time
-- AND al.productsku = d.productsku

/* NOTE: all_sessions appeared to have 453 counted duplications
within all columns except productsku, and none once included.
 Which implies 453 instances of a user purchasing > 1 product */
```

```sql
-- Row duplications in analytics
WITH dup_rows AS (
	SELECT visitnumber
		, visitid
		, visitstarttime
		, date
		, fullvisitorid
		, channelgrouping
		, unit_price
		, COUNT(*) AS num_rows
	FROM analytics an
	GROUP BY 1,2,3,4,5,6,7
	HAVING COUNT(*) > 1)

SELECT *
FROM analytics an
JOIN dup_rows d
ON an.visitid = d.visitid
AND an.visitstarttime = d.visitstarttime
AND an.unit_price = d.unit_price
-- Current parameters appear to show 3,492,284 duplicate rows.

-- Further inquiry required.
```

#### Check for row duplications:

```sql

```

#### Renaming a column:

```sql

```

#### Sort column values:

```sql

```

#### Count column values:

```sql

```

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

#### Number of unique/duplicate values:

```sql

```

if there are, what do you know enough about the data to decide if they are appropriate or not yet?

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
