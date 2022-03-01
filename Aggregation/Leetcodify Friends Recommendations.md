### Leetcodify Friends Recommendations

------

문제 주소: https://leetcode.com/problems/leetcodify-friends-recommendations/



##### 문제

```
Table: Listens
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| song_id     | int     |
| day         | date    |
+-------------+---------+

Table: Friendship
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user1_id      | int     |
| user2_id      | int     |
+---------------+---------+

```

어떤 날에 감상한 음악이 서로 다른 3곡 이상 겹치는 유저들을 서로에게 추천해주고자 한다. 기존에 친구인 경우에는 제외하고 추천한다고 했을 때, 추천 친구 목록을 출력하시오. 출력 시 추천받는 친구와 추천되는 친구 쌍을 복수로 출력해야 함.           

​    

##### 문제 접근 방법

기존에 친구인 경우를 제외해야 하는데 친구쌍의 순서와 무관하게 제외해야 하므로 Friendship 테이블에서 column의 순서를 바꾼 테이블을 기존 테이블과 합쳐서 뷰를 생성한다.     

날짜별로 같은 곡을 감상한 유저쌍을 추출하기 위해 날짜와 곡id가 같은 경우를 조건으로 Listens 테이블을 자기 join하고, 날짜와 유저아이디1, 유저아이디2로 group by를 한다. 이후 having절로 count값이 3 이상인 경우만 필터링한다.    

친구쌍이 중복으로 추천되는 경우가 있을 수 있어 distinct하게 유저 아이디값을 추출한다.    

테스트 중 timeout이 자꾸 발생했는데 Freindship테이블을 뷰로 처리하니 해결할 수 있었음!    

​     

##### 문제풀이

```
WITH cte(user_id1, user_id2) AS (
SELECT user1_id, user2_id FROM Friendship
UNION
SELECT user2_id, user1_id FROM Friendship
)
SELECT
    distinct l1.user_id AS user_id, 
    l2.user_id AS recommended_id
FROM Listens l1
JOIN Listens l2
ON l1.day = l2.day AND l1.song_id = l2.song_id AND l1.user_id <> l2.user_id
WHERE (l1.user_id, l2.user_id) NOT IN (SELECT * FROM cte)
GROUP BY l1.day, l1.user_id, l2.user_id
HAVING count(distinct l1.song_id) >= 3
;
```

​    
