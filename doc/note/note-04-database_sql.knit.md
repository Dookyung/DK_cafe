---
title: "Database & SQL"
author: "Dookyung Kim"
date: "2018-07-11"
output: 
    html_document:
        css: 'Mystyle.css'
mainfont: NanumGothic
---




## Merging Data

### joins


`dplyr` package의 Data sets 결합(combining) 함수: 

* `full_join` - preserving all rows in both `a` and `b`.
* `inner_join` - preserving only rows with keys in both `a` and `b`.
* `left_join` - preserving all rows of `a`
* `right_join` - preserving all rows of `b`.
* `semi_join` - Subset rows in `a` that have a match in `b`.
* `anti_join` - Subset rows in `a` that do not have a match in `b`.

<img src="img/04-database_sql/join_venn.png" width="512" style="display: block; margin: auto;" />

---

### Joining Data

아래 두 개의 table이 있다고 가정

<div class = "split-50">
<div class = "column">


```r
addr
```

```
##    name             email
## 1 Alice alice@company.com
## 2   Bob   bob@company.com
## 3 Carol carol@company.com
## 4  dave  dave@company.com
## 5   Eve   eve@company.com
```
</div>

<div class = "column">


```r
phone
```

```
##    name        phone
## 1   Bob 919 555-1111
## 2 Carol 919 555-2222
## 3   Eve 919 555-3333
## 4   Eve 310 555-3333
## 5 Frank 919 555-4444
```
</div>

</div>

<hr>

### Mutating Joins

__Outer Join__

dplyr:

```r
full_join(addr, phone)
```

```
## Joining, by = "name"
```

```
##    name             email        phone
## 1 Alice alice@company.com         <NA>
## 2   Bob   bob@company.com 919 555-1111
## 3 Carol carol@company.com 919 555-2222
## 4  dave  dave@company.com         <NA>
## 5   Eve   eve@company.com 919 555-3333
## 6   Eve   eve@company.com 310 555-3333
## 7 Frank              <NA> 919 555-4444
```

```r
full_join(addr, phone, by="name")
```

```
##    name             email        phone
## 1 Alice alice@company.com         <NA>
## 2   Bob   bob@company.com 919 555-1111
## 3 Carol carol@company.com 919 555-2222
## 4  dave  dave@company.com         <NA>
## 5   Eve   eve@company.com 919 555-3333
## 6   Eve   eve@company.com 310 555-3333
## 7 Frank              <NA> 919 555-4444
```

Base R:

```r
merge(addr, phone, all=TRUE)
```

```
##    name             email        phone
## 1 Alice alice@company.com         <NA>
## 2   Bob   bob@company.com 919 555-1111
## 3 Carol carol@company.com 919 555-2222
## 4  dave  dave@company.com         <NA>
## 5   Eve   eve@company.com 919 555-3333
## 6   Eve   eve@company.com 310 555-3333
## 7 Frank              <NA> 919 555-4444
```


__Inner Join__

dplyr:

```r
inner_join(addr,phone)
```

```
##    name             email        phone
## 1   Bob   bob@company.com 919 555-1111
## 2 Carol carol@company.com 919 555-2222
## 3   Eve   eve@company.com 919 555-3333
## 4   Eve   eve@company.com 310 555-3333
```

Base R:

```r
merge(addr, phone, all=FALSE)
```

```
##    name             email        phone
## 1   Bob   bob@company.com 919 555-1111
## 2 Carol carol@company.com 919 555-2222
## 3   Eve   eve@company.com 919 555-3333
## 4   Eve   eve@company.com 310 555-3333
```

__Left Join__

dplyr:

```r
left_join(addr,phone)
```

```
##    name             email        phone
## 1 Alice alice@company.com         <NA>
## 2   Bob   bob@company.com 919 555-1111
## 3 Carol carol@company.com 919 555-2222
## 4  dave  dave@company.com         <NA>
## 5   Eve   eve@company.com 919 555-3333
## 6   Eve   eve@company.com 310 555-3333
```

Base R:

