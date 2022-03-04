### Game Play Analysis V

------

문제 주소: https://leetcode.com/problems/game-play-analysis-v/



##### 문제

```
Table: Activity
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+
```

player가 처음 로그인한 날을 install_date라고 할 때, 날짜별로 처음 접속한 유저의 수와 다음날 또 접속한 비율인 Day 1 retention을 구하시오. retention은 소수점 2째 자리까지 구하시오.       

​     

##### 문제 접근 방법

1. 플레이어별로 접속일자의 최소값을 구해 최초 접속일을 구한다.    
2. 최초 접속일보다 하루 더한 날에 기록이 있는 경우를 추출하고, 여기서 date를 하루 빼서 최초 접속일을 기준으로 다음날 재접속자의 수를 구한다.    
3. 두 결과를 날짜 기준으로 join하고 재접속자의 수를 전체 접속자의 수로 나눠서 retention을 구한다.    

​     

##### 문제풀이

```
WITH cte AS (
SELECT
    player_id,
    MIN(event_date) AS install_dt
FROM Activity
GROUP BY 1
), cte2 as (
SELECT
    DATE_SUB(a.event_date, INTERVAL 1 DAY) as install_dt, count(a.player_id) as cnt
FROM Activity a
WHERE (a.player_id, a.event_date) IN (SELECT player_id, date_add(install_dt, interval 1 day) from cte )
group by 1
)
SELECT
    tmp.install_dt, tmp.installs, round(IFNULL(cte2.cnt, 0) / tmp.installs, 2) AS Day1_retention 
FROM (
SELECT install_dt, count(*) as installs FROM cte GROUP BY 1
    ) tmp
    LEFT JOIN cte2
        ON cte2.install_dt = tmp.install_dt
;
```

