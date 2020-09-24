Spring Cloud Gateway

용어
 - Route : ID, URI, predicate의 조합으로 정의
 - Predicate : Http Request에서 header나 parameter 같은 일치하는 항목을 찾을 수 있는 Java8 함수
 - Filter : 요청을 보내기 전/후에 request/response를 수정할 수 있는 필터


**동작방식**

```
    클라이언트가 SCG로 요청을 보낸다
    Gateway Handler Mapping에서 요청이 라우트와 일치한다고 판단한다  
    Gateway Web Handler로 전송한다
    필터 체인을 통해 요청을 실행한다 (pre, post 논리 수행가능)
```

