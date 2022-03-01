### Reported Posts II

------

문제 주소: https://leetcode.com/problems/reported-posts-ii/



##### 문제

```
Table: Actions
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| post_id       | int     |
| action_date   | date    | 
| action        | enum    |
| extra         | varchar |
+---------------+---------+


Table: Removals
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| post_id       | int     |
| remove_date   | date    | 
+---------------+---------+
```

Actions 테이블에는 포스트와 포스트를 작성한 유저의 아이디, 포스트에 취해진 action과 날짜, action이 취해진 이유(extra)가 저장되어 있음. (action은 ('view', 'like', 'reaction', 'comment', 'report', 'share') 중 하나임.)    

Removals에는 삭제 조치된 포스트의 아이디와 삭제 날짜가 저장됨.    

spam으로 report된 포스트 중 삭제 조치된 비율을 날짜별로 구한 뒤 그 평균을 구하시오.    

​     

##### 문제 접근 방법

LEFT JOIN으로 스팸으로 신고된 포스트의 삭제 날짜를 출력하는 임시 뷰를 생성한다. (Actions 테이블에서 중복된 row가 있을 수 있다고 해서 post_id는 DISTINCT하게 출력해야 한다.)     

여기서 삭제 날짜가 없는 경우 삭제되지 않은 포스트이므로 이를 이용해 날짜별 삭제 비율을 구하고, 마지막으로 그 평균을 구한다.    

​     

##### 문제풀이

```
WITH cte AS (
SELECT
    DISTINCT a.post_id,
    a.action_date,
    IFNULL(r.remove_date, '1999-01-01') AS remove_date
FROM Actions a
    LEFT JOIN Removals r
        ON a.post_id = r.post_id
WHERE a.action = 'report' AND a.extra='spam' 
)
SELECT ROUND(AVG(rm_rate) * 100 , 2) AS average_daily_percent
FROM (
SELECT action_date,
        COUNT(CASE WHEN remove_date != '1999-01-01' THEN post_id ELSE NULL END) / COUNT(*)  AS rm_rate
FROM cte
GROUP BY action_date
    ) AS tmp
;select
    distinct c1.seat_id
from Cinema c1
join Cinema c2
on abs(c1.seat_id - c2.seat_id) = 1
and c1.free = 1 and c2.free = 1
order by seat_id
;
```