```r
merge(addr, phone, all.x=TRUE)
```

```
##    name             email        phone
## 1 Alice alice@company.com         <NA>
## 2   Bob   bob@company.com 919 555-1111
## 3 Carol carol@company.com 919 555-2222
## 4  dave  dave@company.com         <NA>
## 5   Eve   eve@company.com 919 555-3333
## 6   Eve   eve@company.com 310 555-3333
```

__Right Join__

dplyr:

```r
right_join(addr, phone)
```

```
##    name             email        phone
## 1   Bob   bob@company.com 919 555-1111
## 2 Carol carol@company.com 919 555-2222
## 3   Eve   eve@company.com 919 555-3333
## 4   Eve   eve@company.com 310 555-3333
## 5 Frank              <NA> 919 555-4444
```

Base R:

```r
merge(addr, phone, all.y=TRUE)
```

```
##    name             email        phone
## 1   Bob   bob@company.com 919 555-1111
## 2 Carol carol@company.com 919 555-2222
## 3   Eve   eve@company.com 919 555-3333
## 4   Eve   eve@company.com 310 555-3333
## 5 Frank              <NA> 919 555-4444
```

### Filtering Joins

__Semi Joins__


```r
semi_join(addr, phone)
```

```
##    name             email
## 1   Bob   bob@company.com
## 2 Carol carol@company.com
## 3   Eve   eve@company.com
```

__Anti Joins__


```r
anti_join(addr, phone)
```

```
##    name             email
## 1 Alice alice@company.com
## 2  dave  dave@company.com
```
---

### many-to-many relationships




```r
addr_new
```

```
##    name             email
## 1 Alice alice@company.com
## 2 Alice   alice@gmail.com
## 3   Bob   bob@company.com
## 4   Bob   bob@hotmail.com
```





```r
phone_new
```

```
##    name        phone
## 1 Alice 919 555-1111
## 2 Alice 310 555-2222
## 3   Bob 919 555-3333
## 4   Bob 310 555-3333
```


```r
full_join(addr, phone, by="name")
```

```
##    name             email        phone
## 1 Alice alice@company.com         <NA>
## 2   Bob   bob@company.com 919 555-1111
## 3 Carol carol@company.com 919 555-2222
## 4  dave  dave@company.com         <NA>
## 5   Eve   eve@company.com 919 555-3333
## 6   Eve   eve@company.com 310 555-3333
## 7 Frank              <NA> 919 555-4444
```


## databases

### Numbers everyone should know

| Task                                | Timing            |
|-------------------------------------|-------------------|
| L1 cache reference                  | 0.5 ns            |
| L2 cache reference                  | 7 ns              |
| Main memory reference               | 100 ns            |
| Read 1 MB sequentially from memory  | 250,000 ns        |
| Disk seek                           | 10,000,000 ns     |
| Read 1 MB sequentially from network | 10,000,000 ns     |
| Read 1 MB sequentially from disk    | 30,000,000 ns     |


From: http://surana.wordpress.com/2009/01/01/numbers-everyone-should-know/



### Implications for bigish data

Lets imagine we have a *10 GB* flat data file and that we want to select certain rows based on a given criteria. This requires a sequential read across the entire data set.


If we can store the file in memory:

* $10~GB \times (250~\mu s / 1~MB) = 0.25$ seconds

If we have to access the file from disk:

* $10~GB \times (30~ms / 1~MB) = 30$ seconds

<br/>

This is just for *reading* data, if we make any modifications (*writing*) things are much worse. 


---

### Blocks

<div class="centered">
Cost: Disk << Memory

Speed: Disk <<< Memory
</div>

<br/>

So usually possible to grow our disk storage to accommodate our data. However, memory is usually the limiting resource, and if we can't fit everything into memory?

<br/>

Create *blocks* - group rows based on similar attributes and read in multiple rows at a time. Optimal size will depend on the task and the properties of the disk.



---

### Linear vs Binary Search {.build}

Even with blocks, any kind of subsetting of rows requires a linear search, which requires $\mathcal{O}(N)$ accesses where $N$ is the number of blocks.

