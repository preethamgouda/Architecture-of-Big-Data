
# Architecture of Big Data System


## Hive Commands

-By Preetham Gouda

    

### 1. Create database in hive

```powershell
#Create database in hive
hive> CREATE DATABASE assignment_db;
```

```
OK
Time taken: 0.427 seconds
```

### 2. Create Hive table inside database.

**Create skeleton of `customers` table in hive.**

```sql
hive>  CREATE TABLE assignment_db.customers(
    >  customer_id int,
    >  customer_fname varchar(45),
    >  customer_lname varchar(45),
    >  customer_email varchar(45),
    >  customer_password varchar(45),
    >  customer_street varchar(255),
    >  customer_city varchar(45),
    >  customer_state varchar(45),
    >  customer_zipcode varchar(45)) row format delimited fields terminated by ',' stored as textfile;
```

```
OK
Time taken: 1.195 seconds
```

### 3. Load Data into hive

**Load data into `customers` table from hdfs**

```sql
hive> load data inpath '/retail_db/customers' into table assignment_db.customers;
```

```
Loading data to table assignment_db.customers
OK
Time taken: 2.936 seconds
```

### 4. Load data from local file into Hive table (Hint: use keyword `load data local inpath`command)

```sql
hive> load data local inpath '/home/sois/Documents/file.csv' overwrite into table assignment_db.customers;
```

```
Loading data to table assignment_db.customers
OK
Time taken: 1.129 seconds
```

`**file.csv` - Local csv file used to load data from local machine to hive.**



### Run queries for customer table for following:

**a) Find unique states**

```sql
hive> SELECT DISTINCT customer_state
    > FROM assignment_db.customers;
```

**b) Find number of customers from each state**

```sql
hive>  SELECT customer_state, count(*) AS COUNT
    >  FROM assignment_db.customers
    >  GROUP BY customer_state;
```

**c) List and count number of unique `fnames`.**

```sql
--Group people with same first name
hive> SELECT customer_fname, COUNT(*) 
    > FROM assignment_db.customers
    > GROUP BY customer_fname;
```

```sql
--Total Unique fnames
hive> SELECT COUNT(DISTINCT customer_fname) 
    > FROM assignment_db.customers;
```

**d) List and count number of unique `cities`.**

```sql
--Group people who are from same city
hive> SELECT customer_city, COUNT(*) 
    > FROM assignment_db.customers
    > GROUP BY customer_city;
```

```sql
--Total Unique Cities
hive> SELECT COUNT(DISTINCT customer_city) 
    > FROM assignment_db.customers;
```
