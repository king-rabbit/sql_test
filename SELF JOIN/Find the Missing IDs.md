### Find the Missing IDs

------

문제 주소: https://leetcode.com/problems/find-the-missing-ids/



##### 문제

```
Table: Customers
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| customer_name | varchar |
+---------------+---------+
```

customer_id 중 missing id를 출력하시오. customer_id의 최대값보다 작은데 id값으로 저장돼있지 않으면 missing id 로 봅니다.    


##### 문제 접근 방법

recursive cte로 customer id값의 최대값까지 출력하는 임시 테이블을 만든 뒤,    
해당 테이블에 속하지 않은 id값을 where절을 이용해 구한다.    
    

##### 문제풀이
      
      
```
WITH recursive cte as (
    SELECT 1 AS n
    UNION ALL
    SELECT n+1
    FROM cte
    WHERE n < (SELECT MAX(customer_id) FROM Customers)
)
SELECT n AS ids
FROM cte
WHERE n NOT IN (SELECT customer_id FROM Customers)
;
```


