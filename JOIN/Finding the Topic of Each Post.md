### Finding the Topic of Each Post

------

문제 주소: https://leetcode.com/problems/finding-the-topic-of-each-post/



##### 문제

```
Table: `Keywords`
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| topic_id    | int     |
| word        | varchar |
+-------------+---------+
Table:  `Posts`
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| post_id     | int     |
| content     | varchar |
+-------------+---------+
```

각 토픽별로 키워드가 있을 때 포스트의 토픽 아이디를 구하시오.    
포스트의 내용에 토픽 키워드가 포함돼있으면 해당 포스트의 토픽으로 보며,    
토픽이 2개 이상인 경우 ','로 연결합니다.    
토픽이 없는 경우는 'Ambiguous!'를 출력하시오.

​    

##### 문제 접근 방법
정규식을 이용해서 토픽 키워드가 내용에 포함되어 있는 조건으로 두 테이블을 조인한다.    
포스트별로 GROUP_CONCAT을 이용해 2개 이상의 토픽을 concat한다.     


​     

##### 문제풀이

```
WITH cte AS (
    SELECT
    p.post_id,
    k.topic_id as topic_id
    FROM Posts p
    LEFT JOIN Keywords k
    ON p.content REGEXP concat('\\b' , k.word, '\\b')
    order by 1, 2
)
SELECT
 distinct post_id,
 IFNULL(group_concat(distinct topic_id order by topic_id), 'Ambiguous!') as topic
FROM cte 
group by 1
;
```


