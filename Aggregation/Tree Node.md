### Tree Node

------

문제 주소: https://leetcode.com/problems/tree-node/



##### 문제

```
Table: Tree
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| p_id        | int  |
+-------------+------+
```

각 노드별로 id와 부모 노드인 p_id가 주어진 테이블임.    

자식이 없는 노드는 'Leaf'로, 부모가 없는 노드는 'Root'로, 둘다 아닌 경우는 'Inner'로 출력하시오.         

​     

##### 문제 접근 방법

CASE~WHEN구문을 통해 부모 노드가 NULL인 경우 'Root'로 출력하고, 그 누구의 부모 노드도 아닌 노드는 'Leaf' 노드로, 둘다 아닌 경우는 'Inner'로 출력한다.         

​     

##### 문제풀이

```
SELECT 
    id,
    CASE WHEN ISNULL(p_id) then 'Root'
        WHEN ISNULL(p_id)=FALSE and id IN (SELECT DISTINCT p_id FROM Tree) THEN 'Inner'
        ELSE 'Leaf'
    END AS type
FROM Tree
;
```

