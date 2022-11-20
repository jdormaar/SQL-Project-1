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

```sql
SELECT
    CAST((id + 500000)AS INT) AS pr_hits_id
    , product_quantity
    , product_price
    , product_revenue
    , product_sku
    , CAST(id AS INT) AS alls_id
    , CAST((id + 600000)AS INT) AS category_id
    , CAST((id + 700000)AS INT) AS ecommerce_id
FROM all_sessions;


CREATE TABLE product_hits(
    pr_hits_id INT
    , product_quantity VARCHAR
    , product_price BIGINT
    , product_revenue VARCHAR
    , product_sku VARCHAR(50)
	, alls_id INT
    , category_id INT
    , ecommerce_id INT
    , PRIMARY KEY (pr_hits_id)
    , FOREIGN KEY (alls_id)
    REFERENCES all_sessions (id)
    , FOREIGN KEY (category_id)
    REFERENCES category (category_id)
    , FOREIGN KEY (ecommerce_id)
    REFERENCES ecommerce_hits (ecommerce_id)
);

COPY product_hits(
  pr_hits_id
  , product_quantity
  , product_price
  , product_revenue
  , product_sku
  , alls_id
  , category_id
  , ecommerce_id
)
FROM '/Users/Shared/product_hits.csv'
DELIMITER ','
CSV HEADER;

SELECT
    CAST((id + 400000)AS INT) AS tr_hits_id
    , transaction_id
    , transaction_revenue
    , total_transaction_revenue
    , currency_code
    , CAST(id AS INT) AS alls_id
    , CAST((id + 700000)AS INT) AS ecommerce_id
FROM all_sessions;

CREATE TABLE transaction_hits(
    tr_hits_id INT
    , transaction_id VARCHAR(50)
    , transaction_revenue VARCHAR
    , total_transaction_revenue VARCHAR
    , currency_code VARCHAR(20)
    , alls_id INT
    , ecommerce_id INT
    , PRIMARY KEY (tr_hits_id)
    , FOREIGN KEY (alls_id)
    REFERENCES all_sessions (id)
    , FOREIGN KEY (ecommerce_id)
    REFERENCES ecommerce_hits (ecommerce_id)
);

COPY transaction_hits(
    tr_hits_id
    , transaction_id
    , transaction_revenue
    , total_transaction_revenue
    , currency_code
    , alls_id
    , ecommerce_id
)
FROM '/Users/Shared/transaction_hits.csv'
DELIMITER ','
CSV HEADER;

SELECT
    CAST((id + 300000)AS INT) AS totals_id
    , total_transaction_revenue
    , transactions
    , time_on_site
    , page_views
    , session_quality_dim
    , CAST(id AS INT) AS alls_id
FROM all_sessions;

CREATE TABLE totals(
    totals_id INT
    , total_transaction_revenue VARCHAR
    , transactions VARCHAR
    , time_on_site VARCHAR
    , page_views VARCHAR
    , session_quality_dim VARCHAR
    , alls_id INT
    , PRIMARY KEY (totals_id)
    , FOREIGN KEY (alls_id)
    REFERENCES all_sessions (id)
);

COPY totals(
    totals_id
    , total_transaction_revenue
    , transactions
    , time_on_site
    , page_views
    , session_quality_dim
    , alls_id
)
FROM '/Users/Shared/totals.csv'
DELIMITER ','
CSV HEADER;

ALTER TABLE all_sessions
  DROP COLUMN total_transaction_revenue;

ALTER TABLE all_sessions
  DROP COLUMN transactions;

ALTER TABLE all_sessions
  DROP COLUMN time_on_site;

ALTER TABLE all_sessions
  DROP COLUMN page_views;

ALTER TABLE all_sessions
  DROP COLUMN session_quality_dim;

ALTER TABLE all_sessions
  DROP COLUMN transaction_id;

ALTER TABLE all_sessions
  DROP COLUMN transaction_revenue;

ALTER TABLE all_sessions
  DROP COLUMN currency_code;

ALTER TABLE all_sessions
  DROP COLUMN product_quantity;

ALTER TABLE all_sessions
  DROP COLUMN product_price;

ALTER TABLE all_sessions
  DROP COLUMN product_revenue;
```
