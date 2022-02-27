### Article Views II

------

문제 주소: https://leetcode.com/problems/article-views-ii/



##### 문제

```
Table: Views
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |
+---------------+---------+
```

같은 날에 2개 이상의 아티클을 읽은 독자의 아이디를 오름차순으로 출력하시오.      

중복된 row가 있을 수 있음.    

​    

##### 문제 접근 방법

id와 날짜로 집계해서 각 독자가 날짜별로 읽은 article 개수를 집계할 수 있게 한다. 같은 기사를 2번 읽었을 수 있으므로 article_id는 distinct하게 집계해야 한다.    

Article_id가 2개 이상인 경우를 having절로 조건을 주고, id순으로 정렬한다.     

​     

##### 문제풀이

```
select
distinct viewer_id as id
from Views
group by viewer_id, view_date
having count(distinct article_id) > 1
order by id
;
```

​    
