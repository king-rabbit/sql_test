### Confirmation Rate

------

문제 주소: https://leetcode.com/problems/confirmation-rate/

##### 문제

```
Table: Signups
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+

Table: Confirmations
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
+----------------+----------+


```

Signups 테이블에는 유저와 유저별 가입 시각이, Confirmations 테이블에는 유저가 요청을 한 시각과 결과(action)가 저장돼 있다. action 컬럼은 요청이 통과된 경우에는 confirmed, 시간초과된 경우에는 timeout 값을 갖는다. 유저별로 요청이 받아들여진 비율을 구하시오.      

​     

##### 문제 접근 방법

case when 구문을 이용해 action이 confirmed인 경우만 count한다. 요청이 없는 유저의 경우에 0으로 계산해야하므로 ifnull함수로 0값을 처리해준다.     

​     

##### 문제풀이

```
SELECT
    s.user_id,
    IFNULL(ROUND(COUNT(CASE WHEN c.action = 'confirmed' THEN c.user_id ELSE NULL END) / COUNT(c.user_id), 2), 0) as confirmation_rate
FROM Signups s
    LEFT JOIN Confirmations c
        ON s.user_id = c.user_id
GROUP BY s.user_id
;
```

