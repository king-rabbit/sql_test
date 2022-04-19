### Restaurant Growth

------

문제 주소: https://leetcode.com/problems/restaurant-growth/



##### 문제

```
Table: Customer

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| visited_on    | date    |
| amount        | int     |
+---------------+---------+
```

테이블에 레스토랑의 고객 아이디, 고객 이름, 방문 날짜, 매출이 기록되어 있을 때 해당 날짜를 포함해 7일간의 이동평균을 구하시오.

​    

##### 문제 접근 방법

SUM() OVER () 함수에서 range between interval - day preceding and current row 표현으로 6일 전 row에 대해서만 누적합을 구한다.    

그리고 지난 6일치 데이터가 없는 경우는 제외하기 위해 dense_rank() 함수로 날짜별 순위를 매기고, 이 순위가 7 이상인 값만 추출한다.    

​     

##### 문제풀이

```
with cte as (
select visited_on, 
       dense_rank() over (order by visited_on) as dr,
      sum(amount) over (order by visited_on range between interval 6 day preceding and current row) as amount
from Customer
)
select distinct visited_on, amount, round(amount/7, 2) as average_amount
from cte
where dr >= 7
;
```

