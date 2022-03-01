### Friend Requests II: Who Has the Most Friends

------

문제 주소: https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/



##### 문제

```
Table: RequestAccepted

+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |
+----------------+---------+
```

RequestAccepted에는 요청을 보낸 사람의 아이디와 받은 사람의 아이디가 저장되어 있다. 요청을 하거나 받은 사이가 친구라고 했을 때, 친구가 가장 많은 사람의 아이디와 그의 친구 수를 출력하시오.    

​    

##### 문제 접근 방법

요청을 한 사람의 아이디별 count와 요청을 받은 사람별 count를 합쳐서 총 친구수를 구한다. 이 때 두 테이블을 union all해야 중복 컬럼이 빠지는 걸 피할 수 있다(union하면 제외됨). 여기서 where절을 이용해 친구수가 max값에 해당하는 id와 해당 친구 수를 구한다.     

​     

##### 문제풀이

```
WITH cte AS (
    SELECT id,
            sum(cnt) as num
    FROM (
    SELECT requester_id  AS id,
          COUNT(accepter_id) AS cnt
    FROM RequestAccepted
    GROUP BY 1
    UNION ALL
    SELECT accepter_id  AS id,
          COUNT(requester_id) AS cnt
    FROM RequestAccepted
    GROUP BY 1
) AS tmp
GROUP BY 1
)
SELECT id, num
FROM cte
WHERE num = (SELECT MAX(num) FROM cte)
;
```

​    