<br/>

We can do much better if we are careful about how we structure our data, specifically sorting some or all of the columns. 

* Sorting is expensive, $\mathcal{O}(N \log N)$, but it only needs to be done once. 

* After sorting, we can use a binary search for any subsetting tasks ($\mathcal{O}(\log N)$).

* These sorted columns are known as *indexes*.

* Indexes require additional storage, but usually small enough to be kept in memory while blocks stay on disk.


---

### Binary Search Example

| Age  | Name         |
|------|--------------|
| 19   | Carol        |
| 20   | Greg         |
| 21   | Alice        |
| 21   | Dave         |
| 22   | Eve          |
| 23   | Bob          |
| 23   | Frank        |

<br/><br/>

Lets search for records for people who are 22 or older.


---

### and then?

This is just barely scratching the surface,

* Efficiency gains are not just for disk, access is access

* In general, trade off between storage and efficiency

* Reality is a lot more complicated for everything mentioned so far, lots of very smart people have spent a lot of time thinking about and implementing tools

* Different tasks with different requirements require different implementations and have different criteria for optimization

## Databases


### SQL

Structures Query Language is a special purpose language for interacting with (querying and modifying) these indexed tabular data structures. 

* ANSI Standard but with some dialect divergence

* This functionality maps very closely (but not exactly) with the data manipulation verbs present in dplyr.

* We will see this mapping in more detail in a bit.


---

### DBI

Low level package for interfacing R with Database management systems (DBMS) that provides a common interface to achieve the following functionality:

* connect/disconnect from DB
* create and execute statements in the DB
* extract results/output from statements
* error/exception handling
* information (meta-data) from database objects
* transaction management (optional)

---

## RSQLite

Provides the implementation necessary to use DBI to interface with an SQLite database.

```r
library(RSQLite)

---

## Loading required package: DBI

con = dbConnect(RSQLite::SQLite(), ":memory:")
str(con)

## Formal class 'SQLiteConnection' [package "RSQLite"] with 5 slots
##   ..@ Id                 :<externalptr> 
##   ..@ dbname             : chr ":memory:"
##   ..@ loadable.extensions: logi TRUE
##   ..@ flags              : int 6
##   ..@ vfs                : chr ""
```

---

## Example Table
 
```r
employees = data.frame(name   = c("Alice","Bob","Carol","Dave","Eve","Frank"),
                       email  = c("alice@company.com", "bob@company.com",
                                  "carol@company.com", "dave@company.com",
                                  "eve@company.com",   "frank@comany.com"),
                       salary = c(52000, 40000, 30000, 33000, 44000, 37000),
                       dept   = c("Accounting", "Accounting","Sales",
                                  "Accounting","Sales","Sales"),
                       stringsAsFactors = FALSE)
```

```r
dbWriteTable(con, "employees", employees)
## [1] TRUE

dbListTables(con)
## [1] "employees"
```

---

### Removing Tables

```r
dbWriteTable(con, "employs", employees)
## [1] TRUE

dbListTables(con)
## [1] "employees" "employs"

dbRemoveTable(con,"employs")

---

## [1] TRUE

dbListTables(con)
## [1] "employees"
```

---

### Querying Tables

```r
res = dbSendQuery(con, "SELECT * FROM employees")
dbFetch(res)
##    name             email salary       dept
## 1 Alice alice@company.com  52000 Accounting
## 2   Bob   bob@company.com  40000 Accounting
## 3 Carol carol@company.com  30000      Sales
## 4  Dave  dave@company.com  33000 Accounting
## 5   Eve   eve@company.com  44000      Sales
## 6 Frank  frank@comany.com  37000      Sales

dbClearResult(res)
## [1] TRUE
```

---

### Closing the connection

```r
dbDisconnect(con)
## [1] TRUE
```

---
class: middle
count: false

## SQL Queries

---

### Connecting

```shell
cr173@gort [2018-03-29-sqlite]$ sqlite3 employees.sqlite
SQLite version 3.11.0 2016-02-15 17:29:24
Enter ".help" for usage hints.
```

