### Biggest Window Between Visits

------

문제 주소: https://leetcode.com/problems/biggest-window-between-visits/



##### 문제

```
Table: UserVisits
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| visit_date  | date |
+-------------+------+
```

유저별로 방문 날짜가 저장되어 있는 테이블에서 유저별로 가장 긴 window를 구하시오. 마지막 방문 날짜의 경우 '2021-01-01'까지로 window를 계산하시오.    

​    

##### 문제 접근 방법

window는 유저별로 어느 방문 날짜부터 다음 방문까지의 기간을 의미한다.    

LEAD 함수로 유저별/ 날짜순으로 다음 날짜가 며칠인지를 확인하고(다음 날짜가 없는 경우 '2021-01-01'로 디폴트를 설정해준다),    

이 임시 테이블에서 datediff 함수를 이용해 유저별로 가장 길었던 기간을 계산한다.    

​     

##### 문제풀이

```
SELECT
    user_id,
    MAX(DATEDIFF(nx_dt,visit_date)) as biggest_window
FROM (
SELECT
    user_id,
    visit_date,
    LEAD(visit_date, 1, '2021-01-01') OVER (PARTITION BY user_id ORDER BY visit_date) as nx_dt
FROM UserVisits
) AS tmp
GROUP BY user_id
ORDER BY user_id
;
```

