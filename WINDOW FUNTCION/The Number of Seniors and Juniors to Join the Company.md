### The Number of Seniors and Juniors to Join the Company

------

문제 주소: https://leetcode.com/problems/the-number-of-seniors-and-juniors-to-join-the-company/



##### 문제

```
Table: Candidates
+-------------+------+
| Column Name | Type |
+-------------+------+
| employee_id | int  |
| experience  | enum |
| salary      | int  |
+-------------+------+
```

Candidate 테이블은 채용 후보자 목록으로, experience는 Senior와 Junior로 나누어져 있음.    

70000$ 예산 안에서 가능한 많은 Senior와 Junior를 채용하고자 함.    

Senior를 우선적으로 가능한 가장 많은 인원을 채용하고,    

Senior 채용 후 남은 예산으로 가능한 많은 Junior를 채용한다고 했을 때,    

experience별로 채용 가능한 인원을 구하시오.    

​    

##### 문제 접근 방법

경력별로 연봉이 적은 순서대로 누적 합계를 구하고, 그 순서대로 경력별 순서값(row_num)을 매김. 이는 이후 인원 수를 구하기 위함임.    

임시테이블에서 먼저 가능한 예산 안에서 채용 가능한 Senior의 최대 인원을 max(row_num)으로 구하되, 1명도 해당되지 않을 경우를 대비해 ifnull 함수를 적용함.    

senior에 배정된 최대 예산을 70000$에서 제외하고 나머지 금액으로, Junior 최대 인원을 같은 방식으로 구한 뒤 두 테이블을 union함.     

​     

##### 문제풀이

```
WITH CTE as (
    SELECT
        ROW_NUMBER() OVER (PARTITION BY experience) as row_num,
        experience,
        SUM(salary) OVER (PARTITION BY experience ORDER BY salary) AS 'CUM_SAL'
    FROM Candidates   
)
SELECT 'Senior' AS experience, 
    IFNULL(MAX(row_num),0) as accepted_candidates 
FROM cte 
WHERE experience = 'Senior' 
    AND cum_sal <= 70000
UNION 
SELECT 'Junior' AS experience,  
    MAX(row_num) AS accepted_candidates 
FROM cte 
WHERE experience = 'Junior' 
    AND cum_sal <= ( 70000 - (SELECT IFNULL(MAX(CUM_SAL), 0)
                          FROM cte
                          WHERE experience = 'Senior'
                            AND cum_sal <= 70000)
                   )
;
```