---

### Table information

The following is specific to SQLite

```sqlite
sqlite> .tables

employees
```

```sqlite
sqlite> .schema employees

CREATE TABLE `employees` (
  `name` TEXT,
  `email` TEXT,
  `salary` REAL,
  `dept` TEXT
);
```

```sqlite
sqlite> .indices employees
```

---

### SELECT Statements

```sqlite
sqlite> SELECT * FROM employees;

Alice|alice@company.com|52000.0|Accounting
Bob|bob@company.com|40000.0|Accounting
Carol|carol@company.com|30000.0|Sales
Dave|dave@company.com|33000.0|Accounting
Eve|eve@company.com|44000.0|Sales
Frank|frank@comany.com|37000.0|Sales
```

---

### Pretty Output

We can make this table output a little nicer with some additonal SQLite options:

```sqlite
sqlite> .mode column
sqlite> .headers on
```

```sqlite
sqlite> SELECT * FROM employees;

name        email              salary      dept      
----------  -----------------  ----------  ----------
Alice       alice@company.com  52000.0     Accounting
Bob         bob@company.com    40000.0     Accounting
Carol       carol@company.com  30000.0     Sales     
Dave        dave@company.com   33000.0     Accounting
Eve         eve@company.com    44000.0     Sales     
Frank       frank@comany.com   37000.0     Sales  
```

---

### select using SELECT

We can subset for certain columns (and rename them) using `SELECT`

```sqlite
sqlite> SELECT name AS first_name, salary FROM employees;

first_name  salary    
----------  ----------
Alice       52000.0   
Bob         40000.0   
Carol       30000.0   
Dave        33000.0   
Eve         44000.0   
Frank       37000.0  
```

---

### arrange using ORDER BY

We can sort our results by adding `ORDER BY` to our `SELECT` statement

```sqlite
sqlite> SELECT name AS first_name, salary FROM employees ORDER BY salary;

first_name  salary    
----------  ----------
Carol       30000.0   
Dave        33000.0   
Frank       37000.0   
Bob         40000.0   
Eve         44000.0   
Alice       52000.0  
```

---

We can sort in the opposite order by adding `DESC` 

```sqlite
SELECT name AS first_name, salary FROM employees ORDER BY salary DESC;

first_name  salary    
----------  ----------
Alice       52000.0   
Eve         44000.0   
Bob         40000.0   
Frank       37000.0   
Dave        33000.0   
Carol       30000.0  
```

---

### filter via WHERE

We can filter rows by adding `WHERE` to our statements

```sqlite
sqlite> SELECT * FROM employees WHERE salary < 40000;

name        email              salary      dept      
----------  -----------------  ----------  ----------
Carol       carol@company.com  30000.0     Sales     
Dave        dave@company.com   33000.0     Accounting
Frank       frank@comany.com   37000.0     Sales  

sqlite> SELECT * FROM employees WHERE salary < 40000 AND dept = "Sales";

name        email              salary      dept      
----------  -----------------  ----------  ----------
Carol       carol@company.com  30000.0     Sales     
Frank       frank@comany.com   37000.0     Sales 
```

---

### group_by via GROUP BY

We can create groups for the purpose of summarizing using `GROUP BY`. As with dplyr it is not terribly useful by itself.

```sqlite
sqlite> SELECT * FROM employees GROUP BY dept;

name        email             salary      dept      
----------  ----------------  ----------  ----------
Dave        dave@company.com  33000.0     Accounting
Frank       frank@comany.com  37000.0     Sales     

sqlite> SELECT dept, AVG(salary) AS mean_salary, COUNT(*) AS n FROM employees GROUP BY dept;

dept        mean_salary       n         
----------  ----------------  ----------
Accounting  41666.6666666667  3         
Sales       37000.0           3   
```


---

### head via LIMIT

We can limit the number of rows we get by using `LIMIT`.

