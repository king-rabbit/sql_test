### Project Employees III

------

문제 주소: https://leetcode.com/problems/project-employees-iii/



##### 문제

```
Table: Project
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+

Table: Employee
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+
```

각 프로젝트별로 경력이 가장 많은 직원의 아이디를 출력하시오. 동점자가 있으면 모두 출력하시오.     

​    

##### 문제 접근 방법

rank() 함수를 이용해 프로젝트별 / 경력 순으로 순위를 구한 뒤 순위가 1위인 경우만 출력한다.    

​     

##### 문제풀이

```
select
    project_id,
    employee_id
from (
    SELECT
        p.project_id,
        p.employee_id,
        rank() over (partition by p.project_id order by e.experience_years desc) as ex_rank
    FROM Project p
        left join Employee e
        on p.employee_id = e.employee_id
    ) tmp
where ex_rank=1
;
```

