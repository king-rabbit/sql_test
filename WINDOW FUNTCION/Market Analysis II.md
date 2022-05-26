### Market Analysis II

------

문제 주소: https://leetcode.com/problems/market-analysis-ii/



##### 문제

```
Table: Users
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| join_date      | date    |
| favorite_brand | varchar |
+----------------+---------+

Table: Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| item_id       | int     |
| buyer_id      | int     |
| seller_id     | int     |
+---------------+---------+

Table: Items

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| item_id       | int     |
| item_brand    | varchar |
+---------------+---------+

```

각 셀러에 대해 날짜순으로 2번째로 판매한 제품의 브랜드가 자신이 가장 좋아하는 브랜드이면 yes를, 아니면 no를 출력하시오.    

​    

##### 문제 접근 방법

판매기록에 대해서 셀러 아이디별로 / 날짜순으로 rank를 매긴 뒤,    
세 테이블을 조인했을 때 favorite_brand와 item_brand가 일치하고 rank가 2인 경우만 추출한다.    
마지막으로 case when 절을 이용해 여기에 해당하는 user는 yes를, 아니면 no를 출력한다. 

​     

##### 문제풀이

```
with cte as (
select o.seller_id, 
    u.favorite_brand,
    i.item_brand,
    rank() over (partition by o.seller_id order by o.order_date) as order_rank
from Orders o
    left join Items i
    on o.item_id = i.item_id
    left join Users u
    on o.seller_id = u.user_id
), cte2 as (
    select seller_id
    from cte
    where order_rank=2 and favorite_brand=item_brand
)
select user_id as seller_id, 
    case when user_id in (select * from cte2) then 'yes'
            else 'no' end as 2nd_item_fav_brand
from Users
;

```

