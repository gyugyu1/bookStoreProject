# **Mybatis 메소드 정리**  

- ## **forEach**  

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

---
- ## **selectKey**
    **insert 이후에 생성 된 시퀀스 값 or insert 하기 위해 생성된 시퀀스 값을 가져오는 함수**
    >- `keyProperty`: selectKey구문의 결과를 받는 칼럼 이름. dto에 저장된다.
    >- `keyColumn`: 리턴되는 결과셋의 칼럼명은 프로퍼티에 일치한다. 여러개의 칼럼을 사용한다면 칼럼명의 목록은 콤마를 사용해서 구분한다.
    >- `resultType`: 결과의 타입. 마이바티스는 이 기능을 제거할 수 있지만 추가하는게 문제가 되지는 않을것이다. 마이바티스는 String을 포함하여 키로 사용될 수 있는 간단한 타입을 허용한다.
    >- `order`: BEFORE 또는 AFTER를 셋팅할 수 있다. `<selectKey>`를 포함하는 부모 SQL이 있을 때 실행 순서를 정한다.BEFORE 는 부모SQL문을 실행하기 전 AFTER는 부모 SQL을 실행한 후에 해당 `<selectKey>` 를 실행한다.
    >- `statementType`: STATEMENT, PREPARED 또는 CALLABLE중 하나를 선택할 수 있다. 마이바티스에게 Statement, PreparedStatement 또는 CallableStatement를 사용하게 한다. 디폴트는 PREPARED 이다.

    EX)
    ```sql
    <selectKey resultType="int" keyProperty = "order_seq_num" order ="BEFORE">
        select order_seq_num.nextval from dual
    </selectKey>
    <-- 오라클 시퀀스 번호를 가져온 다음 주문테이블의 각 레코드의 구분 번호로 사용-->
    ```
    EX)실사용
    ```sql
    <insert id="insertNewOrder" parameterType="orderVO" >
        <selectKey resultType ="int" keyProperty = "order_seq_num" order = "BEFORE">
            <select order_seq_num.nextval form dual>s
        </selelctKey>

        insert into t_shopping_order(order_seq_num,
                                    order_id,
                                    ...)
                                values(#{order_seq_num}
                                    #{order_id},
                                    #{member_id},
                                    #{goods_id}
                                    ...)
    </insert>
    ```
    위 코드는 주문 정보를 추가하기 전 현재 시퀀스 값을 가져와서 keyProperty에 저장한다. 






