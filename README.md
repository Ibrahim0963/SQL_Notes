# SQL_Notes


-- First Session 1
-- ctrl + k + c -> comment selected
-- ctrl + k + u -> uncomment selected

-- SELECT | ORDER BY ASC, DESC
SELECT * FROM Customers
ORDER BY FirstName ASC, LastName DESC
-- ASC für Aufsteigend, DESC für Absteigend


-- SELECT | WHERE CONDITION | ORDER BY 
SELECT Name, UnitPrice
FROM Products
WHERE UnitPrice > 100
ORDER BY UnitPrice 

/* -- Syntax
SELECT *

FROM "TABLE"

ORDER BY "FIELD"
*/ 

-- IS is only used with NULL 
--- Select all but CostumerID 2 
SELECT * FROM Customers WHERE CustomerID <> 2;

-- #####################################################################
-- CTEATED BY: Ibrahim Alakeel - - part 0
-- CREATED ON: 7/8/2021
-- #####################################################################

-- SELECT
SELECT *
FROM Customers

SELECT FirstName , LastName
FROM Customers



-- #####################################################################
-- CREATED BY: Ibrahim Alakeel - - part 2
-- CRETED ON: 9/8/2021
-- #####################################################################

SELECT FirstName , LastName FROM Customers

-- ORDER BY
SELECT * FROM Customers ORDER BY FirstName ASC

SELECT * FROM Customers ORDER BY FirstName DESC

-- ORDER BY FirstName ASC, if there is same FirstName then order by LastName DESC
SELECT * FROM Customers ORDER BY FirstName ASC , LastName DESC

SELECT * FROM Customers ORDER BY FirstName ASC , LastName ASC

-- WHERE
SELECT * FROM Products WHERE UnitPrice > 100

SELECT Name , UnitPrice FROM Products WHERE UnitPrice > 100 ORDER BY UnitPrice DESC

-- AND
SELECT * FROM Products WHERE ProductID > 5 AND UnitPrice > 100

-- OR
SELECT * FROM Products WHERE ProductID > 5 OR UnitPrice > 100

-- NOT - Comes before condition or (), to not a condition or a sentence 
-- Just put it inside () and write NOT before
SELECT * FROM Products WHERE NOT ( ProductID > 5 AND UnitPrice > 100)

-- IS is only used with NULL 
SELECT * FROM Invoices WHERE CustomerID IS NULL

SELECT * FROM Invoices WHERE CustomerID IS NOT NULL

-- BETWEEN - Check For Range
SELECT * FROM Products WHERE UnitPrice BETWEEN 18 AND 140 ORDER BY UnitPrice ASC

-- IN - Check For many specific values (Array)
SELECT * FROM Customers WHERE FirstName IN ('Ahmed', 'Karim', 'Salim') ORDER BY LastName ASC

-- LIKE comes always with string, equal = comes always with numbers(date, time+), IS comes always NULL
SELECT * FROM Customers WHERE FirstName LIKE 'Ahmed'

-- WildCards

-- Anything starts with D
SELECT * FROM Customers WHERE FirstName LIKE 'D%'

-- Anything ends with Y
SELECT * FROM Customers WHERE FirstName LIKE '%Y'

-- Anything contains N
SELECT * FROM Customers WHERE FirstName LIKE '%N%'

-- underscore _ for unkown character
SELECT * FROM Customers WHERE FirstName LIKE 'S_RA'

-- Range start with 
SELECT * FROM Customers WHERE FirstName LIKE '[A-F]%'

-- Range does not start with 
SELECT * FROM Customers WHERE FirstName LIKE '[^A-F]%'

-- ALIAS 
SELECT InvoiceNumber AS 'INVOICE NUMBER', InvoiceDate AS 'تاريخ الفاتورة' FROM Invoices

-- creating new virtual column 
SELECT Name, UnitPrice, (UnitPrice + (UnitPrice * 0.1)) AS "New Price" FROM Products

SELECT (FirstName + ' '+ LastName) AS 'FULL NAME' FROM Employees


-- #####################################################################
-- CREATED BY: Ibrahim Alakeel - part 3
-- CREATED ON: 11/8/2021
-- #####################################################################

-- AGGRIGATION FUNCTIONS
SELECT MAX(UnitPrice) AS 'MAXIMUM PRICE'
FROM Products

SELECT MIN(UnitPrice) AS 'MINIMUM PRICE'
FROM Products

SELECT AVG(UnitPrice) AS 'PRICES AVERAGE'
FROM Products

