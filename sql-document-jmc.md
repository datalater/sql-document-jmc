
**source**: https://www.w3schools.com/sql/sql_distinct.asp

---

## TABLE SCHEMA

**Customers (7 cols)**

+ CustomerID | CustomerName | ContactName | Address | City | PostalCode | Country

**Suppliers (8 cols)**

+ SupplierID | SupplierName | ContactName | Address | City | PostalCode | Country | Phone

**Orders (5 cols)**

+ OrderID | CustomerID | EmployeeID | OrderDate | ShipperID

---

### 01 공통된 Primary Key를 기준으로 외부 테이블의 칼럼 추가하기

NUM | Customers | Orders | diff
--|---:|---:|--:
TOTAL RECORDS | 91 | 196 | 105
DISTINCT CustomerID | 91 | 74 | 17

**Table1-1.** 테이블1

```sql
SELECT *
FROM Orders
ORDER BY CustomerID;

--Number of Records: 196
```

**Table1-2.** 테이블1 + 테이블2 `where(primaryKey)`

```sql
SELECT *
FROM Orders AS o, Customers AS c
WHERE o.CustomerID=c.CustomerID
ORDER BY o.CustomerID;

--Number of Records: 196
```

+ primaryKey가 겹치는 테이블1과 테이블2의 로우만 걸러내서 모든 칼럼을 함께 보여준다.

---

**Table1-3.** 테이블1 + `intersection(테이블1, 테이블2, primaryKey)`

```sql
SELECT *
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
ORDER BY CustomerID;

--Number of Records: 196
```

+ step1: primaryKey 기준으로 테이블1과 테이블2의 교집합(INNER) 로우를 찾는다.
+ step2: 교집합된 테이블2의 로우와 칼럼을 테이블1에 조인한다.

**Table1-4.** 테이블1 + `left(테이블1, 테이블2, primaryKey)`

```sql
SELECT *
FROM Orders
LEFT JOIN Customers ON Orders.CustomerID = Customers.CustomerID
ORDER BY CustomerID;

--Number of Records: 196
```

+ step1: primaryKey 기준으로 테이블1과 테이블2의 교집합 로우를 찾는다.
+ step2: 왼쪽(LEFT) 테이블에 있는 모든 로우를 뱉어내야 한다.
+ step3: 교집합된 테이블2의 로우와 칼럼을 왼쪽 테이블1에 조인한다.
+ step4: 교집합되지 않은 왼쪽(LEFT) 테이블의 로우 또한 테이블1에 조인한다.

**Table1-5.** 테이블1 + `intersection(테이블1+테이블2, 테이블3, primaryKey)`

```sql
SELECT *
FROM ((Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)
INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID)
ORDER BY Orders.CustomerID;

--Number of Records: 196
```

+ step1: primaryKey 기준으로 테이블1과 테이블 2의 교집합(INNER) 로우를 찾는다.
+ step2: 교집합된 테이블2의 로우와 칼럼을 테이블1에 조인한다.
+ step3: primaryKey 기준으로 step2 테이블과 테이블3의 교집합(INNER) 로우를 찾는다.
+ step4: 교집합된 테이블3의 로우와 칼럼을 step2 테이블에 조인한다.

**Table1-6.** 테이블1 + `right(테이블1, 테이블2, primaryKey)`

```sql
SELECT *
FROM Orders
RIGHT JOIN Customers ON Orders.CustomerID = Customers.CustomerID
ORDER BY Orders.CustomerID;

--Number of Records: 213
```

+ step1: primaryKey 기준으로 테이블1과 테이블2의 교집합 로우를 찾는다.
+ step2: 오른쪽(RIGHT) 테이블에 있는 모든 로우를 뱉어내야 한다.
+ step3: 교집합된 테이블2의 로우와 칼럼을 오른쪽 테이블1에 조인한다.
+ step4: 교집합되지 않은 오른쪽(RIGHT) 테이블의 로우 또한 테이블1에 조인한다.

@@@TODO. SELF JOIN

---
---
---

### 02 추가

Q. `fixed`: Customers.ALL `added`: Suppliers.ALL `condition`: Customers.Country = Suppliers.Country

```sql
SELECT * FROM Customers
INNER JOIN Suppliers ON Customers.Country = Suppliers.Country;

--result_set : 165
```

Q. `fixed`: Orders.ALL `added`: Customers.ALL `condition`: Orders.CustomerID = Customers.CustomerID

```sql
SELECT * FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

### 02 일치 by records

---

# 01 Part I. count(table) == 1

### 01 Combine columns

Q1. creates an alias named "Address" that combine four columns (Address, PostalCode, City and Country):

```sql
SELECT CustomerName, Address + ', ' + PostalCode + ' ' + City + ', ' + Country AS Address
FROM Customers;
```

```sql
--mysql

