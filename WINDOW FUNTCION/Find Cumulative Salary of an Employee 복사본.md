### User Purchase Platform

------

문제 주소: https://leetcode.com/problems/user-purchase-platform/



##### 문제

```
Table: Spending
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| spend_date  | date    |
| platform    | enum    | 
| amount      | int     |
+-------------+---------+
```

결제일별로 mobile only, desktop only, 모바일&데스크탑 모두(both) 결제한 유저의 수와 결제수단별 결재액의 합과 유저의 수를 구하시오.       

결제수단별 유저의 수가 0명인 경우는 유저의 수, 결제액 모두 0원으로 표기해 출력하시오.    

​    

##### 문제 접근 방법

결제한 케이스가 없는 날짜의 경우 0원/0명으로 출력하기 위해 모바인 온리/데스크탑 온리/둘다의 케이스를 나눠서 임시 뷰를 생성했다.    

그리고 기존 Spending 테이블에서 distinct하게 날짜를 추출한 뒤 각 임시 뷰를 여기에 left join해 결제 케이스가 없는 경우 0원/0명으로 집계되게 하고,    

이렇게 구한 결과들을 union all해서 최종 결과를 냈다.    

​     

##### 문제풀이

```
with cte as (
select
 spend_date, user_id, count(platform), sum(amount) as amount
from Spending
group by spend_date, user_id
having count(platform) = 2
), cte_mobile as (
select
 spend_date, user_id, platform, count(platform), sum(amount) as amount
from Spending
group by spend_date, user_id
having count(platform) = 1 and platform='mobile'
), cte_desktop as (
select
 spend_date, user_id, platform, count(platform), sum(amount) as amount
from Spending
group by spend_date, user_id
having count(platform) = 1 and platform='desktop'
)

select distinct Spending.spend_date, 
        "both" as 'platform', 
        ifnull(tmp.am, 0) as total_amount, 
        ifnull(tmp.cnt, 0) as total_users
from Spending
left join (
select spend_date, count(user_id) as cnt, sum(amount) as am from cte group by 1
) tmp
on Spending.spend_date = tmp.spend_date
union all
select distinct Spending.spend_date, 
        "mobile" as 'platform', 
        ifnull(tmp.am, 0) as total_amount, 
        ifnull(tmp.cnt, 0) as total_users
from Spending
left join (
select spend_date, count(user_id) as cnt, sum(amount) as am from cte_mobile group by 1
) tmp
on Spending.spend_date = tmp.spend_date
union all
select distinct Spending.spend_date, 
        "desktop" as 'platform', 
        ifnull(tmp.am, 0) as total_amount, 
        ifnull(tmp.cnt, 0) as total_users
from Spending
left join (
select spend_date, count(user_id) as cnt, sum(amount) as am from cte_desktop group by 1
) tmp
on Spending.spend_date = tmp.spend_date
;
```

