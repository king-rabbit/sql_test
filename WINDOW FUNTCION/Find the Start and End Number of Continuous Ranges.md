### Find the Start and End Number of Continuous Ranges

------

문제 주소: https://leetcode.com/problems/find-the-start-and-end-number-of-continuous-ranges/



##### 문제

```
Table: Logs
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| log_id        | int     |
+---------------+---------+
```

연속된 id 값을 가진 그룹별로 시작 id와 마지막 id값을 구하시오.    

​    

##### 문제 접근 방법

연속된 값끼리를 같은 그룹으로 묶어주기 위해 해당 id에서 row number를 뺀 값을 구한다.    

그 컬럼으로 group by해서 id값의 최소값과 최대값을 구하면 된다.    

​     

##### 문제풀이

```
SELECT 
    MIN(log_id) as start_id,
    MAX(log_id) as end_id
FROM (
SELECT
    log_id,
    log_id - ROW_NUMBER() OVER (ORDER BY log_id) as rn
FROM Logs
) tmp
GROUP BY rn
ORDER BY start_id 
;
```

