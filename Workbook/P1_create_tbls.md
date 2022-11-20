### CREATE sales_by_sku TABLE:

```sql
CREATE TABLE sales_by_sku(
id SERIAL,
productSKU VARCHAR(50),
total_ordered INT,
PRIMARY KEY (id)
);
```

### LOAD sales_by_sku TABLE WITH CSV

```sql
COPY sales_by_sku(productSKU, total_ordered)
FROM '/Users/Shared/sales_by_sku.csv'
DELIMITER ','
CSV HEADER;
```

### CREATE sales_report TABLE:

```sql
CREATE TABLE sales_report(
id SERIAL,
productSKU VARCHAR(50),
total_ordered INT,
name VARCHAR(100),
stockLevel INT,
restockingLeadTime INT,
sentimentScore DECIMAL(15,1),
sentimentMagnitude DECIMAL(15,1),
ratio FLOAT,
PRIMARY KEY (id)
);
```

### LOAD all_sessions TABLE WITH CSV

```sql
COPY sales_report(
productSKU,
total_ordered,
name,
stockLevel,
restockingLeadTime,
sentimentScore,
sentimentMagnitude,
ratio
)
FROM '/Users/Shared/sales_report.csv'
DELIMITER ','
CSV HEADER;
```

### CREATE products TABLE:

```sql
CREATE TABLE products(
id SERIAL,
SKU VARCHAR(50),
name VARCHAR(100),
orderedQuantity INT,
stockLevel INT,
restockingLeadTime INT,
sentimentScore DECIMAL(15, 1),
sentimentMagnitude DECIMAL(15, 1),
PRIMARY KEY (id)
);
```

### LOAD products TABLE WITH CSV

```sql
COPY products(
SKU,
name,
orderedQuantity,
stockLevel,
restockingLeadTime,
sentimentScore,
sentimentMagnitude
)
FROM '/Users/Shared/products.csv'
DELIMITER ','
CSV HEADER;
```

### CREATE analytics TABLE:

```sql
CREATE TABLE analytics(
id SERIAL,
visitNumber SMALLINT,
visitId VARCHAR(50),
visitStartTime INT,
date DATE,
fullvisitorId VARCHAR(50),
userid VARCHAR(50),
channelGrouping VARCHAR(100),
socialEngagementType VARCHAR(100),
units_sold SMALLINT,
pageviews SMALLINT,
timeonsite INT,
bounces SMALLINT,
revenue BIGINT,
unit_price INT,
PRIMARY KEY (id)
);
```

### LOAD analytics TABLE WITH CSV

```SQL
COPY analytics(
visitNumber,
visitId,
visitStartTime,
date,
fullvisitorId,
userid,
channelGrouping,
socialEngagementType,
units_sold,
pageviews,
timeonsite,
bounces,
revenue,
unit_price
)
FROM '/Users/Shared/analytics.csv'
DELIMITER ','
CSV HEADER;
```

### CREATE all_sessions TABLE:

```SQL
CREATE TABLE all_sessions(
id SERIAL,
fullVisitorId VARCHAR(50),
channelGrouping VARCHAR(100),
time INT,
country VARCHAR(50),
city VARCHAR(100),
totalTransactionRevenue BIGINT,
transactions INT,
timeOnSite INT,
pageviews SMALLINT,
sessionQualityDim INT,
date DATE,
visitId VARCHAR(50),
type VARCHAR(50),
productRefundAmount INT,
productQuantity INT,
productPrice BIGINT,
productRevenue INT,
productSKU VARCHAR(50),
v2ProductName VARCHAR(100),
v2ProductCategory VARCHAR(100),
productVariant VARCHAR(50),
currencyCode VARCHAR(50),
itemQuantity INT,
itemRevenue BIGINT,
transactionRevenue BIGINT,
transactionId VARCHAR(50),
pageTitle VARCHAR(1000),
searchKeyword VARCHAR(50),
pagePathLevel1 VARCHAR(50),
eCommerceAction_type SMALLINT,
eCommerceAction_step SMALLINT,
eCommerceAction_option VARCHAR(50),
PRIMARY KEY (id)
);
```

### LOAD all_sessions TABLE WITH CSV

```sql
COPY all_sessions(
fullVisitorId,
channelGrouping,
time,
country,
city,
totalTransactionRevenue,
transactions,
timeOnSite,
pageviews,
sessionQualityDim,
date,
visitId,
type,
productRefundAmount,
productQuantity,
productPrice,
productRevenue,
productSKU,
v2ProductName,
v2ProductCategory,
productVariant,
currencyCode,
itemQuantity,
itemRevenue,
transactionRevenue,
transactionId,
pageTitle,
searchKeyword,
pagePathLevel1,
eCommerceAction_type,
eCommerceAction_step,
eCommerceAction_option
)
FROM '/Users/Shared/all_sessions.csv'
DELIMITER ','
CSV HEADER;
```