-- 1+2+3+4+5 - Additional all value of a specific column
SELECT SUM(Quantity) AS 'NUMBER OF SOLD UNITS'
FROM InvoicesDetails

-- Count How many rows in a specific column - Anzahl der Werte in einer Spalte zählen
SELECT COUNT(CustomerID) AS 'NUMBER OF INVOICES'
FROM Invoices

-- With distinct, all different values sum - Mit distinct Alle Unterschiedliche Werte zählen
SELECT COUNT(distinct CustomerID) AS 'NUMBER OF INVOICES'
FROM Invoices

SELECT COUNT(InvoiceID) AS 'NUMBER OF INVOICES'
FROM Invoices

-- Using many AGGRIGATION FUNCTIONS 
SELECT MAX(UnitPrice) AS 'MAXIMUM PRICE' , MIN(UnitPrice) AS 'MINIMUM PRICE' , AVG(UnitPrice) AS 'PRICES AVERAGE'
FROM Products

-- GROUP BY - GROUP BY Syntax
/* 
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
*/

-- Important!!!! aggrigation function does not work with WHERE 
-- aggrigation function works with having instead
-- To make it work you have to use GROUP BY
-- GROUP BY will count all same value then but them in a new column then order them ASC or DESC
-- but why ? because the result we got from COUNT(InvoiceID) are less row then EmployeeID row
-- so we got the error, it is logical error
SELECT EmployeeID ,COUNT(InvoiceID) AS 'NUMBER OF INVOICES' --Does not work
FROM Invoices
WHERE EmployeeID = 3

SELECT EmployeeID ,COUNT(InvoiceID) AS 'NUMBER OF INVOICES' -- Does work with GROUP BY
FROM Invoices
WHERE EmployeeID = 3
GROUP BY EmployeeID

-- Test 
SELECT EmployeeID
FROM Invoices -- Test
SELECT EmployeeID , COUNT(InvoiceID) AS 'NUMBER OF INVOICES'
FROM Invoices
GROUP BY EmployeeID

SELECT InvoiceID , SUM(Quantity) AS 'NUMBER OF SOLD UNITS'
FROM InvoicesDetails
GROUP BY InvoiceID

SELECT InvoiceID , COUNT(ProductID) AS 'NUMBER OF SOLD ITEMS' , SUM(Quantity) AS 'NUMBER OF SOLD UNITS' 
FROM InvoicesDetails
GROUP BY InvoiceID

SELECT InvoiceID , COUNT(ProductID) AS 'NUMBER OF SOLD ITEMS' , SUM(Quantity) AS 'NUMBER OF SOLD UNITS' 
FROM InvoicesDetails
GROUP BY InvoiceID
-- ORDER BY 'NUMBER OF SOLD UNITS' DESC
ORDER BY SUM(Quantity)  DESC

-- can't use WHERE Statement with GROUP BY, so we use HAVING instead
-- can't use WHERE Statement with aggrigation function, so we use HAVING instead
SELECT InvoiceID , COUNT(ProductID) AS 'NUMBER OF SOLD ITEMS' , SUM(Quantity) AS 'NUMBER OF SOLD UNITS' 
FROM InvoicesDetails
GROUP BY InvoiceID
HAVING SUM(Quantity) > 10

SELECT InvoiceID , COUNT(ProductID) AS 'NUMBER OF SOLD ITEMS' , SUM(Quantity) AS 'NUMBER OF SOLD UNITS' 
FROM InvoicesDetails
WHERE InvoiceID >= 2
GROUP BY InvoiceID

SELECT InvoiceID , COUNT(ProductID) AS 'NUMBER OF SOLD ITEMS' , SUM(Quantity) AS 'NUMBER OF SOLD UNITS' 
FROM InvoicesDetails
WHERE InvoiceID >= 2
GROUP BY InvoiceID
HAVING SUM(Quantity) > 10
ORDER BY SUM(Quantity)  ASC
/*
note for the above code. 
We ask database to get some information from InvoiceDetails WHERE InvoiceID eqaul or bigger then 2
GROUP BY ?
and Have the summation of the Quantity bigger then 10. we used here HAVING instead of WHERE 
because we are using aggrigation function 
Finally we ask the code to order the asked information
from database by the summation of the Quantity
*/


--HAVING used aggrigation condition + group by vs. WHERE comes after regular condition + after any where 
--GROUP BY used, when you have one row in one column and many rows in another column


-- #####################################################################
-- CREATED BY: Ibrahim Alakeel - part 4
-- CRETAED ON: 14/8/2021
-- #####################################################################


-- JOINS
SELECT  *
FROM Invoices

