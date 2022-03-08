### Tournament Winners

------

문제 주소: https://leetcode.com/problems/tournament-winners/



##### 문제

```
Table: Players
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| player_id   | int   |
| group_id    | int   |
+-------------+-------+

Table: Matches
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| match_id      | int     |
| first_player  | int     |
| second_player | int     | 
| first_score   | int     |
| second_score  | int     |
+---------------+---------+
```

Player 테이블에는 플레이어별 id와 그룹이, Matches 테이블에는 경기별 아이디와 첫번째 플레이어, 두번째 플레이어의 아이디 그리고 각 플레이어의 점수가 저장되어 있다.    

그룹별로 가장 점수가 높은 플레이어의 아이디를 구하시오. 최다득점자가 2명 이상일 땐 아이디값이 낮은 사람으로 구하시오.    

​     

##### 문제 접근 방법

with절로 first player와 second player별 score를 각각 조회해 union을 통해 하나의 테이블로 만들어서 조회한다.    

여기서 다시 플레이어별 score의 합계를 보여주는 테이블을 조회하고,    

score합계의 max값에 해당하는 플레이어 id를 조회해 그룹별 최다득점자를 구한다. 2명 이상인 경우 플레이어 아이디가 가장 작은 값만 출력해야하므로 플레이어 아이디는 최소값으로 구한다.     

​     

##### 문제풀이

```
WITH cte AS (
SELECT
    first_player as player_id, first_score as score
FROM Matches
UNION ALL
SELECT
    second_player as player_id, second_score as score
FROM Matches
), cte2 AS (
    SELECT
        cte.player_id,
        p.group_id,
        SUM(cte.score) as score
    FROM cte
    LEFT JOIN Players p
    ON cte.player_id = p.player_id
    GROUP BY 1,2
)
SELECT cte2.group_id, MIN(cte2.player_id) AS PLAYER_ID
FROM cte2 
    JOIN (
        SELECT
             group_id, MAX(score) as score
        FROM cte2
        GROUP BY 1
        ) tmp
    ON cte2.group_id = tmp.group_id
    AND cte2.score = tmp.score
GROUP BY 1
;
```

