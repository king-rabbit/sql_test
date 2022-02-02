### Weather Observation Station 19

------

문제 주소: https://www.hackerrank.com/challenges/weather-observation-station-19



##### 문제

```
Table: Station
+-------------+--------------+
| Column Name | Type         |
+-------------+--------------+
| id          | number       |
| city        | varchar2(21) |
| state       | varchar2(21) |
| lat_n       | number       |
| long_w      | number       |
+-------------+--------------+
```

테이블에 있는 도시를 대상으로 위도와 경도의 최소값과 최대값을 구한 뒤 최소값 위치와 최대값 위치의 유클리디안 거리를 구하시오. 결과는 소수점 4자리까지 출력하시오.        

​     

##### 문제 접근 방법

위도와 경도의 최소값과 최대값을 min, max 함수로 구한 뒤 pow와 sqrt 함수를 이용해 유클리디안 거리를 구한다.    

​     

##### 문제풀이

```
select round( sqrt ( pow(la_max - la_min, 2) + pow(lo_max - lo_min, 2) ) , 4)
from (
select max(lat_n) as la_max, 
    min(lat_n) as la_min,
    max(long_w) as lo_max, 
    min(long_w) as lo_min
from station
) as min_max_tbl
;
```

