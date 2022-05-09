### Longest Winning Streak

------

문제 주소: https://leetcode.com/problems/longest-winning-streak/



##### 문제

```
Table: `Matches`
+-------------+------+
| Column Name | Type |
+-------------+------+
| player_id   | int  |
| match_day   | date |
| result      | enum |
+-------------+------+
```

Matches 테이블에는 플레이어별로 아이디와 경기 날짜, 경기 결과가 저장되어 있음.    
플레이어별로 연속해서 승리한 최대 횟수를 구하시오. (0인 경우도 포함)    

​    

##### 문제 접근 방법

플레이어별로 경기 순서에 따라 ROW NUMBER를 붙인 후,    
경기 결과가 win인 경우에 한정해 다시 row number를 붙이고 이를 원래 row number에서 빼면 연속 승리한 그룹별로 번호를 매길 수 있다.    
이 그룹 번호별로 경기 count를 구하면 최대 연속 승리 횟수를 알 수 있다.    

​     

##### 문제풀이

```
WITH cte AS (
SELECT *, 
    lag(result) OVER (PARTITION BY player_id ORDER BY match_day) as lag_result,
    ROW_NUMBER() over (PARTITION BY player_id ORDER BY match_day) as rn
FROM Matches
), cte2 AS (
SELECT *, rn - ROW_NUMBER() over (PARTITION BY player_id ORDER BY match_day) as win_group
FROM cte
WHERE result='Win'
)
SELECT tmp.player_id, IFNULL(tmp2.ls,0) as longest_streak 
FROM (SELECT DISTINCT player_id FROM Matches) tmp
LEFT JOIN (
    SELECT player_id, MAX(consecutive_wins) as ls
    FROM (
    SELECT player_id, win_group, count(*) as consecutive_wins
    FROM cte2
    GROUP BY 1,2
    ) tmp3
GROUP BY 1
) tmp2
ON tmp.player_id = tmp2.player_id
;
```


