# 오라클 함수  
- ## **decode**
    IF ELSE 와 비슷한 기능을 수행
    ```sql
    decode(gender, 'm' , '남자' , 'f', '여자', '기타') 
            컬럼   조건1  결과1    조건2 결과2   ELSE
    ```
    else 부분은 생략이 가능하다 (null을 리턴)


    EX)
    ```SQL
    --주민등록번호(MY_NUM)이(가) 1또는3으로 시작하면 남자 아니면 여자
    SELECT 
    DECODE(SUBSTRING(MY_NUM,0,1),1||3,'남','여') AS GENDER
    FROM
    EMP_TABLE
    ```
*오라클에만 사용되는 함수이다.