SELECT *
FROM Customers

-- INNER JOIN - Get invoices that has costumers and ignore these without customers 
SELECT Invoices.InvoiceNumber , Invoices.InvoiceDate , Customers.FirstName , Customers.LastName
FROM Invoices INNER JOIN Customers
ON Invoices.CustomerID = Customers.CustomerID

-- LEFT OUTER JOIN - Get all  invoices فاتورة ,Does not matter if the invoices has a customer or not
SELECT Invoices.InvoiceNumber , Invoices.InvoiceDate , Customers.FirstName , Customers.LastName
FROM Invoices LEFT OUTER JOIN Customers
ON Invoices.CustomerID = Customers.CustomerID 

-- RIGHT OUTER JOIN - Get all costumers, Does not matter if the costumers has a invoices or not
SELECT Invoices.InvoiceNumber , Invoices.InvoiceDate , Customers.FirstName , Customers.LastName
FROM Invoices RIGHT OUTER JOIN Customers
ON Invoices.CustomerID = Customers.CustomerID

-- FULL OUTER JOIN - Get all customers and all invoices - Get everything from the right and the left
SELECT Invoices.InvoiceNumber , Invoices.InvoiceDate , Customers.FirstName , Customers.LastName
FROM Invoices FULL OUTER JOIN Customers
ON Invoices.CustomerID = Customers.CustomerID

-- ALIAS TABLE ( TABLE_NAME AS ALIAS_NAME )
SELECT I.InvoiceNumber , I.InvoiceDate , CONCAT( C.FirstName ,' ', C.LastName) AS 'FULL NAME'
FROM Invoices AS I FULL OUTER JOIN Customers AS C
ON I.CustomerID = C.CustomerID

SELECT Products.Name , Products.UnitPrice , InvoicesDetails.Quantity
FROM Invoices INNER JOIN InvoicesDetails 
ON Invoices.InvoiceID = InvoicesDetails.InvoiceID INNER JOIN Products -- Second INNER JOIN bind InvoicesDetails with Products
ON InvoicesDetails.ProductID = Products.ProductID
WHERE Invoices.InvoiceNumber = '100A'

SELECT  SUM(Products.UnitPrice * InvoicesDetails.Quantity) AS 'TOTAL'
FROM  Invoices INNER JOIN InvoicesDetails 
ON  Invoices.InvoiceID = InvoicesDetails.InvoiceID INNER JOIN Products
ON InvoicesDetails.ProductID = Products.ProductID
WHERE Invoices.InvoiceNumber = '101A'

SELECT Invoices.InvoiceNumber,  SUM(Products.UnitPrice * InvoicesDetails.Quantity) AS 'TOTAL'
FROM  Invoices INNER JOIN InvoicesDetails 
ON  Invoices.InvoiceID = InvoicesDetails.InvoiceID INNER JOIN Products
ON InvoicesDetails.ProductID = Products.ProductID
GROUP BY Invoices.InvoiceNumber
HAVING SUM(Products.UnitPrice * InvoicesDetails.Quantity) > 30


-- SELECT INTO -- Copy a complete database / columns into another database
SELECT *
FROM Customers

SELECT FirstName , LastName
INTO DokkiCustomers
FROM Customers
WHERE CustomerID <= 7

-- TEST
SELECT *
FROM DokkiCustomers

SELECT *
FROM Customers


-- #####################################################################
---- CREATED BY: Ibrahim Alakeel - part 5
---- CREATED ON: 16/8/2021
-- #####################################################################


-- UNION ALL - get all employees with repeated value
SELECT FirstName , LastName
FROM Customers
UNION ALL
SELECT FirstName, LastName
FROM DokkiCustomers

-- UNION - get all employees with ignore repeated value
SELECT FirstName , LastName
FROM Customers
UNION 
SELECT FirstName, LastName
FROM DokkiCustomers

-- INTERSECT
SELECT FirstName , LastName
FROM Customers
INTERSECT 
SELECT FirstName, LastName
FROM DokkiCustomers

-- EXCEPT
SELECT FirstName , LastName
FROM Customers
EXCEPT 
SELECT FirstName, LastName
FROM DokkiCustomers

SELECT FirstName , LastName
FROM DokkiCustomers
EXCEPT 
SELECT FirstName, LastName
FROM Customers

-- DISTINCT ignore the repeated value depend on row - row must be same in all columns 
SELECT DISTINCT Name
FROM Products

SELECT DISTINCT Name , UnitPrice
FROM Products

-- TOP
SELECT TOP 6 FirstName , LastName
FROM Customers

