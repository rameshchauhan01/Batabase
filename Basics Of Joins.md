# The various types of joins are as follows  
1.SQL inner join or Join(Equi join,Non-equi join (Theta join))  
2.SQL outer join(SQL left join or left outer join,SQL right join or right outer join,SQL full join or full outer join)  
3.SQL cross join  
4.SQL self join  

Tables: Customer,Order


|CustID|CustName|ContactName|Addess|City|Postal Code|Country|
|--|----|-----------|------|----|-----------|-------|
|1 |Alfreds Futterkiste|Maria Anders|Obere Str. 57|Berlin|12209|Germany|
|2 |Ana Trujillo Emparedadosy helados|Ana Trujillo|Avda. de la Constitución 2222|Mexico D.F|05021|Mexico|
|3 |Antonio Moreno Taquería|Antonio Moreno|Antonio Moreno|Mexico D.F|05023|Mexico|

|OrderID|CustID|EmpID|OrderDate|ShipperID|
|-------|------|------|--------|---------|
|10308 |2|7|1996-09-18|3|
|10309 |37|3|1996-09-19|1|
|10310 |77|8|1996-09-20|2|


**Self Join:** Matches customers that are from the same city:

SELECT A.CustName AS CustName1, B.CustName AS CustName2, A.City  
FROM Customers A, Customers B  
WHERE A.CustID <> B.CustID   
AND A.City = B.City   
ORDER BY A.City;
