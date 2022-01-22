### Weather Observation Station 2-

------

문제 주소: https://www.hackerrank.com/challenges/weather-observation-station-20/



##### 문제

```
Table: Station
+--------------+---------------+
| Column Name  | Type          |
+--------------+---------------+
| ID           | NUMBER        |
| CITY         | VARCHAR2(21)  |
| STATE        | VARCHAR2(2)   |
| LAT_N        | NUMBER        |
| LONG_W       | NUMBER        |
+--------------+---------------+
```

Nothern Latitudes(LAT_N 컬럼)의 median 값을 소수점 네 자리까지 구하시오.       

​    

##### 문제 접근 방법

0부터 데이터 크기만큼 행의 number를 매기기 위해 rownum 변수를 설정한다(기본값 -1).    

데이터의 크기가 홀수인 경우에는 데이터 크기  / 2 번째 값을,     

짝수인 경우에는 데이터의 크기 / 2 번째 값과 데이터 크기 / 2 + 1 번째 값의 평균값을 구해야 하므로.   

rownum 변수를 2로 나눈 몫의 floor와 ceil 값을 구한 뒤 해당하는 lat_n값의 평균을 구한다.    

​     

##### 문제풀이

```
set @rownum=-1;
select round( avg(lat_n), 4) as median
from (
    select @rownum:=@rownum+1 as rn,
    lat_n
    from station 
    order by lat_n
    ) as rank_tbl
where rn in ( floor( @rownum / 2 ), ceil( @rownum / 2 ) )
;
```

​    
