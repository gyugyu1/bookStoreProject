# 629commit

 
## **1.캐싱**

- ### 캐싱이란?  
캐시(Cache)라고 하는 좀 **더 빠른 메모리** 영역으로 데이터를 가져와서 접근하는 방식을 말한다.
<img src ="image\chche.png" width = "400"/>

위 그림에서처럼 서버가 브라우저에게 HTTP HEADER를 보냄.  
HEADER에서 캐싱에 관련된 지시가 이루어진다.  
컨텐츠 타입, 길이, 캐시 전략, 유효성 토큰 정보를 내려줌.  
위 사진에서는 1024바이트의 응답, 클라이언트에게 120초 동안 유지 지시, 유효 토큰(X234DFF)를 내려준다.

>- ETag란?  
ETag를 HTTP 헤더에 담아 유효 토큰으로 통신.  
 >>**Etag 사용x 시**  
브라우저에서 120초 후 동일한 요청을 재요청 -> 로컬 캐시를 확인 하였으나 그 데이터는 만료 되었으므로 사용 불가 -> 서버에 재요청 -> 불필요한 리소스 다운로드 발생  
>>**Etag 사용 시**  
서버는 중재 토큰 생성 ( 파일에 대한 해시 값 또는 파일에 대한 식별자 정보 포함)  
클라이언트가 보낸 토큰을 받고 서버에서 비교 -> 같으면 다운로드 x , 다르면 재 다운로드


- ## Cache-Control
누가 응답을 어떤 조건에서, 얼마나 캐시할 수 있는지 설정.  
http header를 통해서 정의.  
1. no-cache  
매번 Etag토큰을 체크한다!
원래는 로컬에 캐시가 있을 시에는 서버와 통신이 발생하지 않지만, no-chche로 설정을 하면 토큰 대조를 위해 통신이 발생한다.   
하지만 토큰이 일치하면 다운로드를 하지 않으므로 불필요한 다운로드는 막을 수있다. (즉, 캐시의 max-age가 0 라는 의미)

2. no-store  
반환된 응답을 브라우저에게 어떠한 형태로든 캐시를 하지 말라는 설정.  
은행 데이터와 같이 민감한 정보를 응답 할 때 사용됨
no-chche는 캐시는 저장하지만 사용 할 때마다 서버에 재검증 요청을 해야하지만  
no-store는 캐시 자체를 만들지 않다는 점에서 no-cache와 차이.













