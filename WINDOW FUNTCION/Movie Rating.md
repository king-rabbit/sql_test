### Movie Rating

------

문제 주소: https://leetcode.com/problems/movie-rating



##### 문제

```
Table: Movies
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| title         | varchar |
+---------------+---------+

Table: Users
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
+---------------+---------+

Table: MovieRating
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| user_id       | int     |
| rating        | int     |
| created_at    | date    |
+---------------+---------+
```

유저 중에 가장 많은 영화를 평가한 사람의 이름과,    

2020년 2월에 가장 평점이 높은 영화의 제목을 results라는 하나의 컬럼으로 출력하시오.   

동점자가 있는 경우 이름/제목의 사전순으로 1위만 출력하시오.    

​    

##### 문제 접근 방법

rank() 함수로 유저 / 영화의 순위를 매기는 뷰를 생성하고 여기서 순위가 1인 경우만 추출한다.      

​     

##### 문제풀이

```
with cte as (
    SELECT
    u.name as results,
    rank() over(order by count(distinct mr.movie_id) desc, u.name) as movie_cnt_r
    FROM MovieRating mr
    join Users u
    on mr.user_id = u.user_id
    group by 1
), cte2 as (
    SELECT
    m.title as results,
    rank() over(order by avg(mr.rating) desc, m.title) as movie_cnt_r
    FROM MovieRating mr
    left join Movies m
    on mr.movie_id = m.movie_id
    where date_format(mr.created_at, '%Y-%m') = '2020-02'
    group by 1
)
select results
from cte
where movie_cnt_r = 1
union
select results
from cte2
where movie_cnt_r = 1
;

```

​    
