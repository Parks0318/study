### UML (Unified Modeling Language)
: 통합 모델링 언어, 시스템을 모델로 표현해주는 대표적인 모델링 언어

* UML이 필요한 순간
    - 다른 사람들과의 의사소통 또는 설계 논의
    - 전체 시스템의 구조 및 클래스의 의존성 파악
    - 유지보수를 위한 설계의 back-end 문서


* UML의 요소
    - Thing : 추상적인 개념으로써 UML을 이용한 모델링의 기본요소
        > 클래스, 노드, 서버, 사용자, 행위요소, 패키지, 라이브러리, 주석, 메모
    - Relationships : 구성 요소들간의 연관성을 표현
        > 의존관계, 일반화관계, 연관관계, 집합관계, 구성관계, 실현관계
    - Diagram 
        > 구조 다이어그램, 행위 다이어그램
    

---

#### Diagram 
``` 
구조 다이어그램 (Structure UML Diagrams)
    * 클래스 다이어그램 (Class) : 클래스 명세와 클래스 간의 관계를 표현
    * 객체 다이어그램 (Object) : 인스턴스 간의 연관 관계를 표현
    * 패키지 다이어그램 (Package) : 패키지 간의 연관 관계를 표현
    * 컴포넌트 다이어그램 (Component) : 파일과 데이터베이스, 프로세스와 스레드 등의 소프트웨어 구조를 표현
    * 복합 구조 다이어그램 (Composite Structure) : 전체-부분 구조를 가진 클래스를 실행할 때의 구조를 표현
    * 배치 다이어그램 (Deployment) : 하드웨어와 네트워크 등의 시스템의 물리 구조를 표현
    * 프로필 다이어그램 (Profile) : 프로필만 간단하게 구성된 다이어그램

행위 다이어그램 (Behavioral UML Diagrams)
    * 유즈케이스 다이어그램 (UseCase) : 시스템이 제공하는 기능과 이용자의 관계를 표현
    * 액티비티 다이어그램 (Activity) : 일련의 처리에 있어 제어의 흐름을 표현
    * 시퀀스 다이어그램 (Sequence) : 인스턴스 간의 상호 작용을 시계열로 표현
    * 커뮤니케이션 다이어그램 (Cummunication) : 인스턴스 간의 상호작용을 구조중심으로 표현
    * 상호작용 개요 다이어그램 (Interaction Overview) : 조건에 따라 다르게 동작하는 시퀀스
    * 타이밍 다이어그램 (Timing) : 인스턴스 간의 상태 전이와 상호작용을 시간제약으로 표현
    * 상태 머신 다이어그램 (State Machine) : 인스턴스의 상태 변화를 표현
```

* 클래스 다이어그램 
    
    : 클래스의 구성요소 및 클래스 간의 관계를 표현. 시스템의 일부 혹은 전체 구조를 나타낼 수 있음
    
    * 용도 
        1. 문제 해결을 위한 도메인 구조를 나타내어 보이지않는 도메인 안의 개념과 같은 추상적인 개념을 기술
        2. 소프트웨어의 설계 혹은 완성된 소프트웨어의 구현 설명을 목적으로 사용할 수 있
    
    * 기본요소
        * 윗부분 : 클래스 이름
        * 중간 부분 : 속성
            > * 속성(Attribute)
            > * 접근제어자 이름: 타입 = 기본값
            > * ex) -title: String = ""
        * 마지막 부분 : 메서드 (경우에 따라 생략가능)
            > * 접근제어자 이름(파라미터 속성): 리턴값
            > * ex_1) +setTitle(String)
            > * ex_2) +getTitle(): String
        * 접근제어자
            * \+ : public
            * \- : private
            * \# : protected
    * 관계표현
        * 일반화 : 상속. 실선에 비어있는 화살표
        * 실체화 : 인터페이스에 있는 spec을 오버라이딩 하여 실제로 구현하는 것. 점선과 비어있는 화살표
        * 의존 : 클래스간 참조가 일어나는 것 중 하나. 메서드 내에서 대상클래스의 객체를 생성하거나 인자로 받아 사용하는 것. 점선과 화살표
        * 연관 : 다른 객체의 참조를 가지는 필드. 실선.
        * 집합 : Collection이나 Array를 이용하는 관계. 연관으로도 표현가능...해서 모호함의 논란이 있음. 실선에 빈 다이아몬드
        * 합성 : 클래스의 연관관계에서 강한 결합의 관계를 의미. 참조하는 클래스의 라이프 사이클이 종속적이라는 말. 실선에 채워져있는 다이아몬



* 시퀀스 다이어그램

    : 어떠한 순서로 어떤 객체들과 어떻게 상호작용했는지를 표현하는 다이어그램. api등의 유즈케이스를 디테일하게 알 수 있음. 시나리오를 파악하기 좋음
    
    * LifeLine 
        * 모델링 되는 개개의 인스턴스를 나타냄. 네모박스와 점선으로 이루어져있음. 네모박스는 Object관점-class / Service관점-Component
        * 점선이 아래로 내려올수록 시간이 경과됨을 나타낸다
    
    * Activations
        * 점선을 따라 세로로 이루어진 네모박스
        * LifeLine의 인스턴스가 실제로 다른 인스턴스와 상호작용을하며 활성화되어있는것을 나타냄
    
    * message
        * 실제로 인스턴스간의 주고받는 데이터를 나타냄
        * 일반적으로 request / response로 구성
            1. sync : 요청(실선 가득찬 화살표)과 응답(점선 골격 화살표)이 존재
            2. async : 요청(점선 골격 화살표)만 존재
            3. self : 인스턴스간의 상호작용 없이 하나의 인스턴스에서 처리하면, 본인 lifeline으로 재귀 화살표 사용
            
    * 흐름제어
        * guard : 단일 메세지에 대해서 조건을 명시할 수 있음
            > text 앞쪽에 []로 감싼 후 조건 명시
        * sequence fragments
            1. alt (alternatives) - if / else 구문
            2. opt (options) - if 구문
            3. loop - for 또는 while 구문


MS UML 템플릿 다운로드 주소

``` https://support.microsoft.com/ko-kr/office/%ec%a3%bc%ec%9a%94-visio-%ed%85%9c%ed%94%8c%eb%a6%bf-%eb%b0%8f-%eb%8b%a4%ec%9d%b4%ec%96%b4%ea%b7%b8%eb%9e%a8-27d4274b-5fc2-4f5c-8190-35ff1db34aa5?ui=ko-kr&rs=ko-kr&ad=kr#bkmk_basicumlsequence```



참조
- [https://velog.io/@hanblueblue/UML-UML-%EA%B8%B0%EC%B4%88][UML 기초]

[UML 기초]: https://velog.io/@hanblueblue/UML-UML-%EA%B8%B0%EC%B4%88

- [https://sabarada.tistory.com/84?category=800100][시퀀스 다이어그램] 

[시퀀스 다이어그램]: https://sabarada.tistory.com/84?category=800100

- [https://sabarada.tistory.com/72][클래스 다이어그램]

[클래스 다이어그램]: https://sabarada.tistory.com/72