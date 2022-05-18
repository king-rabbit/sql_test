### Human Traffic of Stadium

------

문제 주소: https://leetcode.com/problems/human-traffic-of-stadium/



##### 문제

```
Table: Stadium
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| visit_date    | date    |
| people        | int     |
+---------------+---------+

```
 경기별로 고유 id와 날짜, 관중 수(people)가 저장된 Stadium 테이블에서,    

id 순서상 3회 연속 관중 수가 100명 이상인 경우에 해당하는 경기들을 구하시오.    

​    

##### 문제 접근 방법
3회 연속이 되기 위해서는 해당 첫 ROW의 경우 다음과 다다음 ROW의 관중 수가 100이상이어야 하고,    

두번째 ROW의 경우 전 ROW와 다음 ROW의 관중 수가 100이상이어야,    

세번째 이상의 ROW에서는 이전 ROW와 2번째 전 ROW의 관중 수가 100이상이어야 한다.    

LAG 함수(이전 행 값 추출)와 LEAD 함수(다음 행 값 추출)을 이용해 2단계 전/후 관중 수를 구하는 컬럼을 만든 후 위 조건에 해당하는 ROW들만 추출한다.    

​      

##### 문제풀이

```
WITH cte AS (
SELECT
id, 
visit_date,
people,
LAG(people, 1) OVER (ORDER BY visit_date) as lag_people,
LAG(people, 2) OVER (ORDER BY visit_date) as lag_people_2,
LEAD(people, 1) OVER (ORDER BY visit_date) as lead_people,
LEAD(people, 2) OVER (ORDER BY visit_date) as lead_people_2,
ROW_NUMBER() OVER (PARTITION BY id) 
FROM Stadium
ORDER BY visit_date
) 
SELECT id, visit_date, people
FROM cte
WHERE (people >= 100 and lag_people >= 100 and lag_people_2 >= 100 )
    or (people >= 100 and lag_people >= 100 and lead_people >= 100 )
    or (people >= 100 and lead_people >= 100 and lead_people_2 >= 100 )
;
```
