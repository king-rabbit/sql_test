### Managers with at Least 5 Direct Reports

------

문제 주소: https://leetcode.com/problems/managers-with-at-least-5-direct-reports/



##### 문제

```
Table: Employee
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
```

각 직원의 아이디와 매니저 아이디가 주어졌을 때, 부하 직원이 5명 이상인 직원의 이름을 출력하시오.      

​    

##### 문제 접근 방법

매니저 아이디와 직원 아이디가 같은 경우로 조인해서 직원 및 매니저의 연결 테이블을 만들고,    

여기서 매니저별 직원의 수가 5명 이상인 경우를 having 절로 필터링한다.    

​     

##### 문제풀이

```
select
    e2.name
from Employee e1, Employee e2
where e1.managerId = e2.id
group by e2.id
having count(distinct e1.id) >= 5
;
```

​    
