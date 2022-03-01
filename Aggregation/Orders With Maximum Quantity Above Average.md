### Orders With Maximum Quantity Above Average

------

문제 주소: https://leetcode.com/problems/orders-with-maximum-quantity-above-average/



##### 문제

```
Table: OrdersDetails
+-------------+------+
| Column Name | Type |
+-------------+------+
| order_id    | int  |
| product_id  | int  |
| quantity    | int  |
+-------------+------+
```

주문별로 상품 종류 대비 평균 주문 수량 (주문별 전체 수량 / 주문별 상품 종류 개수)를 구하고, 주문별 row에서 quantity의 최대값을 구한뒤, row별 최대 수량이 주문별 평균 수량의 최대값보다 큰 주문 번호를 구하시오.     

​    

##### 문제 접근 방법

주문별 평균 수량의 최대값을 먼저 구한 뒤, 이 최대값보다 주문수량의 최대값이 더 큰 order의 order_id를 출력한다. 마지막에 having절로 조건을 설정ㅎ나다.    

​     

##### 문제풀이

```
SELECT
    order_id
FROM OrdersDetails
GROUP BY 1
HAVING max(quantity) > (
    SELECT MAX(avg_quantity)
    from(
        SELECT
            order_id,
            sum(quantity) / count(distinct product_id) AS avg_quantity
        FROM OrdersDetails
        GROUP BY order_id ) as avg_per_order
)
;
```

​    
