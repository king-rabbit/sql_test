### Unpopular Books

------

문제 주소: https://leetcode.com/problems/unpopular-books/



##### 문제

```
Table: Books
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| book_id        | int     |
| name           | varchar |
| available_from | date    |
+----------------+---------+

Table: Orders
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| order_id       | int     |
| book_id        | int     |
| quantity       | int     |
| dispatch_date  | date    |
+----------------+---------+
```

오늘이 2019년 6월 23일이라고 했을 때 지난 1년간 10권 이하로 팔린 책의 아이디와 이름을 출력하시오.    

(0권 팔린 경우도 출력해야 함).

책이 출간된 지 한달이 되지 않은 경우는 제외하시오.    

​    

##### 문제 접근 방법

where와 having절을 사용하면 전혀 팔리지 않은 책을 출력할 수 없기 때문에 10권 이상 팔린 책을 집계한 뒤,

여기에 포함되지 않고 출간된 지 한달이상된 책 목록을 출력하는 식으로 문제를 해결했다.    

 

​     

##### 문제풀이

```
with cte as (
select
    b.book_id
from books b
    left join orders o
        on b.book_id = o.book_id
WHERE o.dispatch_date between date_sub('2019-06-23', interval 1 year) and '2019-06-23'
group by b.book_id
having sum(o.quantity) >= 10
)
select
    book_id, name
from books
where book_id not in (select * from cte)
    and available_from < date_sub('2019-06-23', interval 1 MONTH)
;
```

​    