```sqlite
sqlite> SELECT * FROM employees LIMIT 3;

name        email              salary      dept      
----------  -----------------  ----------  ----------
Alice       alice@company.com  52000.0     Accounting
Bob         bob@company.com    40000.0     Accounting
Carol       carol@company.com  30000.0     Sales 

sqlite> SELECT * FROM employees ORDER BY name DESC LIMIT 3;

name        email             salary      dept      
----------  ----------------  ----------  ----------
Frank       frank@comany.com  37000.0     Sales     
Eve         eve@company.com   44000.0     Sales     
Dave        dave@company.com  33000.0     Accounting
```



---

### Import CSV files

```sqlite
sqlite> .mode csv
sqlite> .import phone.csv phone
sqlite> .tables

employees  phone   

sqlite> .mode column
sqlite> SELECT * FROM phone;

name        phone       
----------  ------------
Bob         919 555-1111
Carol       919 555-2222
Eve         919 555-3333
Frank       919 555-4444
```

---

### Joins - Default

By default SQLite uses a `CROSS JOIN` which is not terribly useful

```sqlite
sqlite> SELECT * FROM employees JOIN phone;

name        email              salary      dept        name        phone       
----------  -----------------  ----------  ----------  ----------  ------------
Alice       alice@company.com  52000.0     Accounting  Bob         919 555-1111
Alice       alice@company.com  52000.0     Accounting  Carol       919 555-2222
Alice       alice@company.com  52000.0     Accounting  Eve         919 555-3333
Alice       alice@company.com  52000.0     Accounting  Frank       919 555-4444
Bob         bob@company.com    40000.0     Accounting  Bob         919 555-1111
Bob         bob@company.com    40000.0     Accounting  Carol       919 555-2222
Bob         bob@company.com    40000.0     Accounting  Eve         919 555-3333
Bob         bob@company.com    40000.0     Accounting  Frank       919 555-4444
Carol       carol@company.com  30000.0     Sales       Bob         919 555-1111
Carol       carol@company.com  30000.0     Sales       Carol       919 555-2222
Carol       carol@company.com  30000.0     Sales       Eve         919 555-3333
Carol       carol@company.com  30000.0     Sales       Frank       919 555-4444
Dave        dave@company.com   33000.0     Accounting  Bob         919 555-1111
Dave        dave@company.com   33000.0     Accounting  Carol       919 555-2222
Dave        dave@company.com   33000.0     Accounting  Eve         919 555-3333
Dave        dave@company.com   33000.0     Accounting  Frank       919 555-4444
Eve         eve@company.com    44000.0     Sales       Bob         919 555-1111
Eve         eve@company.com    44000.0     Sales       Carol       919 555-2222
Eve         eve@company.com    44000.0     Sales       Eve         919 555-3333
Eve         eve@company.com    44000.0     Sales       Frank       919 555-4444
Frank       frank@comany.com   37000.0     Sales       Bob         919 555-1111
Frank       frank@comany.com   37000.0     Sales       Carol       919 555-2222
Frank       frank@comany.com   37000.0     Sales       Eve         919 555-3333
Frank       frank@comany.com   37000.0     Sales       Frank       919 555-4444
```


---

### Inner Join

If you want SQLite to find the columns to merge on automatically then we prefix the join with `NATURAL`.

```sqlite
sqlite> SELECT * FROM employees NATURAL JOIN phone;

name        email            salary      dept        phone       
----------  ---------------  ----------  ----------  ------------
Bob         bob@company.com  40000.0     Accounting  919 555-1111
Carol       carol@company.c  30000.0     Sales       919 555-2222
Eve         eve@company.com  44000.0     Sales       919 555-3333
Frank       frank@comany.co  37000.0     Sales       919 555-4444
```

---

### Inner Join - Explicit

```sqlite
sqlite> SELECT * FROM employees JOIN phone ON employees.name = phone.name;

name        email            salary      dept        name        phone       
----------  ---------------  ----------  ----------  ----------  ------------
Bob         bob@company.com  40000.0     Accounting  Bob         919 555-1111
Carol       carol@company.c  30000.0     Sales       Carol       919 555-2222
Eve         eve@company.com  44000.0     Sales       Eve         919 555-3333
Frank       frank@comany.co  37000.0     Sales       Frank       919 555-4444
```


