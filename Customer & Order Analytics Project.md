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

**7.Which locations in New York received at least 3 orders in January, and how many orders did they each receive?**
| Location                              | Num_orders |
| ------------------------------------- | ---------------- |
| 148 Elm St, New York City, NY 10001   | 3                |
| 515 Lincoln St, New York City, NY 10001| 3                |
| 916 Pine St, New York City, NY 10001  | 3                |
| 961 Jefferson St, New York City, NY 10001| 4              |

SELECT location, COUNT(orderID) as num_orders<br>
FROM BIT_DB.JanSales<br>
WHERE location LIKE '%New York City, NY%'<br>
GROUP BY location<br>
HAVING COUNT(orderID) >= 3<br>

**8.How many of each type of headphone were sold in February?**

SELECT Product, SUM(Quantity) as total_sold<br>
FROM BIT_DB.FebSales<br>
WHERE Product LIKE '%headphones%'<br>
GROUP BY Product<br>

| Product               | Quantity Sold |
| ---------------------------- | ------------- |
| Apple Airpods Headphones     | 1013          |
| Bose SoundSport Headphones   | 844           |
| Wired Headphones             | 1282          |

**9.What was the average amount spent per account in February?**

Avegage per account: 190.00034676304287

SELECT avg(quantity*price)<br>
FROM BIT_DB.FebSales Feb<br>

LEFT JOIN BIT_DB.customers cust<br>
ON FEB.orderid=cust.order_id<br>

WHERE length(orderid) = 6 <br>
AND orderid <> 'Order ID'<br>

**10. What was the average quantity of products purchased per account in February?**
Average: 1
SELECT sum(quantity)/count(cust.acctnum) <br>
FROM BIT_DB.FebSales Feb <br>
LEFT JOIN BIT_DB.customers cust <br>
ON FEB.orderid=cust.order_id <br>

WHERE length(orderid) = 6  <br>
AND orderid <> 'Order ID' <br>

**11.Which product brought in the most revenue in January and how much revenue did it bring in total?**
Macbook Pro Laptop brought total revenue of 399500
SELECT Product, SUM(Quantity * price) as Total_Revenue<br>
FROM BIT_DB.JanSales<br>
GROUP BY Product<br>
ORDER BY Total_Revenue DESC<br>
LIMIT 1<br>

**12. What is the average popularity of each artist based on the data in the 'SpotifyData' table? + Identify any artist who has an average popularity score of 90 or above.**

WITH popularity_average_CTE AS (<br>v
SELECT s.artist_name,<br>
AVG(s.popularity) AS average_popularity<br>
FROM SpotifyData s <br>
GROUP BY s.artist_name<br>
)<br>
 
SELECT  artist_name,<br>
        average_popularity,<br>
        'Top Star' AS tag<br>
FROM popularity_average_CTE<br>
WHERE average_popularity>=90;<br>
