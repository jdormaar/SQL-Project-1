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

### TABLE 1: all_sessions table:

| column_name | What the data is or means:                                   |
| ----------- | ------------------------------------------------------------ |
| sku_id      | This is the primarykey for this table. Each unique id is.... |
| name        | The name of each product item.                               |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |

### TABLE 1: all_sessions table:

| column_name | What the data is or means:                                   |
| ----------- | ------------------------------------------------------------ |
| sku_id      | This is the primarykey for this table. Each unique id is.... |
| name        | The name of each product item.                               |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |

### TABLE 1: all_sessions table:

| column_name | What the data is or means:                                   |
| ----------- | ------------------------------------------------------------ |
| sku_id      | This is the primarykey for this table. Each unique id is.... |
| name        | The name of each product item.                               |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |

### TABLE 1: all_sessions table:

| column_name | What the data is or means:                                   |
| ----------- | ------------------------------------------------------------ |
| sku_id      | This is the primarykey for this table. Each unique id is.... |
| name        | The name of each product item.                               |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |

### TABLE 1: all_sessions table:

| column_name | What the data is or means:                                   |
| ----------- | ------------------------------------------------------------ |
| sku_id      | This is the primarykey for this table. Each unique id is.... |
| name        | The name of each product item.                               |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |

### TABLE 1: all_sessions table:

| column_name | What the data is or means:                                   |
| ----------- | ------------------------------------------------------------ |
| sku_id      | This is the primarykey for this table. Each unique id is.... |
| name        | The name of each product item.                               |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |
| stock_level | wakka wakkka yep                                             |

## Methods and Procedure: Queries

.....(and this is how I did it)

### STEP 1: Getting Oriented with the Data.

#### Useful Merges?:

_Concatenation_:

```sql
SELECT surname||', '||firstname
FROM cd.members;
```

#### Useful splits?:

```sql

```

#### Create (Temp?) new table:

```sql

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

```sql

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

_SQL Like_

```sql
-- find values by string values to find typos etc
-- This ex: Find all facilities whose name BEGINS with 'Tennis'
SELECT * FROM cd.facilities
WHERE name LIKE 'Tennis%'
-- This ex: Find all facilities whose name CONTAINS  'Tennis'
SELECT * FROM cd.facilities
WHERE name LIKE '%Tennis%'
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

### STEP 4: Load Data Back into the DB

---

_Other notes?_

_Definitions?_

_Abreviations?_

---

## References

1. Surname, F. (2021) Title title of titleynesses of article. _Name of Page._ https://urly-ish.ness.business/goods.live.here
