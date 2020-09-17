Lock

* 낙관적 락
    ```
    * 충돌이 발생하지 않는다고 낙관적으로 가정하는 방법
    * JPA가 제공하는 버전 관리 기능 (플리케이션이 제공하는 락)
    * 트랜잭션을 커밋하기 전까지 충돌을 알 수 없다 (커밋 시 버전 체크)
    * @Version 어노테이션 Entity에 사용      
    ```
    
* 비관적 락
    ```
    * 충돌이 발생한다고 가정하고 우선 락을 거는 방법
    * DB가 제공하는 락 (대표적으로 select for update 구문)
    * 락을 획득할 때까지 트랜잭션이 대기
    ```
  
* JPA가 제공하는 락 옵션
    * PESSIMISTIC WRITE : 데이터베이스에 쓰기 락을 걸때 사용
    * PESSIMISTIC_READ : 데이터를 반복 읽기만 하고 수정하지 않는 용도로 락을 걸 때 사용
    * PESSIMISTIC_FORCE_INCREMENT : 버전 정보를 강제로 증가. 비관적 락중 유일하게 버전 정보를 사용.
    * OPTIMISTIC : 낙관적 락
    * OPTIMISTIC_FORCE_INCREMENT : 버전 정보를 강제로 증가.