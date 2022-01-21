### New Companies

------

문제 주소: https://www.hackerrank.com/challenges/the-company



##### 문제

```
Table: Company
+--------------+----------+
| Column Name  | Type     |
+--------------+----------+
| company_code | String   |
| founder      | String   |
+--------------+----------+

Table: Lead_Manager
+-------------------+----------+
| Column Name       | Type     |
+-------------------+----------+
| lead_manager_code | String   |
| company_code      | String   |
+-------------------+----------+

Table: Senior_Manager
+---------------------+----------+
| Column Name         | Type     |
+---------------------+----------+
| senior_manager_code | String   |
| lead_manager_code   | String   |
| company_code        | String   |
+---------------------+----------+

Table: Manager
+---------------------+----------+
| Column Name         | Type     |
+---------------------+----------+
| manager_code        | String   |
| senior_manager_code | String   |
| lead_manager_code   | String   |
| company_code        | String   |
+---------------------+----------+

Table: Employee
+---------------------+----------+
| Column Name         | Type     |
+---------------------+----------+
| employee_code       | String   |
| manager_code        | String   |
| senior_manager_code | String   |
| lead_manager_code   | String   |
| company_code        | String   |
+---------------------+----------+
```

 Company 테이블에 회사의 코드와 Founder 데이터가 있고, 각 직급별 테이블에 직원 코드와 회사 코드가 있음. 직원 테이블의 회사 코드는 Company 테이블의 회사 코드를 참조함.    

회사별로 회사 코드와 회사의 창립자, 회사별 각 직급의 직원 수를 출력하시오. 회사 코드 오름차순으로 출력할 것.    

​    

##### 문제 접근 방법

회사별로 데이터를 집계해야 하므로 GROUP BY절을 사용하며, 데이터가 중복될 수 있으므로 COUNT DISTINCT를 사용해 직급별 직원 수를 집계함.    

​     

##### 문제풀이

```
SELECT 
    Company.company_code,
    Company.founder,
    COUNT(DISTINCT lead_manager.lead_manager_code),
    COUNT(DISTINCT Senior_Manager.senior_manager_code),
    COUNT(DISTINCT Manager.manager_code),
    COUNT(DISTINCT Employee.employee_code)
FROM Company
    LEFT JOIN Lead_Manager
        ON Company.company_code = Lead_Manager.company_code
    LEFT JOIN Senior_Manager
        ON Company.company_code = Senior_Manager.company_code
    LEFT JOIN Manager
        ON Company.company_code = Manager.company_code
    LEFT JOIN Employee
        ON Company.company_code = Employee.company_code
GROUP BY 1, 2
ORDER BY Company.company_code
;
```

