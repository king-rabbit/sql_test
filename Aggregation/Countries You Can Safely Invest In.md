### Countries You Can Safely Invest In

------

문제 주소: https://leetcode.com/problems/countries-you-can-safely-invest-in/



##### 문제

```
Table: Person
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| id             | int     |
| name           | varchar |
| phone_number   | varchar |
+----------------+---------+

Table: Country
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| name           | varchar |
| country_code   | varchar |
+----------------+---------+

Table: Calls
+-------------+------+
| Column Name | Type |
+-------------+------+
| caller_id   | int  |
| callee_id   | int  |
| duration    | int  |
+-------------+------+

```

Calls테이블의 caller_id와 callee_id는 Person테이블의 id를 참조하고,    

Person테이블의 phone_number 앞 3자리는 Country 테이블의 country_code를 의미함.    

전화회사가 전세계 평균보다 통화시간이 긴 나라를 찾아서 그 국가에 투자하고자 한다. 전세계 평균보다 통화시간이 긴 국가의 이름을 출력하시오.    

​     

##### 문제 접근 방법

Calls 테이블을 Person 테이블과 조인해 caller_id와 callee_id에 해당하는 국가코드를 추출한다.    

이 임시테이블을 Country 테이블과 조인해 나라 이름을 출력하되, 국가코드로 group by를 해서 통화 시간의 평균을 집계하고, 시계 평균보다 높은 나라만 출력하도록 having 절에 조건을 준다.    

​     

##### 문제풀이

```
SELECT
    Country.name as country
FROM (
    SELECT
        left(p1.phone_number,3) AS country_code,
        Calls.duration AS call_duration
    FROM Calls
        JOIN  Person p1
        ON Calls.caller_id = p1.id
    UNION ALL
    SELECT
        left(p2.phone_number,3) AS country_code,
        Calls.duration AS call_duration
    FROM Calls
        JOIN  Person p2
        ON Calls.callee_id = p2.id
) AS tmp
    JOIN Country
        ON Country.country_code = tmp.country_code
GROUP BY tmp.country_code
HAVING AVG(call_duration) > (SELECT avg(duration) from Calls)
;
```

