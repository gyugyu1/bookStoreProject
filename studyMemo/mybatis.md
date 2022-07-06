# mybatis 메소드 정리  

## **forEach**  

`collection` : 전달받은 인자. List or Array 형태만 가능  
`item` : 전달받은 인자 값을 alias 명으로 대체  
`open` : 구문이 시작될때 삽입할 문자열  
`close` : 구문이 종료될때 삽입할 문자열  
`separator` : 반복 되는 사이에 출력할 문자열  
`index` : 반복되는 구문 번호이다. 0부터 순차적으로 증가  
ex)
```XML
<foreach item = "item" collection="list" open = "INSERT ALL" separator = " " close = "SELECT * FROM DUAL" > 
INTO t_imageFile(
    imageFileNO,
    imageFileName,
    articleNO,
    regDate) 
VALUES (
    #{item.imageFileNO},
    #{item.imageFIleName},
    #{item.articleNO},
    sysdate)
</foreach>
<insert>
```
>\* INSERT ALL : 여러 테이블에 여러 각기 다른 테이터를 넣을 때 사용   
ex) insert all into  테이블이름x(칼럼이름a,칼럼이름b) values (값a,값b)  
<BR>
\* DUAL 테이블 : 임시 테이블으로서 원하는 SELECT를 테이블 없이 실행 가능하게 해준다.  
ex) SELECT 38 * 23 FROM DUAL; 은 DUAL 테이블을 이용하여 결과 값을 나타낸다.



