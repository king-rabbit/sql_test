### Find Cumulative Salary of an Employee

------

문제 주소: https://leetcode.com/problems/find-cumulative-salary-of-an-employee/



##### 문제

```
Table: Employee
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| month       | int  |
| salary      | int  |
+-------------+------+
```

직원별로 매월 최근 3개월간의 급여 합계를 구하시오. 단, 직원별 가장 최근 일한 달은 제외하며, 일하지 않은 달의 급여는 0으로 처리해 구하시오.    

​    

##### 문제 접근 방법

직원별로 가장 최근 일한 달은 where 절을 통해 제외해준다.    

id와 month 순으로 정렬한 뒤 이전 1,2행의 개월 수가 현재 행의 개월 수보다 1달 또는 2달 적은 달과 일치하는 경우, 즉 이전 한 두달에 일한 경우에는 이전 행의 급여를 가지고 온다. 여기서 이전 행의 급여 값과 개월 수는 lag 행을 이용해 가져올 수 있다. 그렇지 않은 경우에는 if 문을 사용해 급여를 0으로 처리한다.        

이렇게 구한 세 급여 값을 더해 최종 값을 구해주면 된다.    

​     

##### 문제풀이

```
SELECT
*
FROM (
SELECT
    id, 
    month,
    (salary
    + IF(lag(month, 1) over (partition by id order by month) = month-1, lag(salary, 1) over (partition by id order by month) ,0) 
    + IF(lag(month, 2) over (partition by id order by month) = month-2, lag(salary, 2) over (partition by id order by month) ,0) 
     ) as salary
FROM Employee
WHERE (id, month) NOT IN (SELECT id, max(month) FROM Employee GROUP BY id)
ORDER BY id, month
) AS tmp
ORDER BY id, month desc
;
```

