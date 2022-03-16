### Strong Friendship

------

문제 주소: https://leetcode.com/problems/strong-friendship/



##### 문제

```
Table: Friendship
+-------------+------+
| Column Name | Type |
+-------------+------+
| user1_id    | int  |
| user2_id    | int  |
+-------------+------+
```

Friendship  테이블에서 같은 row의 user1_id 값과 user2_id값이 서로 친구라고 했을 때,    

(user1_id  < user2_id )

3명 이상 같은 친구를 갖고 있는 경우 strong friendship관계라고 볼 수 있음.    

여기에 해당하는 친구 관계와 공동 친구 수를 출력하시오.    

​     

##### 문제 접근 방법

 user2 id값이 user1 id값보다 작은 경우는 포함이 안되어 있기 때문에 두 행을 바꾼 테이블을 기존 테이블과 union 한다.    

이후 공동의 친구를 갖고 있는 경우를 파악하기 위해 user2컬럼이 같게 셀프 조인하고,    

(user1, user2) 튜플에 따라 GROUP BY를 해서 공동 친구의 숫자를 구한다.    

HAVING절로 숫자가 3명 이상인 경우만 필터링하며, 친구가 아닌 경우를 걸러내기 위해 WHERE절에서  (user1, user2) 튜플값이 기존 테이블에 포함된 경우만 추출하도록 한다.    

​     

##### 문제풀이

```
WITH cte AS (
SELECT * FROM Friendship
UNION
SELECT user2_id as user1_id, user1_id as user2_id FROM Friendship
)
SELECT 
    f1.user1_id,
    f2.user1_id as user2_id,
    COUNT(*) AS common_friend
FROM 
    cte f1
    JOIN cte f2
    ON f1.user2_id = f2.user2_id
    AND f1.user1_id < f2.user1_id
WHERE (f1.user1_id, f2.user1_id) IN (SELECT * FROM Friendship)
GROUP BY 1,2
HAVING COUNT(*) >= 3
;
```

