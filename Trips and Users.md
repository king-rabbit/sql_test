### Trips and Users

------

문제 주소: https://leetcode.com/problems/trips-and-users/



##### 문제

```
Table: Trips
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| client_id   | int      |
| driver_id   | int      |
| city_id     | int      |
| status      | enum     |
| request_at  | date     |     
+-------------+----------+

Table: Users
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| users_id    | int      |
| banned      | enum     |
| role        | enum     |
+-------------+----------+
```

클라이언트와 택시 드라이버가 모두 'banned' 상태가 아닌 경우에 한해 '2013-10-01'부터 '2013-10-03'까지 택시 운행이 취소된 비율을 계산할 것. 결과는 소수점 두자리까지 반올림해서 표시할 것.     

Trips의 id는 택시 운행별 id값이며, client_id와 driver_id 컬럼은 모두 Users 테이블의 users_id 컬럼을 참조하고 있음.    

​     

##### 문제 접근 방법

Trips의 두 컬럼이 Users 테이블의 한 컬럼을 참조하고 있어 각 컬럼에 대해 1번씩 조인을 총 2번해야 한다.     

클라이언트와 드라이버가 모두 'banned'가 아닌 케이스만 계산하도록 WHERE문에서 조건을 주고, COUNT( CASE ~ WHEN ~ ) 문으로 운행 콜이 취소된 경우만 계산해 비율을 계산했다.     

​     

##### 문제풀이

```
SELECT  Trips.request_at AS Day,
    ROUND(COUNT(DISTINCT CASE WHEN Trips.status LIKE 'cancelled%' THEN id ELSE NULL END) / COUNT(*), 2 ) AS 'Cancellation Rate'
FROM Trips
    JOIN Users u1
        ON Trips.client_id = u1.users_id
    JOIN Users u2
        ON Trips.driver_id  = u2.users_id
WHERE Trips.request_at BETWEEN '2013-10-01' AND '2013-10-03'
    AND u1.banned  = 'No'
     AND u2.banned  = 'No'
GROUP BY Trips.request_at
;
```

