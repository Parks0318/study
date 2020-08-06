## DB 트랜잭션 실행 방법
   ``` 
   START TRANSACTION; <- 트랜잭션 시작
        이동안 테이블의 일부 기능에 lock이 걸리고, 다른 세션에서 INSERT등의 명령 실행불가
   COMMIT; or ROLLBACK; <- 트랜잭션 종료
```

#### 자동 커밋기능(AUTOCOMMIT) : 
    * 일반적으로 MySQL에서 명령을 실행하면 사용자가 의식하지않아도 모든 명령이 자동으로 COMMIT; 되는 것
    * MySQL 5.1까지는 기본 엔진이었던 MYISAM에서 트랜잭션 기능이 없어서 모든 명령이 반영되었다.
    * 자동 커밋 기능 현재상태 확인 -> SELECT @@autocommit; (on = 1, off = 0)

#### 트랜잭션을 이용할 수 없는 범위
```
      DROP ** (database, table,..)
      ALTER table
```