# Clean Code - Robert C. Martin : PART 11

### PART 11 : 시스템

1. 소프트웨어 제작도 도시를 세우는 것처럼
    1. 적절한 추상화와 모듈화는 도시를 원활하게 돌아가게 하는 중요한 요인이다. 소프트웨어도 마찬가지이다. 
    2. 큰 그림은 이해하지 못할지라도 개인과 개인이 관리하는 '구성요소'가 효율적으로 돌아가기에 전체가 완성된다.
    따라서 높은 추상화 수준을 통해 시스템 수준에서 깨끗함을 유지하는 방법을 살펴본다
2. 시스템 제작과 시스템 사용을 분리하라
    1. 시작 단계 (준비 과정) 을 분리해야 한다.
        1. 본문에서는 **초기화 지연 Lazy Initialization (계산 지연 Lazy Evaluation)**을 예시로 들었다.
            
            ```java
            public Service getService(){
            	if ( service == null ) {
            		service = new MyServiceImpl(...); // 모든 상황에 적합한 기본값일까?
            	return service;
            }
            ```
            
            위 초기화 지연을 통해 장점으로 두가지를 제시했다. 
            
            - 실제로 필요할 때까지 객체를 생성하지 않아 불필요한 부하가 걸리지 않는다는 점, 이를 통해 애플리케이션의 start-up 시간이 그만큼 빨라진다는 것
            - 어떤 경우에도 `NPE` 를 반환하지 않는다는 것
            
            하지만 이에 대한 단점도 존재한다.
            
            - `getService()` 메소드가 `MyServiceImpl`의 생성자 인수에 명시적으로 의존한다. 런타임 로직에서 `MyServiceImpl` 객체를 전혀 사용하지 않더라도, 의존성을 해결하지 않으면 (객체를 주입받지 않으면) 컴파일이 안된다.
            - 테스트를 진행하는 경우 만약 삽입되는 객체 `MyServiceImpl` 가 무겁다면 단위 테스트에서 service 필드에 테스트 전용 객체(`Mock Object`) 나 다른 방법을 마련해야한다. 객체가 무거운 경우 테스트 속도가 저하되기 대문이다.
                
                또한 런타임 로직임에도 불구하고 객체 생성 로직을 섞어놓은 탓에 모든 실행 경로 (service가 null 인 경우 혹은 아닌 경로) 를 테스트해야한다. 이거 메소드가 두 가지 작업을 수행한다는 의미이며 이는 작게나마 `단일 책임 원칙(Single Responsibility Principle, SRP)`를 깨는 위험성이 있다.
                
        2. Main 분리
            
            시스템 생성과 시스템 사용을 분리하는 방법이다. 생성과 관련된 코드는 main 혹은 main이 호출하는 모듈로 옮기고, 나머지 시스템은 모든 객체가 생성되었고 모든 의존성이 연결되었다고 가정하는 것이다.
            
            이 방법을 사용하는 경우 main 함수에서 시스템에 필요한 객체를 생성한 뒤 애플리케이션에 넘긴다. 그렇다면 애플리케이션은 위의 경우처럼 생성 여부를 조건문으로 구분할 필요가 없다. 그저 전달된 객체를 사용하면 그만이다.
            
            ![Untitled](Clean%20Code%20-%20Robert%20C%20Martin%20PART%2011%202d3fb72ccf96402aaaa79b32ce0aca85/Untitled.png)
            
            위 이미지에서 화살표는 의존성의 방향을 뜻한다. 즉 애플리케이션은 main에서 생성되는 객체의 생성에 전혀 관여하지 않은 채 객체가 적절하게 생성되었다고 가정할 뿐이다.
            
        3. 팩토리
            
            하지만 모든 경우에서 main에서 생성된 객체를 넣는 것도 위험성이 존재한다. 객체가 생성되는 시점을 애플리케이션이 결정할 필요가 있을 경우다. 
            
            ![Untitled](Clean%20Code%20-%20Robert%20C%20Martin%20PART%2011%202d3fb72ccf96402aaaa79b32ce0aca85/Untitled%201.png)
            
            애플리케이션은 LineItem 인스턴스를 생서아형 Order에 추가해야한다.  이 경우에는 `ABSTRACT FACTORY (추상 팩토리)` 패턴을 사용한다. 
            
            `ABSTRACT FACTORY (추상 팩토리)` : 팩토리 메서드를 한번더 캡슐화한 방식, 관련이 있는 객체들을 통째로 묶어서 팩토리 클래스로 만들고 이들 팩토리를 조건에 따라 생성하도록 다시 팩토리를 만들어서 객체를 생성한다.
            
            즉, LineItem을 생성 (`MakeLineItem`)하는 시점은 애플리케이션이 결정하지만 생성하는 코드는 모른다. 인스턴스를 생성하는 방법은 `LineItemFactoryImplemetaion`이 알고 있고, 애플리케이션은 모른 채로 인스턴스가 생성되는 시점을 완벽하게 통과하게 되는 것이다. 이를 통해 `OrderProcessing` 애플리케이션은 `LineItem` 인스턴스가 생성되는 시점을 완벽하게 통제할 수 있게 된다.
            
        4. 의존성 주입 (Dependency Injection)
            
            사용과 제작을 완벽하게 분리하는 강력한 메커니즘 중 하나다. `Inversion of Control, IOC` 기법을 의존성 관리를 적용하여 한 객체가 맡은 보조 책임을 새로운 객체에게 전적으로 떠넘긴다.  새로운 객체는 넘겨받은 책임만 맡으므로 `단일 책임 원칙, Single Responsibility Principle (SRP)` 을 지키게 된다. 
            
            의존성 관리 맥락에서 객체는 의존성 자체를 인스턴스로 만드는 책임은 지지않고, 이 책임을 다른 '전담' 메커니즘에 넘기는 것을 통해 제어를 역전한다. 초기 설정은 시스템 설정에서 필요하므로 대개 '책임지는 전담 메커니즘'은 main 루틴이나 특수 컨테이너를 사용하여 처리한다.
            
            1. JNDI 검색
                
                의존성 주입을 '부분적으로' 구현한 기능이다. 객체는 디렉터리 서버에 이름을 제공하고 그 이름에 일치하는 서비스를 요청한다. 
                
                ```java
                MyService myService = (MyService)(jndiContext.lookup("NameOfMyService"));
                ```
                
                호출하는 객체는  실제로 반환되는 객체의 여행을 제어하지 않고, 의존성을 능동적으로 해결한다. 
                
3. 확장
    
    **이 파트의 경우 내용 이해가 어렵다. 추후에 다시 읽어보자. 시스템 수준에 대해서 관심사?를 적절하게 분리하는**