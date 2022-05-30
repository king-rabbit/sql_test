###  Immediate Food Delivery II

------

문제 주소: https://leetcode.com/problems/immediate-food-delivery-ii/



##### 문제

```
Table: Delivery
+-----------------------------+---------+
| Column Name                 | Type    |
+-----------------------------+---------+
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
+-----------------------------+---------+

```
주문한 날짜와 배송을 원하는 날짜가 같으면 immidiate하다고 할때,
전체 고객 중 첫번째 주문이 immidate 배송에 해당하는 고객의 비율을 구하시오.

​    

##### 문제 접근 방법
고객별로 날짜순으로 첫번째 row를 구하기 위해 rank를 구한다.       
rank가 1인 row 중 주문일과 배송 희망일이 같은 경우의 새로운 컬럼에 1을 주고, 그렇지 않으면 0을 준다.    
해당 컬럼의 평균치를 구한다.    

​      

##### 문제풀이

```
SELECT ROUND(AVG(immidiate) * 100, 2) as immediate_percentage
FROM (
SELECT *
FROM (
    SELECT delivery_id,
        customer_id, 
        rank() over (partition by customer_id order by order_date) order_rank,
        case when order_date = customer_pref_delivery_date then 1 else 0 end as immidiate
    FROM Delivery
    ) TMP
WHERE order_rank=1
    ) TMP2
;

```
