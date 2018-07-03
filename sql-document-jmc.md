
**source**: https://www.w3schools.com/sql/sql_distinct.asp

---

## 02 Part II. count(table) >= 2

### 01 교집합

Q1. selects all customers that are from the same countries as the suppliers:

```sql
SELECT * FROM Customers
WHERE Country IN (SELECT Country FROM Suppliers);
```

### 02 일치 

```sql
SELECT c.CustomerID, c.CustomerName, o.OrderID, o.OrderDate
FROM Customers AS c, Orders AS o
WHERE c.CustomerID=o.CustomerID
ORDER BY c.CustomerID, o.OrderID;
```


---

## 01 Part I. count(table) == 1

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

## 00 Part 0. Basic Syntax

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
