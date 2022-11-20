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

| Column Name | Distinct | NULLs | Definition of contained values: |
| ----------- | -------- | ----- | ------------------------------- |
| Col_1       |          |       |                                 |
| Col_2       |          |       |                                 |
| Col_3       |          |       |                                 |
| Col_4       |          |       |                                 |
| Col_5       |          |       |                                 |
| Col_6       |          |       |                                 |
| Col_7       |          |       |                                 |
| Col_8       |          |       |                                 |

|

## Methods and Procedure: Queries

.....(and this is how I did it)

### STEP 1: Getting Oriented with the Data.

#### Title and Naming consistency:

_SQL ALTER TABLE - RENAME COLUMN_:

```sql
-- Rename column headings for consistency to snake case.
ALTER TABLE public.all_sessions
RENAME COLUMN fullVisitorId TO full_visitor_id;
```

#### Number of unique/duplicate values:

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

```

### STEP 2: Cleaning the Issues Found in Orientation.

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
