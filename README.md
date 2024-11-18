# gigachad

-- launch parameters --
use Northwind;


-- 1 -- 
SELECT
 CompanyName,
 Phone
FROM
 Shippers
ORDER BY
 CompanyName DESC


-- 2 --
SELECT
 ProductName,
 CategoryID,
 UnitPrice
FROM
 Products
WHERE
 CategoryID IN (1, 5, 7) 
 AND UnitPrice > 9
 AND ProductName LIKE 'L%'
ORDER BY RIGHT
 (ProductName, 1) DESC


-- 3 --
SELECT
 CompanyName AS 'Company Name',
 ContactName AS 'Contact Name',
 CONCAT(
  COALESCE(orders.ShipAddress, ''), ', ',
  COALESCE(orders.ShipRegion, ''), ', ',
  COALESCE(orders.ShipCity, ''), ', ',
  COALESCE(orders.ShipCountry, ''), ', '
 )
FROM
 Customers AS customers
JOIN
 Orders AS orders
 ON customers.CustomerID = orders.CustomerID
WHERE 
 CompanyName != ShipName
ORDER BY
 customers.CompanyName ASC,
 orders.OrderDate DESC


-- 4 --
SELECT
 COUNT(ProductID) AS 'Number of Products',
 CategoryName AS 'Category Name',
 AVG(UnitPrice)
FROM
 Products AS products
JOIN
 Categories AS categories
 ON products.CategoryID = categories.CategoryID
WHERE
 products.UnitsInStock > products.UnitsOnOrder
GROUP BY 
 categories.CategoryID, categories.CategoryName
HAVING
 COUNT(products.ProductID) > 5
ORDER BY
 COUNT(products.ProductID) DESC


-- 5 --
select ProductName, COUNT(p.ProductID) "Cum" from Products p
join [Order Details] od  on p.ProductID = od.ProductID
where p.CategoryID = (select CategoryID from Products where ProductName = 'Tofu') and p.ProductName != 'Tofu'
group by ProductName
order by ProductName

-- 6 -- 
go
CREATE VIEW
 CustomersWithoutOrders 
AS SELECT
 customers.*
FROM
 Customers AS customers
LEFT JOIN
 Orders As orders
 ON customers.CustomerID = orders.CustomerID
WHERE
 orders.CustomerID IS NULL
go

SELECT * FROM CustomersWithoutOrders

DROP VIEW CustomersWithoutOrders

--VIEW with join and aggregation and subqueries cannot be directly updated--
