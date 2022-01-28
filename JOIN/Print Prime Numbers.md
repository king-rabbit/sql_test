### Print Prime Numbers

------

문제 주소: https://www.hackerrank.com/challenges/print-prime-numbers/



##### 문제

1000까지의 숫자 중 소수를 출력하시오. 각 소수를 '&'로 연결해 한줄로 출력합니다.    

​        

##### 문제 접근 방법

1000까지의 숫자를 출력하기 위해 information_schema.tables 테이블을 병합하고 숫자 변수(nu, num)를 설정해 2부터 1000까지 출력한다.    

이 중 자기 자신보다 작은 값(1이 아닌)으로 나눴을 때 나머지가 0이 되지 않는 값을 출력하고, 이를 group_concat 함수로 연결한다.       

​     

##### 문제풀이

```
select group_concat(numb separator '&')
from ( select @num:=@num+1 as numb
     from information_schema.tables t1,
          information_schema.tables t2,
          (select @num:=1) tn1
) temp_tbl
where numb <= 1000
and not exists (select *
               from (
                select @nu:=@nu+1 as numa
                from information_schema.tables t1,
                  information_schema.tables t2,
                  (select @nu:=1) tn2
                   limit 1000
               ) temp_tbl2
                where floor(numb/numa) = (numb/numa)
                    and numa < numb
                    and 1 < numa
               )
;
```