SELECT CustomerName, CONCAT(Address,', ',PostalCode,', ',City,', ',Country) AS Address
FROM Customers;
```

---
---
---
---
---

# 00 Part 0. Basic Syntax

### 01 SELECT DISTINCT

Q1. list the different (distinct) values

```sql
SELECT DISTINCT Country
FROM Customers;
```

Q2. lists the number of different (distinct) customer countries

```sql
SELECT COUNT(DISTINCT Country)
FROM Customers;
```


### 02 WHERE, IN

> **Note**: The WHERE clause is used to extract only those **records** that fulfill a specified condition.

Q3. selects all the customers from the country "Mexico", in the "Customers" table

```sql
SELECT * FROM Customers
WHERE Country='Mexico';
```

Q4. **caution**: numeric fields should not be enclosed in quotes

```sql
SELECT * FROM Customers
WHERE CustomerID=1;
```

Q27. selects all customers that are located in "Germany", "France" and "UK"

```sql
SELECT * FROM Customers
WHERE Country IN ('Germany', 'France', 'UK');
```

Q28. selects all customers that are NOT located in "Germany", "France" or "UK"

```sql
SELECT * FROM Customers
WHERE Country NOT IN ('Germany', 'France', 'UK');
```

### 03 AND, OR, NOT

Q5. selects specific records depending on the conditions

```sql
SELECT * FROM Customers
WHERE Country='Germany' AND (City='Berlin' OR City='München');
```

Q6. selects specific records depending on the negative conditions

```sql
SELECT * FROM Customers
WHERE NOT Country='Germany' AND NOT Country='USA';
```

### 04 ORDER BY

Q7. displays by specific order (ascending or descending)

```sql
SELECT * FROM Customers
ORDER BY Country ASC, CustomerName DESC;
```

### 05 INSERT INTO

Q8. inserts new records with the whole fields

```sql
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');
```

Q9. inserts new records with some fields nulled

```sql
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Cardinal', 'Stavanger', 'Norway');
```

> **Note**: columns that are not written is automatically updated as null

### 06 IS NULL, IS NOT NULL

Q10. list all persons that have no address

```sql
SELECT LastName, FirstName, Address FROM Persons
WHERE Address IS NULL;
```

Q11. list all persons that do have an address

```sql
SELECT LastName, FirstName, Address FROM Persons
WHERE Address IS NOT NULL;
```

### 07 UPDATE

Q12. updates the first customer (CustomerID = 1) with a new contact person and a new city

```sql
UPDATE Customers
SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
WHERE CustomerID = 1;
```

> **Note**: The WHERE clause specifies which record(s) that should be updated. If you omit the WHERE clause, all records in the table will be updated!

Q13. updates the ContactName to "Juan" for all records where country is "Mexico"

```sql
UPDATE Customers
SET ContactName='Juan'
WHERE Country='Mexico';
```

### 08 DELETE

Q14. deletes the customer "Alfreds Futterkiste" from the "Customers" table

```sql
DELETE FROM Customers
WHERE CustomerName='Alfreds Futterkiste';
```

### 09 TOP, LIMIT

Q15. selects the first three records from the "Customers" table

```sql
SELECT TOP 3 * FROM Customers;
```

Q16. shows the equivalent example using the LIMIT clause

```sql
SELECT * FROM Customers
LIMIT 3;
```

Q17. shows the equivalent example using ROWNUM

```sql
SELECT * FROM Customers
WHERE ROWNUM <= 3;
```

Q18. selects the first three records from the "Customers" table, where the country is "Germany

```sql
SELECT * FROM Customers
WHERE Country='Germany'
LIMIT 3;
```

### 10 LIKE, Wildcards

Q19. selects all customers with a CustomerName starting with "a

```sql
SELECT * FROM Customers
WHERE CustomerName LIKE 'a%';
```

Q20. The following SQL statement selects all customers with a CustomerName that have "or" in any position

```sql
SELECT * FROM Customers
WHERE CustomerName LIKE '%or%';
```

Q21. selects all customers with a CustomerName that have "r" in the second position

```sql
SELECT * FROM Customers
WHERE CustomerName LIKE '_r%';
```

+ `%` : The percent sign represents zero, one, or multiple characters
+ `_` : The underscore represents a single character

Q22. selects all customers with a CustomerName that starts with "a" and are at least 3 characters in length

```sql
SELECT * FROM Customers
WHERE CustomerName LIKE 'a_%_%';
```

Q23. selects all customers with a CustomerName ending with "a"

```sql
SELECT * FROM Customers
WHERE CustomerName LIKE '%a';
```

Q24. selects all customers with a City starting with "b", "s", or "p"

```sql
SELECT * FROM Customers
WHERE City LIKE '[bsp]%';
```

Q25. selects all customers with a City starting with "a", "b", or "c":

```sql
SELECT * FROM Customers
WHERE City LIKE '[a-c]%';
```

Q26. select all customers with a City NOT starting with "b", "s", or "p

```sql
SELECT * FROM Customers
WHERE City LIKE '[!bsp]%';
```

```sql
SELECT * FROM Customers
WHERE City NOT LIKE '[bsp]%';
```

### 11 BETWEEN, IN

Q. selects all products with a price BETWEEN 10 and 20. In addition; do not show products with a CategoryID of 1,2, or 3

```sql
SELECT * FROM Products
WHERE (Price BETWEEN 10 AND 20)
AND NOT CategoryID IN (1,2,3);
```

Q. selects all orders with an OrderDate BETWEEN '04-July-1996' and '09-July-1996'

```sql
SELECT * FROM Orders
WHERE OrderDate BETWEEN #07/04/1996# AND #07/09/1996#;
```

### 12 Aliases

Q. creates two aliases, one for the CustomerID column and one for the CustomerName column:

```sql
SELECT CustomerID AS ID, CustomerName AS Customer
FROM Customers;
```

> **Note**: It requires double quotation marks or square brackets if the alias name contains spaces.



---





















---
