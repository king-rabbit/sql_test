###  Average Salary: Departments VS Company

------

문제 주소: https://leetcode.com/problems/average-salary-departments-vs-company/



##### 문제

```
Table: Salary
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| employee_id | int  |
| amount      | int  |
| pay_date    | date |
+-------------+------+

Table: Employee
+---------------+------+
| Column Name   | Type |
+---------------+------+
| employee_id   | int  |
| department_id | int  |
+---------------+------+
```

월별 + 부서별로 평균 임금이 해당 월의 회사 평균 임금보다 높으면 'higher', 같으면 'same', 낮으면 'lower'로 출력하시오.    



​    

##### 문제 접근 방법

date_format() 함수를 이용해 연도-월을 출력하고, 월별 및 부서별 평균 임금을 추출한다.    

다음으로 같은 포맷의 연도-월별 회사 평균 임금을 추출하는 임시 뷰를 만들고,   

두 테이블을 조인해서 답을 구한다. CASE WHEN 절을 이용해 회사 평균 임금보다 높거나 낮은 경우에 따라 다르게 출력하도록 한다.       

​     

##### 문제풀이

```
WITH cte AS (
SELECT
    DATE_FORMAT(s.pay_date, "%Y-%m") as pay_month,
    e.department_id,
    AVG(s.amount) as avg_sal
FROM Salary s
    LEFT JOIN Employee e
        ON s.employee_id = e.employee_id
GROUP BY 1, 2
), cte2 AS (
SELECT DATE_FORMAT(pay_date, "%Y-%m") as pay_month,
    AVG(amount) as avg_sal_tot
FROM Salary
GROUP BY 1
)
SELECT 
    cte.pay_month,
    cte.department_id,
    CASE WHEN cte.avg_sal > cte2.avg_sal_tot THEN 'higher' 
        WHEN cte.avg_sal = cte2.avg_sal_tot THEN 'same'
        ELSE 'lower'
    END AS 'comparison'
FROM cte
    LEFT JOIN cte2
        ON cte.pay_month = cte2.pay_month
;
```

​    
