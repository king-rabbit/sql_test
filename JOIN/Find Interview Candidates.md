### Find Interview Candidates

------

문제 주소: https://leetcode.com/problems/find-interview-candidates/



##### 문제

```
Table: Contests
+--------------+------+
| Column Name  | Type |
+--------------+------+
| contest_id   | int  |
| gold_medal   | int  |
| silver_medal | int  |
| bronze_medal | int  |
+--------------+------+

Table: Users
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| mail        | varchar |
| name        | varchar |
+-------------+---------+
```

클라이언트와 택시 드라이버가 모두 'banned' 상태가 아닌 경우에 한해 '2013-10-01'부터 '2013-10-03'까지 택시 운행이 취소된 비율을 계산할 것. 결과는 소수점 두자리까지 반올림해서 표시할 것.     

Trips의 id는 택시 운행별 id값이며, client_id와 driver_id 컬럼은 모두 Users 테이블의 users_id 컬럼을 참조하고 있음.    

​     

##### 문제 접근 방법

Trips의 두 컬럼이 Users 테이블의 한 컬럼을 참조하고 있어 각 컬럼에 대해 1번씩 조인을 총 2번해야 한다.     

클라이언트와 드라이버가 모두 'banned'가 아닌 케이스만 계산하도록 WHERE문에서 조건을 주고, COUNT( CASE ~ WHEN ~ ) 문으로 운행 콜이 취소된 경우만 계산해 비율을 계산했다.     

​     

##### 문제풀이

```
select
u.name as name,
u.mail as mail
from contests c
    join users u
        on c.gold_medal = u.user_id
group by 1,2
having count(*) >= 3
union
select 
    distinct u.name as name,
    u.mail as mail
from (
select 
    c1.gold_medal as g1,
    c1.silver_medal as s1,
    c1.bronze_medal as b1,
    c2.gold_medal as g2,
    c2.silver_medal as s2,
    c2.bronze_medal as b2,
    c3.gold_medal as g3,
    c3.silver_medal as s3,
    c3.bronze_medal as b3
from contests c1, contests c2, contests c3
where c1.contest_id+1= c2.contest_id
    and c2.contest_id+1 = c3.contest_id
) as c
join users u
on u.user_id =c.g1 or u.user_id=c.s1 or u.user_id=c.b1
    or u.user_id =c.g2 or u.user_id=c.s2 or u.user_id=c.b2
    or u.user_id =c.g3 or u.user_id=c.s3 or u.user_id=c.b3
where u.user_id IN (c.g1, c.s1, c.b1)
    and u.user_id IN (c.g2, c.s2, c.b2)
    and u.user_id IN (c.g3, c.s3, c.b3)
;
```

