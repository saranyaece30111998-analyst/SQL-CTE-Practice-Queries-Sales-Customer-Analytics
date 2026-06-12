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

- <img width="1237" height="507" alt="Screenshot 2026-06-12 201636" src="https://github.com/user-attachments/assets/314a9d94-14d5-4a5c-a95b-953076b836b3" />

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

- <img width="642" height="493" alt="Screenshot 2026-06-11 200805" src="https://github.com/user-attachments/assets/0bc0880c-15ea-409d-9d16-526633b6baed" />



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
- <img width="1091" height="688" alt="Screenshot 2026-06-11 200911" src="https://github.com/user-attachments/assets/5d838488-f7f1-48cb-aabd-bbbf9dfc8323" />

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
- <img width="708" height="488" alt="Screenshot 2026-06-11 201013" src="https://github.com/user-attachments/assets/e876023e-e40a-4367-8566-8759fb518dc1" />


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
- <img width="953" height="577" alt="Screenshot 2026-06-11 201113" src="https://github.com/user-attachments/assets/7f5b98d5-1612-4cd8-bc5d-d0d2f018aaab" />


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
- <img width="800" height="777" alt="Screenshot 2026-06-11 201238" src="https://github.com/user-attachments/assets/a88badd0-2402-4b9e-bd49-b59f16e5abf5" />

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
- <img width="856" height="628" alt="Screenshot 2026-06-11 201402" src="https://github.com/user-attachments/assets/6006f2bd-f118-4709-8c25-fdd2ae39146a" />


---

## 📷 Screenshots
All query outputs are stored in the `screenshots/` folder with filenames `query1.png`, `query2.png`, … `query15.png`.

---

## 🎯 Purpose
- Practice SQL **CTEs** for analytics  
- Prepare for **interviews** with real-world scenarios  
- Showcase SQL skills in a **portfolio project**  

---
