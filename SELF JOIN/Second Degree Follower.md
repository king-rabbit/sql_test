### Second Degree Follower

------

문제 주소: https://leetcode.com/problems/second-degree-follower/



##### 문제

```
Table: Follow
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| followee    | varchar |
| follower    | varchar |
+-------------+---------+
```

follower가 followee를 팔로우하는 관계일 때,    

1명 이상을 팔로우하면서 1명 이상에게 팔로우되고 있는 사람(=Second Degree Follower)과 그 사람을 팔로우하는 사람의 수를 구하시오.          

​    

##### 문제 접근 방법

셀프 조인으로 follower를 팔로우하는 사람을 구하고, having절로 조건을 줘서 1명 이상을 팔로우하고 1명 이상에게 팔로우되는 사람의 목록을 출력한다.    

​     

##### 문제풀이

```
SELECT
f1.follower, count(distinct f2.follower) as num
FROM Follow f1
    JOIN Follow f2
        ON f1.follower = f2.followee
GROUP BY 1
HAVING count(distinct f1.followee) >= 1 and num >= 1
ORDER BY 1
;
```

​    
