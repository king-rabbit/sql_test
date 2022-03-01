### Game Play Analysis IV

------

문제 주소: https://leetcode.com/problems/game-play-analysis-iv/



##### 문제

```
Table: Customers
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| country       | varchar |
+---------------+---------+

Table: Product
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| description   | varchar |
| price         | int     |
+---------------+---------+

Table: Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| product_id    | int     |
| order_date    | date    |
| quantity      | int     |
+---------------+---------+
```

2020년 6월과 2020년 7월에 각각 100불 이상 구매한 고객의 아이디와 이름을 출력하시오.    

​    

##### 문제 접근 방법

세 테이블을 조인한 뒤 group by절로 고객별/월별 구매금액을 추출한다. 총 구매금액이 100불 이상인 경우만 추출한 뒤 해당 뷰에서 month가 2020년 6월, 2020년 7월 모두에 해당하는 아이디를 출력한다.         

 

​     

##### 문제풀이

```
WITH cte AS (
SELECT
    c.customer_id as customer_id,
    DATE_FORMAT(o.order_date, '%Y-%m') AS month,
    c.name as name,
    sum(p.price * o.quantity) as spend
FROM Customers c 
    JOIN Orders o
        ON c.customer_id = o.customer_id
    JOIN Product p
        ON p.product_id = o.product_id
GROUP BY c.customer_id, DATE_FORMAT(o.order_date, '%Y-%m')
HAVING sum(p.price * o.quantity) >= 100
)
SELECT DISTINCT customer_id, name
FROM cte
WHERE customer_id IN (SELECT customer_id FROM cte WHERE month = '2020-06')
    AND customer_id IN (SELECT customer_id FROM cte WHERE month = '2020-07')
;
```

​    
