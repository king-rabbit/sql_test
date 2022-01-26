### The Report

------

문제 주소: https://www.hackerrank.com/challenges/the-report



##### 문제

```
Table: Students
+--------------+---------------+
| Column Name  | Type          |
+--------------+---------------+
| ID           | Integer       |
| Name         | String        |
| Marks        | Integer       |
+--------------+---------------+

Table: Grades
+--------------+---------------+
| Column Name  | Type          |
+--------------+---------------+
| Grade        | Integer       |
| Min_Mark     | Integer       |
| Max_Mark     | Integer       |
+--------------+---------------+
```

Grades 테이블에는 Grade별로 최소 점수와 최대 점수값이 저장되어 있음. 이 정보를 이용해 각 학생의 이름, 등급, 점수를 등급 내림차순으로 출력하시오.    

이 때 등급 8 미만인 학생의 경우 이름을 출력하지 않고, 등급이 같은 경우 점수 오름차순으로 출력할 것.        

또 등급 8 이상인 학생들의 경우 등급이 같으면 이름 오름차순을 출력할 것.    

​    

##### 문제 접근 방법

등급에 따라 정렬 기준을 다르게 줘야하므로, CASE WHEN문을 사용해 등급 그룹(8이상, 8미만)에 따라 서로 다른 rank를 매김.    

이후 해당 서브테이블에서 등급(내림차순), 이름, 점수 순으로 정렬해 데이터를 출력한다.    

​     

##### 문제풀이

```
SELECT
    name,
    grade,
    mark
FROM(
SELECT 
    CASE WHEN g.Grade >=8 THEN s.Name ELSE NULL END as name, 
    g.Grade as grade,
    s.Marks as mark,
    CASE WHEN g.Grade >=8 THEN RANK() OVER (ORDER BY s.Name ) ELSE NULL END as rank_gt_eight,
    CASE WHEN g.Grade <8 THEN RANK() OVER (ORDER BY s.Marks ) ELSE NULL END as rank_lt_eight
FROM Students s
    JOIN Grades g
        ON s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
) AS temp_tbl
ORDER BY grade desc, rank_gt_eight, rank_lt_eight
;
```

​    
