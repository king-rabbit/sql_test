### Department Top Three Salaries

------

문제 주소: https://leetcode.com/problems/department-top-three-salaries/



##### 문제

```
Table: Employee
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+

Table: Department
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
```

부서별로 임금이 높은 상위 3명을 출력할 것. 공동 순위자가 있는 경우 공동 순위를 포함해 출력(즉, 공동 2위가 있는 경우 4명을 출력함).    

Employee 테이블의 departmentId는 Department 테이블의 id 컬럼을 참조하고 있음.    



​     

##### 문제 접근 방법

공동 순위인 경우를 포함해야 하므로 DENSE_RANK() 함수를 사용해 부서별로 임금순 정렬을 하고, 여기서 순위가 3위 내인 경우만 출력했음.   

​     

##### 문제풀이

```
SELECT Department,
    Employee,
    Salary
FROM
(
SELECT d1.name AS Department,
    e1.name as Employee,
    e1.salary AS Salary,
    DENSE_RANK() OVER (PARTITION BY d1.id ORDER BY e1.salary DESC) as r
FROM Employee e1
    JOIN Department d1
        ON e1.departmentId = d1.id
) AS rank_tbl
WHERE r <= 3
;
```

