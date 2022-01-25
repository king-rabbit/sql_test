### Symmetric Pairs

------

문제 주소: https://www.hackerrank.com/challenges/symmetric-pairs



##### 문제

```
Table: Functions
+--------------+----------+
| Column Name  | Type     |
+--------------+----------+
| X            | Integer  |
| Y            | Integer  |
+--------------+----------+
```

테이블의 두 행이 서로 x1=y2, x2=y1인 경우 두 행이 대칭이라고 할 수 있다. 이렇게 대칭인 행을 distinct하게 x값 오름차순으로 출력하시오.    

​    

##### 문제 접근 방법

대칭인 행을 찾아내기 위해 x값과 y값이 교차로 동일한 행을 대상으로 셀프 조인을 한다.     

distinct한 쌍을 추출하기 위해 x값보다 y값이 큰 경우,    

또는 x값과 y값이 동일한 경우 count값이 2 이상인 경우를 추출한다.    

​     

##### 문제풀이

```
SELECT F1.X, F1.Y
FROM FUNCTIONS F1
    INNER JOIN FUNCTIONS F2
        ON F1.X=F2.Y AND F1.Y=F2.X
GROUP BY F1.X, F1.Y
HAVING COUNT(F1.X)>1 OR F1.X < F1.Y
ORDER BY F1.X
;
```

