### Last Person to Fit in the Bus

------

문제 주소: https://leetcode.com/problems/last-person-to-fit-in-the-bus/



##### 문제

```
Table: Queue
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |
+-------------+---------+
```

turn이 버스에 탈 순서이고, 버스에 실을 수 있는 무게가 최대 1000kg이라고 했을 때 마지막에 타는 승객의 이름을 구하시오.    

​    

##### 문제 접근 방법

turn 순서대로 줄을 세워서 SUM() OVER()절로 누적합을 구한 뒤, 누적합이 1000 아래인 경우 중 누적합이 가장 많은 경우(마지막 승객)의 이름을 출력한다. 누적합 순서대로 정렬한 뒤 limit 1를 줘서 최대값인 경우를 출력하면 된다.    

​     

##### 문제풀이

```
SELECT
    person_name
FROM (
    SELECT
        turn,
        person_id,
        person_name,
        SUM(weight) OVER (ORDER BY turn) as tot_weight
    FROM Queue
    ORDER BY turn
) AS tmp
WHERE tot_weight <= 1000
ORDER BY tot_weight desc
LIMIT 1
;
```