---

### Left Join - Natural

```sqlite
sqlite> SELECT * FROM employees NATURAL LEFT JOIN phone;

name        email              salary      dept        phone     
----------  -----------------  ----------  ----------  ----------
Alice       alice@company.com  52000.0     Accounting            
Bob         bob@company.com    40000.0     Accounting  919 555-11
Carol       carol@company.com  30000.0     Sales       919 555-22
Dave        dave@company.com   33000.0     Accounting            
Eve         eve@company.com    44000.0     Sales       919 555-33
Frank       frank@comany.com   37000.0     Sales       919 555-44
```

---

### Left Join - Explicit

```sqlite
sqlite> SELECT * FROM employees LEFT JOIN phone 
  ON employees.name = phone.name;

name        email              salary      dept        name        phone     
----------  -----------------  ----------  ----------  ----------  ----------
Alice       alice@company.com  52000.0     Accounting                        
Bob         bob@company.com    40000.0     Accounting  Bob         919 555-11
Carol       carol@company.com  30000.0     Sales       Carol       919 555-22
Dave        dave@company.com   33000.0     Accounting                        
Eve         eve@company.com    44000.0     Sales       Eve         919 555-33
Frank       frank@comany.com   37000.0     Sales       Frank       919 555-44
```

---

### Other Joins

Note that SQLite does not support directly support an `OUTER JOIN` or a `RIGHT JOIN`.


---

### Creating an index

```sqlite
sqlite> CREATE INDEX index_name ON employees (name);
sqlite> .indices

index_name

sqlite> CREATE INDEX index_name_email ON employees (name,email);
sqlite> .indices

index_name
index_name_email
```

---

### Subqueries

We can nest tables within tables for the purpose of queries.

```sqlite
SELECT * FROM (SELECT * FROM employees NATURAL LEFT JOIN phone) WHERE phone IS NULL;

name        email              salary      dept        phone     
----------  -----------------  ----------  ----------  ----------
Alice       alice@company.com  52000.0     Accounting            
Dave        dave@company.com   33000.0     Accounting            
```

```sqlite
sqlite> SELECT * FROM (SELECT * FROM employees NATURAL LEFT JOIN phone) WHERE phone IS NOT NULL;

name        email            salary      dept        phone       
----------  ---------------  ----------  ----------  ------------
Bob         bob@company.com  40000.0     Accounting  919 555-1111
Carol       carol@company.c  30000.0     Sales       919 555-2222
Eve         eve@company.com  44000.0     Sales       919 555-3333
Frank       frank@comany.co  37000.0     Sales       919 555-4444
```

---

### Excercise - Stupid SQL Tricks

Lets try to create a table that has a new column - `abv_avg` which contains how much more (or less) than the average, for their department, each person is paid.

Hint - This will require joining a subquery.


`employees.sqlite` is available in the exercises repo.

<!--
```sqlite
sqlite> SELECT *, salary-avg AS diff FROM employees NATURAL JOIN  (SELECT dept, ROUND(AVG(salary),2) AS avg FROM employees GROUP BY dept);

name        email              salary      dept        avg         diff      
----------  -----------------  ----------  ----------  ----------  ----------
Alice       alice@company.com  52000.0     Accounting  41666.67    10333.33  
Bob         bob@company.com    40000.0     Accounting  41666.67    -1666.67  
Carol       carol@company.com  30000.0     Sales       37000.0     -7000.0   
Dave        dave@company.com   33000.0     Accounting  41666.67    -8666.67  
Eve         eve@company.com    44000.0     Sales       37000.0     7000.0    
Frank       frank@comany.com   37000.0     Sales       37000.0     0.0 
```
-->


## Acknowledgments

Above materials are derived in part from the following sources:

* Colin Rundel - [Sta 323 - Statistical Computing](http://www2.stat.duke.edu/~cr173/Sta323_Sp18/slides/Lec15_sql.html#1)
