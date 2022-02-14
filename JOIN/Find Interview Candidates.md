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

콘테스트 테이블에는 콘테스트별 아이디와 콘테스트에서 금메달/은메달/동메달을 딴 유저의 아이디가 저장되어 있다.    

유저 테이블에는 각 유저의 아이디와 메일, 이름이 저장돼 있다.    

콘테스트에서 3번 이상 금메달을 땄거나(연속이 아니어도 됨), 3연속으로 입상한(메달 무관) 유저를 순서 무관하게 출력하시오.    

​     

##### 문제 접근 방법

금메달 3번 이상인 유저를 출력한 결과와 3연속 입상한 유저를 출력한 결과를 UNION으로 결합했다.    

3연속 입상한 유저를 출력할 때는 3연속인 콘테스트 결과를 출력한 뒤, 유저 아이디가 각 콘테스트의 (금, 은, 동메달) 리스트에 포함된 경우만 출력하도록 했다.    

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

