Spring Cloud Gateway

**용어**

 - Route(경로) : 게이트웨이의 기본 골격. ID, 목적지 URI, 조건부(predicate)의 집합으로 구성
 - Predicate(조건부) : header나 parameter 같은 Http 요청의 일치하는 항목을 찾을 수 있는 Java8 함수
 - Filter(필터) : 요청을 보내기 전/후에 request/response를 수정할 수 있는 필터


**동작방식**

```
    클라이언트가 SCG로 요청을 보낸다
    Gateway Handler Mapping에서 요청이 라우트와 일치한다고 판단한다  
    Gateway Web Handler로 전송한다
    필터 체인을 통해 요청을 실행한다 (pre, post 논리 수행가능)
```

설정
```
spring:
  cloud:
    gateway:
      default-filters:
        - name: GlobalFilter
          args:
            baseMessage: GlobalFilter Message
            preLogger: true
            postLogger: true
      routes:
        - id: {name}
          uri: http://localhost:8080/ -- 어디로 보낼지
          predicates:
            - Path=/uri/** -- 어떤 경로로 들어온 요청을

```

