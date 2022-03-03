### Friend Requests II: Who Has the Most Friends

------

문제 주소: https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/



##### 문제

```
Table: SurveyLog
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| action      | ENUM |
| question_id | int  |
| answer_id   | int  |
| q_num       | int  |
| timestamp   | int  |
+-------------+------+
```

action 컬럼은 'show', answer','skip'중 하나의 값을 가짐. show는 문항이 제시된 경우이고 answer은 답을 한 경우, skip은 문항을 건너뛴 경우라고 했을 때 문항을 답한 비율이 가장 높은 문항이 무엇인지 구하시오. 2개 이상이 가장 높은 값을 갖는 경우 문항의 id값이 작은 경우를 출력하시오.     

​    

##### 문제 접근 방법

case when 절로 action별 id를 카운트 해서 비율을 구하고, 비율이 높은 순 + id값이 낮은 순으로 정렬한 뒤 여기서 limit 1로 첫 행만 출력한다.    

​     

##### 문제풀이

```
select question_id as survey_log
    from (
    select
        question_id,
        count(case when action ='answer' then id else null end) / count(case when action ='show' then id else null end) as answered_rate
    from SurveyLog
    group by question_id
    order by 2 desc, 1 asc  
    ) tmp
limit 1
;
```

​    
