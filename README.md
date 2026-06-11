# SQL-CTE-Practice-Queries-Sales-Customer-Analytics

## 📌 Overview
This repository contains **15 SQL Common Table Expression (CTE) practice queries** built on a sample business schema with **Customers, Products, Salespersons, and Orders** tables.  
Each query demonstrates how to use CTEs for **analytics, reporting, and interview preparation**.  
Screenshots of query execution results are included for clarity.

---

## 🗂 Schema
- **Customers**: Customer details (ID, Name, Gender, City, Email)  
- **Products**: Product catalog (ID, Name, Category, Price)  
- **Salespersons**: Sales team details (ID, Name, Region, TargetAmount)  
- **Orders**: Transactions (OrderID, CustomerID, SalespersonID, ProductID, Quantity, OrderDate)  

---

## 🔎 Queries & Screenshots

### 1. Total Order Quantity > 3 per Customer
```sql
WITH CustomerQuantity AS (
    SELECT CustomerID, SUM(Quantity) AS TotalQuantity
    FROM Orders
    GROUP BY CustomerID
)
SELECT * FROM CustomerQuantity WHERE TotalQuantity > 3;
```

- 
---

### 2. Total Sales Amount per Order
```sql
WITH OrderSales AS (
    SELECT OrderID, (p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
)
SELECT * FROM OrderSales;
```
📷 Screenshot: `screenshots/query2.png`

---

### 3. Total Sales Amount per Salesperson
```sql
WITH SalespersonSales AS (
    SELECT s.SalespersonID, s.SalespersonName,
           SUM(p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    JOIN Salespersons s ON o.SalespersonID = s.SalespersonID
    GROUP BY s.SalespersonID, s.SalespersonName
)
SELECT * FROM SalespersonSales;
```
📷 Screenshot: `screenshots/query3.png`

---

### 4. Average Product Price by Category
```sql
WITH CategoryAvg AS (
    SELECT Category, AVG(Price) AS AvgPrice
    FROM Products
    GROUP BY Category
)
SELECT * FROM CategoryAvg;
```
📷 Screenshot: `screenshots/query4.png`

---

### 5. Customers and Their Total Number of Orders
```sql
WITH CustomerOrders AS (
    SELECT CustomerID, COUNT(OrderID) AS OrderCount
    FROM Orders
    GROUP BY CustomerID
)
SELECT * FROM CustomerOrders;
```
📷 Screenshot: `screenshots/query5.png`

---

### 6. Top 3 Most Sold Products
```sql
WITH ProductSales AS (
    SELECT ProductID, SUM(Quantity) AS TotalSold
    FROM Orders
    GROUP BY ProductID
)
SELECT * FROM ProductSales
ORDER BY TotalSold DESC
LIMIT 3;
```
📷 Screenshot: `screenshots/query6.png`

---

### 7. Total Revenue per Product
```sql
WITH ProductRevenue AS (
    SELECT ProductID, SUM(p.Price * o.Quantity) AS Revenue
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    GROUP BY ProductID
)
SELECT * FROM ProductRevenue;
```
📷 Screenshot: `screenshots/query7.png`

---

### 8. Salesperson Performance (Sales vs Target)
```sql
WITH SalesPerformance AS (
    SELECT s.SalespersonID, s.SalespersonName,
           SUM(p.Price * o.Quantity) AS TotalSales,
           s.TargetAmount
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    JOIN Salespersons s ON o.SalespersonID = s.SalespersonID
    GROUP BY s.SalespersonID, s.SalespersonName, s.TargetAmount
)
SELECT *, (TotalSales - TargetAmount) AS PerformanceGap
FROM SalesPerformance;
```
📷 Screenshot: `screenshots/query8.png`

---

