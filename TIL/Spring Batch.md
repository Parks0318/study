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





[참고]
https://jojoldu.tistory.com        