### Consecutive Numbers

------

문제 주소: https://leetcode.com/problems/consecutive-numbers/



##### 문제

```
Table: Logs
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| num         | varchar  |
+-------------+----------+
```

id별로 숫자(num 컬럼)가 저장되어 있는 Logs 테이블에서 id 연속 3개 행으로 같은 숫자가 저장되어 있는 경우, 해당 숫자들을 distinct 하게 출력하시오.    

​     

##### 문제 접근 방법

테이블을 2번 셀프 조인하고, 셀프 조인한 테이블에서 숫자가 같은 경우만 WHERE절을 통해 추출한다.    

id값이 연속이어야 하므로 WHERE절에 각 테이블의 id값 차이가 1씩 나게 조건을 준다.    

​     

##### 문제풀이

```
SELECT
   DISTINCT l1.num as ConsecutiveNums 
FROM Logs l1, Logs l2, Logs l3
WHERE l1.num = l2.num  and l2.num=l3.num
  and (l3.id-l2.id=1 and l2.id-l1.id=1)
;
```

