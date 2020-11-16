DB에서 사용하는 명령어

---
##### 데이터 조작어 : DML (Data Manipulation Language)
    * SELECT : 데이터 조회
    * INSERT, UPDATE, DELETE : 데이터 삽입, 수정, 삭제

##### 데이터 정의어 : DDL (Data Definition Language)
    * CREATE, ALTER, DROP, RENAME, TRUNCATE : 데이터 구조를 정의

##### 데이터 제어어 : DCL (Data Control Language)
    * GRANT, REVOKE : DB에 접근하고 객체들을 사용하도록 권한 부여/회수
    
##### 트랜잭션 제어어 : TCL (Transaction Control Language)
    * COMMIT, ROLLBACK, SAVEPOINT : 논리적인 작업단위를 묶어서 DML에 의해 조작된 결과를 작업단위별로 제어

---
#### 삭제 명령어 심화탐구
 - DELETE : 데이터만 삭제. 용량 그대로. 원하는 데이터 선택 삭제 가능. 복구 가능.
 - TRUNCATE : 데이터 삭제. 용량 초기화. 데이터 전체 삭제. 삭제 후 복구 불가.
 - DROP : 테이블 전체 삭제. 삭제 후 복구 불가.


```$xslt
Delete VS Truncate
Delete는 레코드 단위로 삭제 진행. 그래서 조건도 줄 수 있고, 속도가 상대적으로 느리다.
Truncate는 테이블을 drop 후 create하는 방식. 속도가 빠르나 복구가 불가한 이유. Auto_increment도 초기화됨.
```

