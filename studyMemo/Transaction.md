# Transaction ( 트랜잭션 )
작업의 한 단위
- 원자성(Atomicity)
  - 한 트랜잭션 내에서 실행한 작업은 하나로 간주, 전부 실패하거나 전부 성공하거나
- 일관성(Consistency)
  - 트랜잭션은 일관성 있는 데이터베이스 상태를 유지 함
- 격리성(Isolation)
  - 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않게 한다
- 지속성(Durability)
  - 트랜잭션을 성공적으로 마치면 결과가 항상 저장되어야 한다.


## 스프링에서의 트랜잭션
---
- 어노테이션 방식   
  1. 클래스나 메소드 위에 @Transactional을 선언 
  2. 트랜잭션 기능이 적용된 프록시 객체 생성
  3. 이 프록시 객체는 @Transactional이 포함된 메소드가 호출 될 시, PlatformTransactionManager를 사용하여 트랜잭션을 시작

ex)
```java
@Service
public class TestService {
       ...
       ...
 
 
@Transactional
public int insertLib(TestVO testVO){
    ...
    int result = testService.updateBook(testVO);   --첫번째 접근
 
    if(result > 0){
        testService.insertPoint(testVO); -- 두번째 접근
    }
    
    testDao.insertLib(testVO); -- 세번째 접근
 
    return 1;
}
 
}
```

해당 메소드 도중 에러가 나면 기존에 처리했던 메소드를 모두 롤백한다.