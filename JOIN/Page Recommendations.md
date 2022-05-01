### Page Recommendations

------

문제 주소: https://leetcode.com/problems/page-recommendations/



##### 문제

```
Table: Friendship
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user1_id      | int     |
| user2_id      | int     |
+---------------+---------+

Table: Likes
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| page_id     | int     |
+-------------+---------+
```

Friendship 테이블에는 서로 친구인 유저의 아이디가 같은 행에 저장되어 있고, Likes 테이블에는 각 유저가 좋아하는 페이지가 저장되어 있다.     

User 1에게 그의 친구들이 좋아한 페이지를 추천하시오. 단, user 1이 이미 좋아하고 있는 페이지는 제외하시오.    

​     

##### 문제 접근 방법

Friendship 테이블에서 양 컬럼의 순서를 바꾼 후 원래 테이블과 조인해서 전체 친구 목록을 생성함.    

이후 친구들(user2_id) 컬럼과 Likes테이블의 page_id 컬럼이 같은 경우를 기준으로 조인해서 page_id의 값을 중복없이 출력한다.    

​     

##### 문제풀이

```
with cte as
(select * from Friendship
union all
select user2_id, user1_id from Friendship
)
select
distinct l.page_id as recommended_page
from cte c
join Likes l
on c.user2_id = l.user_id
where c.user1_id = 1 
and l.page_id not in (select page_id from Likes where user_id=1)
;
```