### 9. Cities with More Than 2 Customers
```sql
WITH CityCustomers AS (
    SELECT City, COUNT(CustomerID) AS CustomerCount
    FROM Customers
    GROUP BY City
)
SELECT * FROM CityCustomers WHERE CustomerCount > 2;
```
📷 Screenshot: `sc

---

### 10. Total Sales Per Day
```sql
WITH DailySales AS (
    SELECT OrderDate, SUM(p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    GROUP BY OrderDate
)
SELECT * FROM DailySales;
```
Here’s a polished **README.md** draft for your GitHub repository. It includes each of the 15 SQL CTE practice questions, space for the query, and placeholders for screenshots so you can attach them later. This way, your repo looks professional and interview-ready.

---

te)  

---

## 🔎 Queries & Screenshots

### 1. Total Order Quantity > 3 per Customer
```sql
WITH CustomerQuantity AS (
    SELECT CustomerID, SUM(Quantity) AS TotalQuantity
    FROM Orders
    GROUP BY CustomerID
)
SELECT * FROM CustomerQuantity WHERE TotalQuantity > 3;
```
📷 Screenshot: `screenshots/query1.png`

---

### 2. Total Sales Amount per Order
```sql
WITH OrderSales AS (
    SELECT OrderID, (p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
)
SELECT * FROM OrderSales;
```
📷 Screenshot: `screenshots/query2.png`

---

### 3. Total Sales Amount per Salesperson
```sql
WITH SalespersonSales AS (
    SELECT s.SalespersonID, s.SalespersonName,
           SUM(p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    JOIN Salespersons s ON o.SalespersonID = s.SalespersonID
    GROUP BY s.SalespersonID, s.SalespersonName
)
SELECT * FROM SalespersonSales;
```
📷 Screenshot: `screenshots/query3.png`

---

### 4. Average Product Price by Category
```sql
WITH CategoryAvg AS (
    SELECT Category, AVG(Price) AS AvgPrice
    FROM Products
    GROUP BY Category
)
SELECT * FROM CategoryAvg;
```
📷 Screenshot: `screenshots/query4.png`

---

### 5. Customers and Their Total Number of Orders
```sql
WITH CustomerOrders AS (
    SELECT CustomerID, COUNT(OrderID) AS OrderCount
    FROM Orders
    GROUP BY CustomerID
)
SELECT * FROM CustomerOrders;
```
📷 Screenshot: `screenshots/query5.png`

---

### 6. Top 3 Most Sold Products
```sql
WITH ProductSales AS (
    SELECT ProductID, SUM(Quantity) AS TotalSold
    FROM Orders
    GROUP BY ProductID
)
SELECT * FROM ProductSales
ORDER BY TotalSold DESC
LIMIT 3;
```
📷 Screenshot: `screenshots/query6.png`

---

### 7. Total Revenue per Product
```sql
WITH ProductRevenue AS (
    SELECT ProductID, SUM(p.Price * o.Quantity) AS Revenue
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    GROUP BY ProductID
)
SELECT * FROM ProductRevenue;
```
📷 Screenshot: `screenshots/query7.png`

---

### 8. Salesperson Performance (Sales vs Target)
```sql
WITH SalesPerformance AS (
    SELECT s.SalespersonID, s.SalespersonName,
           SUM(p.Price * o.Quantity) AS TotalSales,
           s.TargetAmount
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    JOIN Salespersons s ON o.SalespersonID = s.SalespersonID
    GROUP BY s.SalespersonID, s.SalespersonName, s.TargetAmount
)
SELECT *, (TotalSales - TargetAmount) AS PerformanceGap
FROM SalesPerformance;
```
📷 Screenshot: `screenshots/query8.png`

---

### 9. Cities with More Than 2 Customers
```sql
WITH CityCustomers AS (
    SELECT City, COUNT(CustomerID) AS CustomerCount
    FROM Customers
    GROUP BY City
)
SELECT * FROM CityCustomers WHERE CustomerCount > 2;
```
📷 Screenshot: `screenshots/query9.png`

---

### 10. Total Sales Per Day
```sql
WITH DailySales AS (
    SELECT OrderDate, SUM(p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    GROUP BY OrderDate
)
SELECT * FROM DailySales;
```
📷 Screenshot: `screenshots/query10.png`

---

### 11. Customers Whose Purchase > Average Purchase
```sql
WITH CustomerPurchase AS (
    SELECT CustomerID, SUM(p.Price * o.Quantity) AS TotalPurchase
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    GROUP BY CustomerID
),
AvgPurchase AS (
    SELECT AVG(TotalPurchase) AS AvgPurchaseAmount
    FROM CustomerPurchase
)
SELECT * FROM CustomerPurchase cp, AvgPurchase ap
WHERE cp.TotalPurchase > ap.AvgPurchaseAmount;
```
📷 Screenshot: `screenshots/query11.png`

---

### 12. Category-Wise Total Sales Amount
```sql
WITH CategorySales AS (
    SELECT p.Category, SUM(p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    GROUP BY p.Category
)
SELECT * FROM CategorySales;
```
📷 Screenshot: `screenshots/query12.png`

---

### 13. Products Whose Price > Category Average
```sql
WITH CategoryAvgPrice AS (
    SELECT Category, AVG(Price) AS AvgPrice
    FROM Products
    GROUP BY Category
)
SELECT p.ProductID, p.ProductName, p.Category, p.Price, c.AvgPrice
FROM Products p
JOIN CategoryAvgPrice c ON p.Category = c.Category
WHERE p.Price > c.AvgPrice;
```
📷 Screenshot: `screenshots/query13.png`

---

### 14. Salesperson Ranking by Total Sales
```sql
WITH SalespersonSales AS (
    SELECT s.SalespersonID, s.SalespersonName,
           SUM(p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    JOIN Salespersons s ON o.SalespersonID = s.SalespersonID
    GROUP BY s.SalespersonID, s.SalespersonName
),
SalesRank AS (
    SELECT SalespersonID, SalespersonName, TotalSales,
           RANK() OVER (ORDER BY TotalSales DESC) AS RankPosition
    FROM SalespersonSales
)
SELECT * FROM SalesRank;
```
📷 Screenshot: `screenshots/query14.png`

---

### 15. Customers Who Purchased More Than One Product
```sql
WITH CustomerProducts AS (
    SELECT CustomerID, COUNT(DISTINCT ProductID) AS ProductCount
    FROM Orders
    GROUP BY CustomerID
)
SELECT * FROM CustomerProducts WHERE ProductCount > 1;
```
📷 Screenshot: `screenshots/query15.png`

---

## 📷 Screenshots
All query outputs are stored in the `screenshots/` folder with filenames `query1.png`, `query2.png`, … `query15.png`.

---

## 🎯 Purpose
- Practice SQL **CTEs** for analytics  
- Prepare for **interviews** with real-world scenarios  
- Showcase SQL skills in a **portfolio project**  

---
```

---

This README is **ready to paste into GitHub**. You just need to add your screenshots into a `screenshots/` folder and name them `query1.png` … `query15.png`.  

Would you like me to also create a **short executive summary section** at the top (like “What you’ll learn from this repo”) so it looks even more recruiter-friendly?
---

### 11. Customers Whose Purchase > Average Purchase
```sql
WITH CustomerPurchase AS (
    SELECT CustomerID, SUM(p.Price * o.Quantity) AS TotalPurchase
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    GROUP BY CustomerID
),
AvgPurchase AS (
    SELECT AVG(TotalPurchase) AS AvgPurchaseAmount
    FROM CustomerPurchase
)
SELECT * FROM CustomerPurchase cp, AvgPurchase ap
WHERE cp.TotalPurchase > ap.AvgPurchaseAmount;
```
📷 Screenshot: `screenshots/query11.png`

---

### 12. Category-Wise Total Sales Amount
```sql
WITH CategorySales AS (
    SELECT p.Category, SUM(p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    GROUP BY p.Category
)
SELECT * FROM CategorySales;
```
📷 Screenshot: `screenshots/query12.png`

---

### 13. Products Whose Price > Category Average
```sql
WITH CategoryAvgPrice AS (
    SELECT Category, AVG(Price) AS AvgPrice
    FROM Products
    GROUP BY Category
)
SELECT p.ProductID, p.ProductName, p.Category, p.Price, c.AvgPrice
FROM Products p
JOIN CategoryAvgPrice c ON p.Category = c.Category
WHERE p.Price > c.AvgPrice;
```
📷 Screenshot: `screenshots/query13.png`

---

### 14. Salesperson Ranking by Total Sales
```sql
WITH SalespersonSales AS (
    SELECT s.SalespersonID, s.SalespersonName,
           SUM(p.Price * o.Quantity) AS TotalSales
    FROM Orders o
    JOIN Products p ON o.ProductID = p.ProductID
    JOIN Salespersons s ON o.SalespersonID = s.SalespersonID
    GROUP BY s.SalespersonID, s.SalespersonName
),
SalesRank AS (
    SELECT SalespersonID, SalespersonName, TotalSales,
           RANK() OVER (ORDER BY TotalSales DESC) AS RankPosition
    FROM SalespersonSales
)
SELECT * FROM SalesRank;
```
📷 Screenshot: `screenshots/query14.png`

---

### 15. Customers Who Purchased More Than One Product
```sql
WITH CustomerProducts AS (
    SELECT CustomerID, COUNT(DISTINCT ProductID) AS ProductCount
    FROM Orders
    GROUP BY CustomerID
)
SELECT * FROM CustomerProducts WHERE ProductCount > 1;
```
📷 Screenshot: `screenshots/query15.png`

---

## 📷 Screenshots
All query outputs are stored in the `screenshots/` folder with filenames `query1.png`, `query2.png`, … `query15.png`.

---

## 🎯 Purpose
- Practice SQL **CTEs** for analytics  
- Prepare for **interviews** with real-world scenarios  
- Showcase SQL skills in a **portfolio project**  

---
```

---

This README is **ready to paste into GitHub**. You just need to add your screenshots into a `screenshots/` folder and name them `query1.png` … `query15.png`.  

Would you like me to also create a **short executive summary section** at the top (like “What you’ll learn from this repo”) so it looks even more recruiter-friendly?
