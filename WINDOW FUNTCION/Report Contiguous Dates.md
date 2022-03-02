### Report Contiguous Dates

------

문제 주소: https://leetcode.com/problems/report-contiguous-dates/



##### 문제

```
Table: Failed
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| fail_date    | date    |
+--------------+---------+

Table: Succeeded
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| success_date | date    |
+--------------+---------+
```

서비스 기능이 성공한 날과 실패한 날이 각 테이블에 저장되어 있을 때, 연속으로 성공 또는 실패한 기간을 출력하시오. 성공 또는 실패 여부는 period_state, 기간별 시작 날짜와 종료 날짜는 start_date, end_date 컬럼으로 출력하시오.    

2019년 기간만을 대상하고, start_date 순으로 출력하시오.    

​    

##### 문제 접근 방법

두 테이블을 union한 테이블을 만든 뒤,    

연속한 기간을 묶어주기 위해 날짜별 랭킹에서 상태별/날짜별 랭킹을 뺀 값(gr)을 구한다.    

상태와 그룹(gr)별로 group by해서 날짜의 최소값은 start_date로, 최대값은 end_date로 구한다.    

​     

##### 문제풀이

```
with cte as (
    SELECT fail_date as date, 'failed' as state
    FROM Failed
    WHERE fail_date BETWEEN '2019-01-01' and '2019-12-31'
    UNION
    SELECT success_date as date, 'succeeded' as state
    FROM Succeeded
    WHERE success_date BETWEEN '2019-01-01' and '2019-12-31'
)
SELECT 
    state as period_state,
    MIN(date) as start_date,
    MAX(date) AS end_date
FROM (
SELECT
    (rank() over (order by date)) - RANK() OVER (PARTITION BY state ORDER BY date) as gr,
    date,
    state
FROM cte
) AS tmp
GROUP BY state, gr
ORDER BY MIN(date)
;
```

