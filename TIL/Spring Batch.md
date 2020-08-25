## Spring Batch

**스프링 배치**    
* DI, AOP, 서비스 추상화 등 Spring 프레임워크의 3대 요소를 모두 사용가능
```
Quarz는 스케줄러의 역할
Batch와 같이 대용량 데이터 배치처리에 대한 기능 지원 X 
Quartz + Batch : 정해진 스케줄마다 Quartz가 Spring Batch를 실행하는 구조
```

* Job : 하나의 배치 작업 단위
    + Job > Step > Tasklet, [Reader, Processor, Writer]


* @EnableBatchProcessing
    + Spring Batch 기능 활성화 어노테이션 (필수 선언)
* @Configuration
    + Spring Batch의 Job 등록
* JobBuilderFactory.get("{job 이름}")
    + Batch Job 생성 / 이름 설정
* StepBuilderFactory.get("{step 이름}")
    + Batch Step 생성 / 이름 설정
    + .tasklet((contribution, chunkContext))
        * Step 안에서 단일로 수행될 커스텀한 기능 선언
        

**Spring Batch에서 메타 데이터 테이블들이 필요**
```
메타 데이터 내용
    - 이전에 실행한 Job정보
    - 최근 실패한 Batch Parameter, 성공한 Job
    - 다시 실행한다면 어디서 부터 시작하면 될지
    - 어떤 Job에 어떤 Step들이 있었고 Step들 중 성공한 Step과 실패한 Step 
```

* BATCH_JOB_INSTANCE
    + 수행한 Batch Job Name
    + Job Parameter(배치가 실행될때 외부에서 받을 수 있는 파라미터)에 따라 생성되는 테이블
    + 동일한 Job Parameter는 여러개 존재할 수 없다.

* BATCH_JOB_EXECUTION
    + JOB_EXECUTION(자식)과 JOB_INSTANCE(부모)는 부모-자식 관계
    + 실행한 Job의 첫 번째 혹은 다음번 시도
... 

------------

* JobParameter
    + 외부/내부에서 파라미터를 받아 여러 Batch컴포넌트에서 사용가능
    + Scope 선언 필요 -> @StepScope, @JobScope
    + 사용가능한 타입 : Double, Long, Date, String
    + 같은 JobParameter로 같은 Job을 두번 실행하지않음
    
* Scope
    + @JobScope는 Step 선언문에서 사용가능
    + @StepScope는 Tasklet이나 ItemReader, ItemWriter, ItemProcessor에서 사용가능
    
-------------

* Chunk
    + 각 커밋 사이에 처리되는 row수
    + chunk 지향 처리 : 한 번에 하나씩 데이터를 읽어 Chunk라는 덩어리를 만든 뒤, Chunk단위로 트랜잭션을 다루는 것
    + 실패 롤백 단위 또한 chunk size만큼.

* Tasklet
    + ItemReader & ItemWriter & ItemProcessor 묶음
    
* ItemReader 
    + 데이터를 읽어들임
    + DB, File, XML, JSON 등 다른 데이터 소슬르 배치 처리의 입력으로 사용할 수 있음
    + 가장 대표적인 구현체는 JdbcPagingItemReader
    + ```
        JdbcCursorItemReader (Jpa에는 없음)
            * chunk 
                - <{Reader에서 반환할 타입}, {Writer에 파라미터로 넘어올 타입}>
                - chunkSize : 트랜잭션 범위
            
            * fetchSize
                - database에서 한번에 가져올 데이터 양
                - paging은 실제 쿼리를 limit, offset을 이용해 분할처리
                - cursor는 fetchSize만큼 가져와 read()를 통해서 하나씩 가져옴
            
            * dataSource
                - database에 접근하기 위해 사용할 datasource객체 할당
      
            * rowMapper
                - 쿼리 결과를 Java 인스턴스로 매핑하기 위한 Mapper
                - 커스텀하게도 가능하나, 보편적으로 VeanPropertyRowMapper.class사용
      
            * sql
                - Reader로 사용할 쿼리문
        
            * name
                - reader의 이름 지정
                - Bean이름 아님. Spring Batch의 ExecutionContext에서 저장되어질 이름
      
    ```

  * JpaPagingItemReader
    + order정렬 필수
    + fetchSize = chunkSize 유지

  * ItemWriter
    + Spring Batch에서 사용하는 출력기능
    + Chunk 단위로 묶인 item List를 다룸
    + JPA, Hibernate의 경우 ItemWriter 구현체에서 flush(), session.clear()가 따라옴
  
  * JpaItemWriter
    + JPA를 사용하며 영속성 관리를 위해 EntityManager를 할당해야한다
    + processor를 통해 Reader에서 읽은 데이터를 가공하여 Entity클래스 전달
  
  * ItemProcessor
    + 필수가 아님
    + 비즈니스 코드가 섞이는 것을 방지
    + 각 계층 (읽기/처리/쓰기)를 분리하기 좋은 방법
    + Reader에서 넘겨준 데이터 개별건을 가공/처리
    + 크게 변환, 필터로 사용. (null을 반환하면 writer에 전달되지않음)
    + ItemProcessor<{Reader에서 받을 타입}, {Writer에 보낼 타입}>. chunkSize 앞에 선언될 타입도 동일.
    + 코드 양이 많아지면 별도 클래스로 Processor를 분리하기도함
    
  

  
[참고]
https://jojoldu.tistory.com        