-- TOP - get first 3 rows -- by default it is ASC, no need for ORDER BY Customers ASC
SELECT TOP 3 FirstName, LastName
FROM Customers

-- TOP - get last 3 rows
SELECT TOP 3 Name
FROM Products
ORDER BY UnitPrice DESC

-- SUBQUERY
SELECT Name
FROM Products
WHERE UnitPrice = (SELECT MAX(UnitPrice) FROM Products)

SELECT Name
FROM Products
GROUP BY Name
HAVING COUNT(Name) > 1


-- INSERT
SELECT *
FROM Products

INSERT INTO Products
VALUES('Pencil' ,6)

-- TEST
SELECT *
FROM Products

-- MULTI INSERT
INSERT INTO Products(Name , UnitPrice)
VALUES('Pen' ,70),
      ('Apricot',35),
      ('Banana',15)

-- TEST
SELECT *
FROM Products


INSERT INTO Invoices(InvoiceNumber ,CustomerID , EmployeeID)
VALUES('109A', NULL , 1)

-- TEST
SELECT *
FROM Invoices

-- ERROR -> Because EmployeeID does not exists 
INSERT INTO Invoices(InvoiceNumber ,CustomerID , EmployeeID)
VALUES('110A', NULL , 100)
------ ERROR!
------ Msg 547, Level 16, State 0, Line 96
------The INSERT statement conflicted with the FOREIGN KEY constraint "FK_Invoices_Employees". The conflict occurred in database "GalaxyStore", table "dbo.Employees", column 'EmployeeID'.
------The statement has been terminated.

SELECT *
FROM Employees


-- UPDATE
SELECT *
FROM Products

UPDATE Products
SET UnitPrice = UnitPrice + (UnitPrice * 0.1)

-- TEST
SELECT *
FROM Products

UPDATE Products
SET UnitPrice = UnitPrice + (UnitPrice * 0.05)
WHERE UnitPrice > 100

-- TEST
SELECT *
FROM Products
WHERE UnitPrice > 100

UPDATE Products
SET Name = 'Mango', UnitPrice = 30
WHERE ProductID = 15

-- TEST
SELECT *
FROM Products
WHERE ProductID = 15


-- DELETE
SELECT *
FROM Products

DELETE FROM Products
WHERE ProductID = 16

-- TEST
SELECT *
FROM Products
WHERE ProductID = 2

-- Test
SELECT * 
FROM Products

-- ERROR!, because of the relationship with other tables
DELETE FROM Products
WHERE ProductID = 1
----- ERROR!
----- Msg 547, Level 16, State 0, Line 148
----- The DELETE statement conflicted with the REFERENCE constraint "FK_InvoicesDetails_Products". The conflict occurred in database "GalaxyStore", table "dbo.InvoicesDetails", column 'ProductID'.
----- The statement has been terminated.
-- Ibrahim: Error happened because ProductID is a foreign key in another table, so we can not delele it ?

-- TEST
SELECT *
FROM InvoicesDetails
WHERE ProductID = 1




-- #####################################################################
---- CREATED BY: Ibrahim Alakeel - part 6
---- CREATED ON: 18/8/2021
-- #####################################################################

-- Variables ( 1. Declare the name of the variable and set to it its type 
--			   2. set a value to the variable )
DECLARE @EmployeeID SMALLINT
SET @EmployeeID = 2
SELECT InvoiceNumber , InvoiceDate
FROM Invoices
WHERE EmployeeID = @EmployeeID

--tables vs. view
--tables : has relationship | real data | SELET, ISERT .... can be used 
--view : JOIN Line | reflection base data | ONLY SELECT can be used 

-- Edit -> IntelliSense -> Refresh local cache To show any changes on viewSELECT *

-- view is about a place to set customize tables in it 
-- and call them instead of using join (Remember only SELECT can by used in VIEW)
SELECT InvoiceNumber , CONCAT(FirstName, ' ' , LastName) AS 'FULL NAME'
FROM View_InvoicesCustomers

SELECT * 
FROM View_InvoicesCustomers


-- CREATE Entity EntityName AS Statements
-- ALTER Entity EntityName AS Statements
-- CREATE Entity EntityName

-- DROP - DELETE A Complete database
-- DROP
DROP VIEW View_InvoicesCustomers

-- CREATE - CREATE A Complete database
CREATE VIEW View_InvoicesCustomers
AS
SELECT Invoices.InvoiceNumber , Customers.FirstName , Customers.LastName
FROM Invoices INNER JOIN Customers
ON Invoices.CustomerID = Customers.CustomerID

