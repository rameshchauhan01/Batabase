### Multiple table join Query:<br/>
    SELECT t1.col, t3.col 
    FROM table1 t1 
    JOIN table2 t2 ON t1.primarykey = t2.foreignkey                                  
    JOIN table3 t3 ON t2.primarykey = t3.foreignkey
### Retrive the data from same table bases on condition: <br/>
	SELECT a.ROLL_NO , b.NAME
	FROM Student a, Student b
	WHERE a.ROLL_NO < b.ROLL_NO;	


### Self Join:
	You can self-join the table to get the manager's name from his ID
		SELECT e.employee_name, m.employee_name AS manager_name
		FROM   employee e
		JOIN   employee m on e.manager_id = m.employee_id
### Right Join:
	This join returns all the rows of the table on the right side of the join 
		ELECT table1.column1,table2.column2
		FROM table1 
		RIGHT JOIN table2
		ON table1.matching_column = table2.matching_column
	Note: table1 column may have the null value but table2 column can't have null values
	
### Stored Procedure(SP): <br/>
A stored procedure is a prepared SQL code that you can save, so the code can be reused over and over again.
	* Create SP With Parameter */
		CREATE PROCEDURE SelectAllCustomers @City nvarchar(30)
		AS
		SELECT * FROM Customers WHERE City = @City
		GO;
    * Execute SP */
		EXEC SelectAllCustomers @City = 'London';
    

### Delete vs Truncate vs Drop: <br/>
	DELETE removes rows one by one depending upon WHERE condition or delete all rows if there is no WHERE condition. Before commit operation ,if we roll back we will get the data. It is a data manipulation language (DML) command. <br/>
	TRUNCATE removes all rows at once . It is a DDL command <br/>
	DROP command removes a table or database completely from database.It is a DDL command
### Built in functions: <br/>
	Count, Avg,Sum,Min,Max
	To find the average of two columns in SQL <br/>
	SELECT AVG(column1 + column2) as Average FROM table_name;
### SQL CASE or IF Else: <br/>
	SELECT OrderID, Quantity,
	CASE
    		WHEN Quantity > 30 THEN 'The quantity is greater than 30'
    		WHEN Quantity = 30 THEN 'The quantity is 30'
    	ELSE 'The quantity is under 30'
	END AS QuantityText
	FROM OrderDetails;
### Find the  nth maximum salary from emp table: <br/>
	* using max inbuilt function*/
		SELECT MAX (Salary) as SecondHighestSalary
		FROM Employee
		WHERE Salary < (SELECT MAX (Salary) FROM Employee);

	* using ROW_NUMBER() inbuilt function*/
		SELECT Salary as secondhigestsalary
		FROM(SELECT Salary,EmpName,ROW_NUMBER() OVER(ORDER BY Salary DESC) As RN 
		FROM EMPLOYEE )AS A
		WHERE A.RN = 2

    * Delete the duplicate records from a table */
		WITH CTE AS
		(
		SELECT *,ROW_NUMBER() OVER (PARTITION BY rollNumber ORDER BY rollNumber) AS RN
		FROM tbl_Student
		)
		DELETE FROM CTE WHERE RN<>1
		Note: A cte is normally not materialized anywhere. It's more like an inline view or named subquery

### Counting the duplicate value in table: <br/>
	* SQL | Distinct Clause*/
		SELECT DISTINCT column1,column2 
		FROM table_name 
	* example              */
		SELECT DISTINCT rollnumber,COUNT(*) AS [Number of Duplcates]
		FROM tbl_Student
		GROUP BY rollNumber

DISTINCT is useful in certain circumstances, but it has drawback that it can increase load on the query engine to perform the sort (since it needs to compare the result set to itself to remove duplicates)

### The SQL HAVING Clause: <br/>
	The HAVING clause was added to SQL because the WHERE keyword could not be used with aggregate functions.
	SELECT COUNT(CustomerID), Country
	FROM Customers
	GROUP BY Country
	HAVING COUNT(CustomerID) > 5;

### SQL Wildcard Characters: <br>
	A wildcard character is used to substitute one or more characters in a string. e.g: %,-,[],^ etc



### where Clause operators: <br/>
	<>	Not equal. Note: In some versions of SQL this operator may be written as !=	
	BETWEEN	Between a certain range	
	LIKE	Search for a pattern	
	IN	To specify multiple possible values for a column
	Any The ANY operator returns TRUE if any of the subquery values meet the condition.
	All The ALL operator returns TRUE if all of the subquery values meet the condition.
	EXISTS The EXISTS operator returns true if the subquery returns one or more records.
	IS NULL
	IS NOT NULL
	
Note: 
	WHERE clause is not only used in SELECT statement, it is also used in UPDATE, DELETE statement, etc.!
	SQL Keywords are case-insensitive (SELECT, FROM, WHERE, etc), but are often written in all caps

### Update syntax in SQL: <br/>
	UPDATE table_name
	SET column1 = value1, column2 = value2, ...
	WHERE condition;
	
### What is a DML command? <br/>
	It stands for Data Manipulation Language. The DML commands deal with the manipulation of existing records of a database. 
	SELECT: This command is used to extract information from a table.
	INSERT: It is a SQL query that allows us to add data into a table's row.
	UPDATE: This command is used to alter or modify the contents of a table.
	DELETE: This command is used to delete records from a database table, either individually or in groups.	

### What is a DDL command? <br/>
	DDL stands for Data Definition Language. As the name suggests, the DDL commands help to define the structure of the databases or schema. 
	CREATE: It is used to create a new database and its objects such as table, views, function, stored procedure, triggers, etc.
	DROP: It is used to delete the database and its objects, including structures, from the server permanently.
	ALTER: It's used to update the database structure by modifying the characteristics of an existing attribute or adding new attributes.
	TRUNCATE: It is used to completely remove all data from a table, including their structure and space allocates on the server.
	RENAME: This command renames the content in the database.

The following points explain the main differences between DDL and DML commands:<br/>
	Data Definition Language (DDL) statements describe the structure of a database or schema. Data Manipulation Language (DML) statements, on the other hand, allow altering data that already exists in the database.
	We use the DDL commands for creating the database or schema, while DML commands are used to populate and manipulate the database.
	DDL commands can affect the whole database or table, whereas DML statements only affect single or multiple rows based on the condition specified in a query.
	Since DDL commands are auto-committed, modifications are permanent and cannot be reversed. DML statements, on the other hand, are not auto-committed, which means that modifications are not permanent and can be reversed.
	DML is an imperative and procedural method, whereas DDL is a declarative method.
	The data in DML statements can be filtered with a WHERE clause, while the records in DDL statements cannot be filtered with a WHERE clause.
	
###  return the names of the people who are reported to (excluding null values), the number of members that report to them, and the average age of those members as an integer. The rows should be ordered by the names in alphabetical order  <br/>
Table:
ID	FirstName	LastName	ReportsTo	Position	Age
1	Daniel		Smith		Bob Boss	Engineer	25
2	Mike		White		Bob Boss	Contractor	22
3	Jenny		Richards	null		CEO		45
4	Robert		Black		Daniel Smith	Sales		22
5	Noah		Fritz		Jenny Richards	Assistant	30
6	David		S		Jenny Richards	Director	32
7	Ashley		Wells		David S		Assistant	25
8	Ashley		Johnson		null		Intern		25
Solution:
SELECT ReportsTo, count(ReportsTo) as Members,
 round(avg(Age), 0) as 'Average Age' 
 FROM maintable_SGDS8
Where ReportsTo IS NOT NULL
Group by ReportsTo
order by ReportsTo

