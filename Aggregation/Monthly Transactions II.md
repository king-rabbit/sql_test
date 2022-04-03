### Monthly Transactions II

------

문제 주소: https://leetcode.com/problems/monthly-transactions-ii/



##### 문제

```
Table: Transactions
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| id             | int     |
| country        | varchar |
| state          | enum    |
| amount         | int     |
| trans_date     | date    |
+----------------+---------+

Table: Chargebacks
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| trans_id       | int     |
| trans_date     | date    |
+----------------+---------+
```

매달 국가별로 approve된 거래 건수과 거래액수, chargeback된 거래 건수와 거래 액수를 구하시오.    

4개의 컬럼이 모두 0인 경우는 제외하시오.    

​    

##### 문제 접근 방법

거래 날짜와 지불거절된 날짜를 함께 처리하기 위해 각 테이블에서 날짜를 추출하고 stage라는 컬럼으로 날짜의 유형을 표시해 임시 테이블을 생성한다.    

이후 거래된 경우/ 거래 승인된 경우 또는 지불 거절된 경우마다 거래 건수와 액수를 case when 절을 이용해 구한다.    

4개 값이 모두 0인 경우는 having 절을 이용해 제외시킨다.    

​     

##### 문제풀이

```
with date_table as (
select 
t.id, t.trans_date as dt, 't' as stage
from Transactions t
union all
select
c.trans_id as id, c.trans_date as dt, 'c' as stage
from Chargebacks c
)
select 
date_format(dt, '%Y-%m') as month, country, 
ifnull(count(distinct case when stage='t' and state='approved' then id else null end),0) as approved_count,
ifnull(sum(case when stage='t' and state='approved' then amount else null end),0) as approved_amount,
ifnull(count(distinct case when stage='c' then id else null end),0) as chargeback_count,
ifnull(sum(case when stage='c' then amount else null end),0) as chargeback_amount
from (
select
date_table.*, t.country, t.state, t.amount
from date_table join Transactions t
on date_table.id = t.id
) tmp
group by 1, 2
having approved_count<>0 or approved_amount<>0 or chargeback_count<>0 or chargeback_amount<>0
;
```

​    