-- TEST
SELECT *
FROM View_InvoicesCustomers

-- ALTER - UPDATE A Complete database
ALTER VIEW View_InvoicesCustomers
AS
SELECT Invoices.InvoiceNumber , Invoices.InvoiceDate , Customers.FirstName , Customers.LastName
FROM Invoices LEFT OUTER JOIN Customers
ON Invoices.CustomerID = Customers.CustomerID

-- TEST 
SELECT *
FROM View_InvoicesCustomers


-- STORED PROCEDURES - You can not do any of SELECT, DELETE, INSERT
-- To save a sql sentence inside it
-- BUILT-IN
EXEC sp_help
EXEC sp_tables

-- to search : difference between functions and store procedures
-- USER-DEFIEND
-- PARAMETERLESS
CREATE PROC usp_AllProducts
AS
SELECT NAME, UnitPrice
FROM Products
ORDER BY UnitPrice DESC

-- TEST
EXEC usp_AllProducts


-- INPUT-PARAMETER
CREATE PROC usp_InvoicesByEmployeeID @EmployeeID SMALLINT 
AS
SELECT InvoiceNumber , InvoiceDate
FROM Invoices
WHERE EmployeeID = @EmployeeID

-- TEST
EXEC usp_InvoicesByEmployeeID 1

EXEC usp_InvoicesByEmployeeID 2

EXEC usp_InvoicesByEmployeeID 3


-- OUTPUT-PARAMETER
-- INPUT OUTPUT-PARAMETER
CREATE PROC usp_NumberOfInvoicesByEmployeeID @EmployeeID SMALLINT , @InvoiceCount INT OUT
AS
SELECT @InvoiceCount = COUNT(InvoiceID)
FROM Invoices
WHERE EmployeeID = @EmployeeID

-- TEST
DECLARE @Result INT
EXEC usp_NumberOfInvoicesByEmployeeID 1 , @Result OUT
SELECT @Result AS 'NUMBER OF INVOICES' , (@Result + 1) AS 'ALL INVOICES'





/* 
Playlist: https://www.youtube.com/playlist?list=PLtPFQl_0853oNyMPC2Fd0K1uW9x41KQn6

https://www.youtube.com/watch?v=RZc4QSRRk98&ab_channel=CrackConcepts
Interview Questions
1. WHERE vs. HAVING
WHERE: filters rows | works on row's data, not on aggrigated data
EX: SELECT * FROM Employees WHERE EmployeeID => 3 
HAVING: Works on Aggrigated data 

2. What is aggrigated Functions ?
functions used to perform calculation on multiple rows of a single column 
it return a single value
EX: SELECT MAX(salary) FROM Employee

3. What is UNION ? 
UNION Operator combine result set of 2 or more select statements 
each select statement must have:
- Same number of columns
- columns must have similar datatype
- columns names must be in same order

4. UNION vs. UNION ALL
UNION: removes duplicate records
UNION ALL: does not remove duplicate records

5. IN vs. EXISTS
IN: is like multiple OR
OR-EX: SELECT * FROM Customers WHERE fname = "Ibrahim" OR fname = "Ali" OR fname = "Karim"
IN-EX: SELECT * FROM Customers WHERE fname in ("Ibrahim","Ali","Karim")

EXISTS: return either TRUE or FALSE value
EX: SELECT * FROM Customers WHERE EXISTS (SELECT city FROM table2 WHERE table2.id = customer.id)

IN -> compare on value to several values
EXISTS -> tells you whether a query returned any results

https://www.youtube.com/watch?v=s1QkS4PfiFg&ab_channel=CrackConcepts
6. ORDER BY vs. GROUP BY
ORDER BY:  Sorting ASC / DESC 
EX: SELECT * FROM Customers ORDER BY CustomerID DESC
GROUP BY: used with aggrigate functions + to filter more you can't use WHERE. You have to use HAVING
EX: SELECT Name,count(*) FROM Customers GROUP BY Name

7. JOIN vs. SUBQUERY
both are used to combine data from different tables into a single result
SUBQUERY: can SELECT only from first tables based on the second SELECT query (Slower)
EX: SELECT phone, fname, lname FROM Customers WHERE CustomerID in (SELECT CustomerID FROM.. ORDER..)

JOIN: can SELECT from either of the tables (Faster)
EX: SELECT phone, fname, lname FROM Customers AS C JOIN Orders AS O ON c.id = o.id 

8. JOIN vs. UNION
UNION: combine rows , not necessary to have same column name but no. of columns + 
								datatype of columns should be the same
JOIN: merges columns

*/
