### Occupations

------

문제 주소: https://www.hackerrank.com/challenges/occupations



##### 문제

```
Table: Occupations
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| Name         | String  |
| Occupation   | String  |
+--------------+---------+
```

직업별로 해당 직종에 종사하고 있는 사람의 이름이 나오도록 pivot 테이블을 작성할 것. 직업은 Doctor, Professor, Singer, Actor 네 가지만 있음. 직업별로 사람의 이름은 오름차순으로 작성되어야 함.    

​    

##### 문제 접근 방법

직업군을 그룹으로 하여 사람 이름 오름차순으로 순위를 매긴 후, 해당 테이블에서 GROUP_CONCAT() 함수를 사용해 직업군별 사람 이름을 출력함.     

직업별로 해당 직군에 해당하는 경우만을 출력하도록 GROUP_CONCAT() 함수 안에 IF 함수를 사용했음.    

​     

##### 문제풀이

```
SELECT GROUP_CONCAT(IF(Occupation='Doctor', NAME, NULL)) as 'Doctor',
GROUP_CONCAT(IF(Occupation='Professor', NAME, NULL)) as 'Professor',
GROUP_CONCAT(IF(Occupation='Singer', NAME, NULL)) as 'Singer',
GROUP_CONCAT(IF(Occupation='Actor', NAME, NULL)) as 'Actor'  
FROM (
SELECT NAME,
    Occupation,
    RANK() OVER (PARTITION BY Occupation ORDER BY NAME) AS row_num
FROM OCCUPATIONS 
    ) AS OCC_RANK
GROUP BY row_num
;
```

​    
