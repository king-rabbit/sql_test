### Binary Tree Nodes

------

문제 주소: https://www.hackerrank.com/challenges/binary-search-tree-1



##### 문제

```
Table: BST
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| N           | Integer  |
| P		      | Integer  |
+-------------+----------+
```

 N은 node, P는 노드별 parent를 의미함. 루트 노드는 Root로, leaf 노드(더이상 자식이 없는 마지막 노드)는 Leaf로, 둘다 아닌 노드는 Inner 노드로 출력하시오.    

​    

##### 문제 접근 방법

Root 노드는 부모가 없는 노드이기 때문에 부모가 없는 노드의 경우 Root 노드로 출력한다.    

셀프 조인을 통해 BST 테이블의 각 노드(N)를 Parent로 갖고 있는 자식 노드가 무엇인지 출력하고, 자식이 없는 노드는 Leaf로 출력하면 된다.    

셀프 조인 후 CASE WHEN 문을 사용해 노드별 역할 값을 돌려준다.    

​     

##### 문제풀이

```
SELECT DISTINCT node as N,
    CASE WHEN ISNULL(child) THEN 'Leaf'
        WHEN ISNULL(parent) THEN 'Root'
        ELSE 'Inner'
        END as VALUE
FROM (
SELECT  b2.N as child,
        b1.N as node,
        b1.P as parent
FROM BST b1
    LEFT JOIN BST b2
        ON b1.N = b2.P
) AS temp_tbl
ORDER BY N
;
```

