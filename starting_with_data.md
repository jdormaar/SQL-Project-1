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

-- Evaluate NULL content:
SELECT COUNT(*)
FROM all_sessions
WHERE full_visitor_id IS NULL;
```

Answer:

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

Question 4:

SQL Queries:

Answer:

Question 5:

SQL Queries:

Answer:
