### League Statistics

------

문제 주소: https://leetcode.com/problems/league-statistics/



##### 문제

```
Table: Teams
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| team_id        | int     |
| team_name      | varchar |
+----------------+---------+

Table: Matches
+-----------------+---------+
| Column Name     | Type    |
+-----------------+---------+
| home_team_id    | int     |
| away_team_id    | int     |
| home_team_goals | int     |
| away_team_goals | int     |
+-----------------+---------+
```

Teams 테이블에는 팀별 아이디와 팀명이, Matches 테이블에는 경기별로 홈팀과 원정팀의 아이디와 각 경기에서 홈 팀과 원정 팀이 득점한 골의 숫자가 저장되어 있음.    

팀별로 승리한 게임마다 3점, 패배한 게임마다 0점, 무승부인 게임마다 1점의 포인트를 부과함.     

팀별로 팀명과 총 경기 수, 총 포인트, 총 득점 골 수, 총 실점 골 수, 득점골과 실점골의 차이를 출력하시오.    

​    

##### 문제 접근 방법

경기별로 홈팀과 원정팀의 포인트를 계산한 임시 테이블을 만들고, 팀이 홈팀으로 경기를 한 경우와 원정팀으로 경기를 한 경우를 나눠서 테이블을 Join함.     

각 테이블에서 경기 수, 득점 및 실점, 포인트를 계산해 union한 뒤 이를 다시 더하는 방식으로 출력했음.        

​     

##### 문제풀이

```
WITH CTE AS (
SELECT 
    *,
    CASE 
        WHEN home_team_goals > away_team_goals THEN 3
        WHEN home_team_goals = away_team_goals THEN 1
        ELSE 0 END
    AS home_team_score ,
    CASE 
        WHEN home_team_goals < away_team_goals THEN 3
        WHEN home_team_goals = away_team_goals THEN 1
        ELSE 0 END
    AS away_team_score 
FROM Matches
)
SELECT
    team_name,
    SUM(matches_played) AS matches_played,
    SUM(points) as points,
    SUM(goal_for) AS goal_for,
    SUM(goal_against)  AS goal_against  ,
    SUM(goal_for) - SUM(goal_against) AS goal_diff
FROM (
    SELECT 
        t.team_name as team_name,
        COUNT(home_team_id) as matches_played,
        SUM(home_team_score) as points,
        SUM(home_team_goals) AS goal_for,
        SUM(away_team_goals ) AS goal_against  
    FROM CTE c
        JOIN Teams t
            ON c.home_team_id = t.team_id
    GROUP BY 1
    UNION
    SELECT 
        t.team_name as team_name,
        COUNT(away_team_id ) as matches_played,
        SUM(away_team_score) as points,
        SUM(away_team_goals ) AS goal_for,
        SUM(home_team_goals)  AS goal_against  

    FROM CTE c
        JOIN Teams t
            ON c.away_team_id  = t.team_id
    GROUP BY 1 
) AS tmp
GROUP BY 1
ORDER BY points DESC, goal_diff DESC, team_name
;
```

​    
