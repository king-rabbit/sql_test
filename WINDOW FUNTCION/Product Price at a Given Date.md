### Product Price at a Given Date

------

문제 주소: https://leetcode.com/problems/product-price-at-a-given-date/



##### 문제

```
Table: Products
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |
+---------------+---------+
```

제품별로 change_date에 new_price로 가격이 변경된 정보가 있을 때,    

2019년 8월 16일 시점에서 제품별 가격을 구하시오.    

제품 가격 변경 정보가 없는 경우에는 기본적으로 가격이 10이라고 가정합니다.    

​    

##### 문제 접근 방법

product_id별 / change_date 순서대로 row number를 구한다. 기준 날짜 이전으로 조건으로 해서 임시 테이블을 구성하고, 이 임시테이블에서 row number가 1인 경우(날짜가 가장 최신인 경우)를 구한다.    

기준 날짜 이전에 가격 변동이 없는 제품이 있기 때문에 임시 테이블에 id가 없는 경우를 조건으로 설정해 products 테이블에서 아이디와 기본 가격(10)을 추출하고 이를 union해서 답을 구한다.    

​     

##### 문제풀이

```
with cte as (
select
product_id,
new_price,
change_date,
row_number() over (partition by product_id order by change_date desc) as rn
from Products
where change_date <= '2019-08-16'
)
select product_id, new_price as price
from cte
where rn=1
union all
select distinct product_id, 10 as price
from Products
where product_id NOT IN (select product_id from cte)
;
```

