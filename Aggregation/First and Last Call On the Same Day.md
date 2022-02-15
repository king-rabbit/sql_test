### First and Last Call On the Same Day

------

문제 주소: https://leetcode.com/problems/first-and-last-call-on-the-same-day/



##### 문제

```
Table: Calls
+--------------+----------+
| Column Name  | Type     |
+--------------+----------+
| caller_id    | int      |
| recipient_id | int      |
| call_time    | datetime |
+--------------+----------+
```

Calls 테이블에는 각 통화의 수신자, 발신자, 통화시각이 저장돼 있음.    

하루 이상 날짜별로 첫번째와 마지막으로 통화한 사람이 같은 사람을 순서와 상관없이 출력하시오.       

통화시 수신자이든 발신자이든 무관하게 같은 사람과 통화한 경우면 됨.    

​    

##### 문제 접근 방법

수신자/발신자 여부가 무관하기 때문에 caller_id와 recipient_id 구분을 없애는 뷰를 생성한다. 원래 테이블과 두 컬럼을 바꾼 테이블을 union하는 방식으로 생성했다.    

이후 사람마다 날짜별로 첫 통화시각과 통화 상대방을 출력하는 테이블, 마지막 통화시각과 통화 상대방을 출력하는 테이블을 만든다.    

두 테이블을 조인하되 통화 상대방이 같은 경우만 추출하도록 조건을 주고, 이 경우에 속한 id를 중복없이 출력해 결과를 얻는다.    

​     

##### 문제풀이

```
WITH cte AS 
(
    SELECT
        caller_id as id,
        recipient_id as w_id,
        call_time
    FROM Calls
    UNION 
    SELECT
        recipient_id as id,
        caller_id as w_id,
        call_time
    FROM Calls
)
SELECT
    DISTINCT tmp1.id as user_id
FROM
    (SELECT DATE(call_time) AS dt,
            id, 
            w_id
        FROM cte
        WHERE (DATE(call_time), id, call_time) IN (SELECT DATE(call_time), id, MIN(call_time) FROM cte GROUP BY 1,2) ) AS tmp1
JOIN    
    (SELECT DATE(call_time) AS dt,
            id, 
            w_id
        FROM cte
        WHERE (DATE(call_time), id, call_time) IN (SELECT DATE(call_time), id, MAX(call_time) FROM cte GROUP BY 1,2) ) AS tmp2
	ON tmp1.dt = tmp2.dt AND tmp1.id = tmp2.id
WHERE tmp1.w_id = tmp2.w_id
;
```

​    
