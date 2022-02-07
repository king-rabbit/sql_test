### Total Sales Amount by Year

------

문제 주소: https://leetcode.com/problems/total-sales-amount-by-year/



##### 문제

```
Table: Product
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| product_name  | varchar |
+---------------+---------+

Table: Sales
+---------------------+---------+
| Column Name         | Type    |
+---------------------+---------+
| product_id          | int     |
| period_start        | date    |
| period_end          | date    |
| average_daily_sales | int     |
+---------------------+---------+
```

제품별로 Sales 테이블에 제품 판매가 시작된 날짜와 끝날 날짜, 매일 평균 판매량이 저장되어 있다.    

제품별로 id, 제품명, 판매년도, 연도별 매출을 출력하시오.    

판매년도는 2018년에서 2020년 사이이며, 제품 id 및 연도 순으로 정렬하시오.    

​     

##### 문제 접근 방법

Recursive CTE를 이용해서 전체 날짜를 출력하는 임시 테이블을 구성한다.    

이 임시 테이블을 Sales 테이블에 조인해 제품별, 날짜별 행을 출력하고,    

제품별, 연도별로 group by를 해 매출을 계산한다.     

​     

##### 문제풀이

```
WITH RECURSIVE CTE AS 
(SELECT MIN(period_start) as date FROM Sales
  UNION ALL
 SELECT DATE_ADD(date, INTERVAL 1 DAY)
 FROM CTE
 WHERE date <= ALL (SELECT MAX(period_end) FROM Sales)
)
SELECT
    s.product_id,
    p.product_name,
    CONCAT(YEAR(c.date)) AS report_year,
    SUM(s.average_daily_sales) as total_amount
FROM Sales s
JOIN CTE c ON S.period_start <= c.date AND c.date <= s.period_end
JOIN Product p ON s.product_id = p.product_id
GROUP BY 1, 2, 3
ORDER BY 1, 2
;
```

