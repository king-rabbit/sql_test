### Find Cutoff Score for Each School

------

문제 주소: https://leetcode.com/problems/find-cutoff-score-for-each-school/



##### 문제

```
Table: Schools
+-------------+------+
| Column Name | Type |
+-------------+------+
| school_id   | int  |
| capacity    | int  |
+-------------+------+

vTable: Exam
+---------------+------+
| Column Name   | Type |
+---------------+------+
| score         | int  |
| student_count | int  |
+---------------+------+

```

Exam 테이블에 점수별로 해당 점수 이상을 받은 학생의 수가 저장되어 있고, Schools 테이블에는 학교별 아이디와 함께 학교가 뽑을 수 있는 최대 학생 수가 저장되어 있음.    

학교들은 Exam 테이블에 저장되어 있는 점수를 기반으로 입학 가능 커트라인 점수를 정하려고 함. 최대한 많은 학생을 뽑되, 해당 커트라인을 만족하는 학생은 모두 뽑을 수 있게(즉, 학교 최대 수용 인원을 초과하는 일이 없게) 커트라인을 정하시오.                  

커트라인을 정할 수 없는 경우 -1를 리턴하시오.    

​    

##### 문제 접근 방법

학교별 최대 인원이 점수별 인원보다 크다는 조건을 WHERE절로 주고, 학교별로 Exam score의 최저 점수를 구한다.    

이렇게 구한 테이블과 조건을 만족하는 점수가 없는 경우 -1을 주는 테이블을 JOIN해서 최종 결과를 구한다.    

​     

##### 문제풀이

```
WITH cte AS (
SELECT
    tmp.school_id,
    min(Exam.score) AS score
FROM (
SELECT s.school_id, s.capacity, max(e.student_count) as max_cnt
FROM Schools s, Exam e
WHERE s.capacity >= e.student_count
GROUP BY 1
) as tmp
JOIN Exam
    ON tmp.max_cnt = Exam.student_count
GROUP BY tmp.school_id
)
SELECT * FROM cte
UNION
SELECT school_id, -1 as score
FROM Schools
WHERE Schools.school_id NOT IN (SELECT school_id FROM cte)
;
```

​    
