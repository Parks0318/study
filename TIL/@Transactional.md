##@Transactional 
    
#### Isolation : 격리수준 옵션. 트랜젝션에서 일관성이 없는 데이터를 허용하도록 하는 수준
```    
    1. READ_UNCOMMITTED (level 0)
        : 트랜잭션에 처리중인 / 아직 커밋되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용
            * Dirty read : 트랜잭션에서 처리하는 작업이 완료되지 않았는데도 다른 트랜잭션에서 볼 수 있는 현상. 해당 옵션에서만 발생.
    
    2. READ_COMMITTED (level 1) - oracle 기본 설정
        :  트랜잭션이 커밋되어 확정된 데이터만을 읽는 것을 허용. 
        
    3. REPEATABLE_READ (level 2) - MySQL InnoDB 기본 설정
        : 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock이 걸려서 다른 사용자는 그 영역에 해당되는 데이터에 대한 수정이 불가능함
          선행 트랜잭션이 읽은 데이터는 트랜잭션이 종료될 때까지 후행 트랜잭션이 갱신하거나 삭제하는 것을 불허함으로써 같은 데이터를 두번 쿼리했을 때 일관성 있는 결과를 리턴함
          
    4. SERIALIZABLE (level 3)
        : 완벽한 읽기 일관성 모드를 제공
          트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock이 걸리므로 다른 사용자는 그 영역에 해당되는 데이터에 대한 수정 및 입력이 불가능하다.
```          
 
#### Propagation : 전파옵션. 트랜잭션 동작 도중 다른 트랜잭션을 호출(실행)하는 상황에 선택할 수 있는 옵션
```
* REQUIRED (기본값) : 부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 새로운 트랜잭션을 생성. 
* REQUIRES_NEW : 부모 트랜잭션을 무시하고 무조건 새로운 트랜잭션이 생성되도록 한다.
* SUPPORT : 부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 nontransactionally로 실행된다.
* MANDATORY : 부모 트랜잭션 내에서 실행되며 부모 트랜잭션이 없을 경우 예외가 발생된다.
* NOT_SUPPORT : nontransactionally로 실행하며 부모 트랜잭션 내에서 실행될 경우 일시 정지 된다.
* NEVER : nontransactionally로 실행되며 부모 트랜잭션이 존재한다면 예외가 발생한다.
* NESTED : 해당 메서드가 부모 트랜잭션에서 진행될 경우 별개로 커밋되거나 롤백될 수 있다. 둘러싼 트랜잭션이 없을 경우 REQUIRED와 동일하게 작동한다.

```                         
                         
    
------    
###Transaction (줄여서 TX)

```시작과 끝이 있는 독립적인 일 여러개를 하나로 묶어놓고 그 중 어느 하나라도 실패하면 모든 일들을 시작하기 전 상태로 돌리는 하나의 작업 단위.```

####트랜잭션 성질 4가지
```
* 원자성(Atomicity)
    : 한 트랜잭션 내에서 실행한 작업들은 하나로 간주한다. 즉, 모두 성공 또는 모두 실패. 

* 일관성(Consistency)
    : 트랜잭션은 일관성 있는 데이타베이스 상태를 유지한다.

* 격리성(Isolation)
    : 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않도록 격리해야한다.

* 지속성(Durability)
    : 트랜잭션을 성공적으로 마치면 결과가 항상 저장되어야 한다.
```
    
-----

####ReadOnly 설정 적용은 어떻게 되는걸까
```
jdbc 커넥션 객체에는 Connection.setReadOnly(true|false)가 이미 존재. **

   @Transactional(readOnly=true|false)하면 Spring의 설정이 연쇄적으로 커넥션 객체의 setReadOnly메소드를 호출하는것.
   Spring LazyConnectionDataSourceProxy + AbstractRoutindDataSource
      AbstractRoutingDataSource : 여러개의 데이터소스를 하나로 묶고 자동으로 분기처리를 해줌

      TransactionSynchronizationManager가 현재 선언된 쓰레드의 트랜잭션 상태를 읽어오는게 가능하지만, 동기화 시점과 connection객체를 가져오는 시점에 문제가 있다
      Spring은 @Transactional을 만나면 
         TransactionManager 선별 -> DataSource에서 Connection 획득 -> Transaction 동기화 (Synchronization)
      순서로 동작하게되는데 동기화를 하고 커넥션을 받아와야하기 때문에 순서가 뒤바뀌어있다.

      LazyConnectionDataSourceProxy는 쿼리 실행여부와 상관없이 트랜잭션이 걸리면 무조건 Connection 객체를 확보하는 단점을 보완하여 트랜잭션 시작 시에 Connection Proxy객체를 리턴하고 실제로 쿼리가 발생할 때 데이터 소스에서 getConnection()을 호출한다.
      그러면, 
         TransactionManager 선별 -> Lazy-에서 ConnectionProxy 객체획득 -> Transaction 동기화 -> 실제 쿼리시 ReplicationRoutingDataSource.getConnection()/determinCurrentLookupKey()호출
```