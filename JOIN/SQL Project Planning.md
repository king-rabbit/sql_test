### SQL Projects Planning

------

문제 주소: https://www.hackerrank.com/challenges/sql-projects



##### 문제

```
Table: Projects
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| Task_ID     | Integer  |
| Start_Date  | Date     |
| End_Date    | Date     |
+-------------+----------+
```

 각 task별로 id값이 주어지고 start_date와 end_date의 차이는 모두 1로 고정되어 있다. start_date가 연속된 task들의 경우 하나의 프로젝트에 속한다고 했을 때, 프로젝트별 start_date와 end_date를 출력하시오.    

프로젝트 기간에 따라 오름차순으로 정력하고, 기간이 동일하면 start_date가 이른 순으로 정렬하시오.    

​    

##### 문제 접근 방법

모든 프로젝트의 start_date와 end_date는 다른 task의  end_date, start_date가 아닌 경우에 해당한다. task의 start_date가 다른 task의 end_date인 경우 , 또는 end_date가 task의 start_date인 경우 프로젝트의 시작과 끝일 수가 없기 때문이다.    

따라서 이러한 경우가 제외되도록 start_date, end_date의 서브쿼리 테이블을 조회하고, start_date보다 end_date가 큰 경우에 한해 start_date로 group by를 해서 프로젝트별 시작일과 종료일을 조회한다.    

​     

##### 문제풀이

```
SELECT
 	START_DATE, MIN(END_DATE)
FROM 
	(SELECT START_DATE FROM Projects WHERE START_DATE NOT IN (SELECT END_DATE FROM Projects)) sd,
	(SELECT END_DATE FROM Projects WHERE END_DATE NOT IN (SELECT START_DATE FROM Projects)) ed
WHERE START_DATE < END_DATE
GROUP BY START_DATE
ORDER BY DATEDIFF(START_DATE, MIN(END_DATE)) DESC, START_DATE
;
```

