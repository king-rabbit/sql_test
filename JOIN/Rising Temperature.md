### Rising Temperature

------

문제 주소: https://leetcode.com/problems/rising-temperature/



##### 문제

```
Table: Weather
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| recordDate  | date     |
| temperature | int      |
+-------------+----------+
```

전날보다 기온이 높은 날짜의 id를 순서와 상관없이 출력하시오.    

​     

##### 문제 접근 방법

테이블을 셀프 조인해서 연속인 날짜의 기온을 한 행에 출력한 뒤 이 중 날짜가 1일 차이인 행들, 또 전날보다 기온이 높은 행에 대해서만 id 값을 출력하도록 한다.    

​     

##### 문제풀이

```
select w2.id
from Weather w1, Weather w2
where date_sub(w2.recordDate, interval 1 day) = w1.recordDate 
and w1.temperature < w2.temperature
;
```

