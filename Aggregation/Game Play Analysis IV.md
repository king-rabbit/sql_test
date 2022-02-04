### Game Play Analysis IV

------

문제 주소: https://leetcode.com/problems/game-play-analysis-iv/



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

player_id, event_date가 primary key임.    

플레이어별, 날짜순으로 플레이한 게임의 누적 개수를 구하시오.           

​    

##### 문제 접근 방법

플레이어 아이디와 날짜를 기준으로 그룹핑해 게임 누적 개수를 집계한다. 플레이어별로, 날짜 순으로 집계해야 하므로 partition by, order by 절을 이용해 합계를 구한다.    

​     

##### 문제풀이

```
SELECT
 player_id,
 event_date,
 sum(games_played) over(partition by player_id order by event_date) as games_played_so_far
FROM activity
group by player_id, event_date
;
```

​    
