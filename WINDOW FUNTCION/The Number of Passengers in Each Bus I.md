### The Number of Passengers in Each Bus I

------

문제 주소: https://leetcode.com/problems/the-number-of-passengers-in-each-bus-i/



##### 문제

```
Table: Buses
+--------------+------+
| Column Name  | Type |
+--------------+------+
| bus_id       | int  |
| arrival_time | int  |
+--------------+------+

Table: Passengers
+--------------+------+
| Column Name  | Type |
+--------------+------+
| passenger_id | int  |
| arrival_time | int  |
+--------------+------+
```

승객이 정류장에 도착한 시각보다 각 버스가 도착한 시각이 늦으면 승객은 해당 버스에 승차한다고 했을 때,     

버스별로 태운 승객의 수를 구하시오.    

​    

##### 문제 접근 방법

버스의 도착 시간이 승객의 도착 시간보다 크다는 조건으로 join을 한 뒤,    

버스별로 승객의 수를 구하고 이전 버스에 탑승한 승객의 수를 lag 함수를 이용해 빼준다.    

​     

##### 문제풀이

```
SELECT
 b.bus_id,
 COUNT(p.passenger_id) - ifnull(lag(COUNT(p.passenger_id)) over (order by b.arrival_time),0) AS passengers_cnt
FROM Buses b
    LEFT JOIN Passengers p
        ON b.arrival_time >= p.arrival_time
GROUP BY 1
ORDER BY 1
;
```

