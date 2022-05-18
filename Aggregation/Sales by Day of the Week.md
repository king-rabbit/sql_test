### Sales by Day of the Week

------

문제 주소: https://leetcode.com/problems/sales-by-day-of-the-week/



##### 문제

```
Table: `Orders`
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| order_date    | date    | 
| item_id       | varchar |
| quantity      | int     |
+---------------+---------+

Table:  `Items`
+---------------------+---------+
| Column Name         | Type    |
+---------------------+---------+
| item_id             | varchar |
| item_name           | varchar |
| item_category       | varchar |
+---------------------+---------+
```
  아이템 카테고리별로 각 요일에 제품이 판매된 수량을 구하시오.

​    

##### 문제 접근 방법
WEEKDAY 함수로 날짜의 요일을 구하고,    
CASE WHEN 절로 요일별 quantity를 합산한다.    
판매수량이 전혀 없는 카테고리를 반영하기 위해 Items 테이블에 LEFT JOIN해야 한다.

​     

##### 문제풀이

```
SELECT
    i.item_category AS CATEGORY,
    IFNULL(SUM(CASE WHEN WEEKDAY(o.order_date)=0 then o.quantity else null end), 0) as MONDAY,
    IFNULL(SUM(CASE WHEN WEEKDAY(o.order_date)=1 then o.quantity else null end), 0) as TUESDAY,
    IFNULL(SUM(CASE WHEN WEEKDAY(o.order_date)=2 then o.quantity else null end), 0) as WEDNESDAY,
    IFNULL(SUM(CASE WHEN WEEKDAY(o.order_date)=3 then o.quantity else null end), 0) as THURSDAY,
    IFNULL(SUM(CASE WHEN WEEKDAY(o.order_date)=4 then o.quantity else null end), 0) as FRIDAY,
    IFNULL(SUM(CASE WHEN WEEKDAY(o.order_date)=5 then o.quantity else null end), 0) as SATURDAY,
    IFNULL(SUM(CASE WHEN WEEKDAY(o.order_date)=6 then o.quantity else null end), 0) as SUNDAY
FROM Items i  
LEFT JOIN Orders o
ON o.item_id = i.item_id
GROUP BY 1
ORDER BY 1    
;
```
