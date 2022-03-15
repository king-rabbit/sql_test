### Median Employee Salary

------

문제 주소: https://leetcode.com/problems/median-employee-salary/



##### 문제

```
Table: Employee
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| company      | varchar |
| salary       | int     |
+--------------+---------+
```

회사별로 임금의 중앙값(median)에 해당하는 직원의 id와 회사, 임금을 출력하시오.     

​    

##### 문제 접근 방법

회사별 임금이 높은 순서대로 row_number()를 매긴 뒤 (임금 값이 같은 경우 id값이 작은 값만 출력해야 하므로 rank()는 쓰지 않음),    

회사별 직원 수를 count()하는 임시 테이블을 따로 생성하고,    

직원 수가 홀수 또는 짝수냐에 따라 중간값을 다르게 출력하도록 where절에 설정한다.    

​     

##### 문제풀이

```
WITH cte as (
    SELECT
        id, company, salary,
        ROW_NUMBER() OVER (PARTITION BY company ORDER BY salary) as sal_rank
    FROM Employee
) , cte2 as (
    select company, count(*) as cnt from Employee group by 1
)
select
    id, company, salary
from cte
where (company, sal_rank) 
    IN (
        select company, floor(cnt/2) + 1 as med_num from cte2
        union all
        select company, 
            case when cnt%2=0 then floor(cnt/2) else null end as med_num 
        from cte2
    )
;
```

