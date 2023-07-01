## SQLite Studio RDBMS
Welcome to my SQL Portfolio where I showcase my data analysis skills using SQL on SQLite Studio, a powerful Relational Database Management System (RDBMS). This 
portfolio contains a series of complex queries performed on a sales dataset, encompassing the months of January and February.

The dataset includes a variety of information such as customer details, product names, quantities ordered, prices, and the purchase address.
Using this data, I've performed detailed analyses to answer critical business questions like how many orders were placed, which products were
sold, and the revenue generated. These insights are crucial to understanding customer behavior, popular products, and overall sales performance.

In this portfolio, you'll see the raw SQL queries I used to gather these insights, as well as the results from each query. 
Each query demonstrates different SQL functionalities, such as conditional statements, joins, groupings, and the use of subqueries. These 
queries range from simple data retrievals to more complex queries involving multiple stages and conditions.

I hope this portfolio provides you with a clear understanding of my ability to manipulate and analyze data using SQL, and my ability to 
derive meaningful insights from large datasets. Thank you for taking the time to review my work, and I look forward to any opportunities to 
further demonstrate my skills.

**1. How many orders were placed in January?** 9681

SELECT * FROM BIT_DB.JanSales;<br>
SELECT COUNT(*)<br>
FROM BIT_DB.JanSales<br>
WHERE length(orderID) = 6<br>
AND orderID <> 'Order ID';<br>

**2. How many of those orders were for an iPhone?** 379

SELECT COUNT(*)<br>
FROM BIT_DB.JanSales<br>
WHERE length(orderID) = 6<br>
AND orderID <> 'Order ID'<br>
AND (Product) = 'iPhone';<br>


**3. Select the customer account numbers for all the orders that were placed in February.**

SELECT customers.acctnum<br>
FROM customers<br>
JOIN FebSales ON customers.order_id = FebSales.orderID<br>

**4. Which product was the cheapest one sold in January, and what was the price?** 
600, GOOGLE PHONE

SELECT price, Product<br>
FROM JanSales<br>
WHERE price <> 'Price Each' AND<br>
length(price) > 2<br>
ORDER BY CAST (price AS REAL) ASC;<br>

**5. What is the total revenue for each product sold in January?** 


SELECT Product, <br>
       SUM(CAST(`Quantity` AS INTEGER) * CAST(Price AS REAL)) as Total_Revenue<br>
FROM JanSales<br>
WHERE `Quantity` <> 'Quantity Ordered' <br>
AND Price <> 'Price Each' <br>
AND length(`Quantity`) > 0 <br>
AND length(Price) > 2<br>
GROUP BY Product<br>

**6. Which products were sold in February at 548 Lincoln St, Seattle, WA 98101, how many of each were sold, and what was the total 
revenue?** Product: AA Batteries, Total_Sold (4-pack)2, Total_Revenue 7.68.

SELECT Product, <br>
       SUM(CAST(`Quantity` AS INTEGER)) as Total_Sold, <br>
       SUM(CAST(`Quantity` AS INTEGER) * CAST(Price AS REAL)) as Total_Revenue<br>
FROM FebSales<br>
WHERE `location` = '548 Lincoln St, Seattle, WA 98101'<br>
AND `Quantity` <> 'Quantity Ordered'<br>
AND Price <> 'Price Each'<br>
AND length(`Quantity`) > 0 <br>
AND length(Price) > 2<br>
GROUP BY Product;<br>

**7. How many customers ordered more than 2 products at a time in February, and what was the average amount spent for those customers?**
Total_Customers: 322 Average_Spent: 322


SELECT *<br>
FROM FebSales;<br>

SELECT COUNT(DISTINCT `orderID`) as Total_Customers, <br>
       AVG(Total_Spent) as Average_Spent<br>
FROM (<br>
  SELECT `orderID`, <br>
         SUM(CAST(`Quantity` AS INTEGER)) as Total_Quantity, <br>
         SUM(CAST(`Quantity` AS INTEGER) * CAST(price AS REAL)) as Total_Spent<br>
  FROM FebSales<br>
  WHERE `Quantity` <> 'Quantity Ordered' <br>
  AND price <> 'Price Each'<br>
  AND length(`Quantity`) > 0 <br>
  AND length(price) > 2<br>
  GROUP BY `orderID`<br>
  HAVING Total_Quantity > 2<br>
) subquery;<br>
