### Consecutive Avaliable Seats

------

문제 주소: https://leetcode.com/problems/consecutive-available-seats



##### 문제

```
Table: Cinema
+-------------+------+
| Column Name | Type |
+-------------+------+
| seat_id     | int  |
| free        | bool |
+-------------+------+
```

영화관 좌석 데이터 테이블에서 seat_id는 자동으로 증가하게 되어 있음. 비어 있는 좌석은 free=1, 그렇지 않으면 free=0일 때, 연속으로 비어있는 좌석을 모두 출력하시오.    

seat_id 기준으로 오름차순으로 정렬하시오.     

​     

##### 문제 접근 방법

2개 이상의 좌석이 연속으로 비어 있으면 되므로, Cinema 테이블을 자기 복제하되 양 테이블의 free 컬럼 값이 1이면서 seat_id 좌석 차이값이 1(연속 좌석)인 경우를 출력했다.     

​     

##### 문제풀이

```
select
    distinct c1.seat_id
from Cinema c1
join Cinema c2
on abs(c1.seat_id - c2.seat_id) = 1
and c1.free = 1 and c2.free = 1
order by seat_id
;
```

