# Clean Code - Robert C. Martin : PART 13

### PART 13 : 동시성

1. 동시성이 필요한 이유?
    1. **무엇 What** 과 **언제 When** 을 분리해라 (Decoupling, 결합 제거)
    2. 동시성에 대한 오해
        1. 동시성은 항상 성능을 높힌다 : 대기 시간이 아주 길어 여러 스레드가 프로세서를 공유할 수 있거나, 여러 프로세서가 동시에 처리할 독립적인 계산이 충분히 많은 경우에만 성능이 높아진다.
        2. 동시성을 구현해도 설계는 변하지 않는다 : 단일, 다중 스레드 시스템은 설계가 판이하게 다르다.
        3. 웹 또는 EJB 컨테이너를 사용하면 동시성을 이해할 필요가 없다 : 컨테이너가 어떻게 동작하는지, 어떻게 동시수정, 데드락과 같은 문제를 피할 수 있는지 알아야 한다.
    3. 동시성에 대한 올바른 견해
        1. 동시성은 부하를 유발하며, 복잡하다.
        2. 동시성 버그는 재현하기 힘들다.
        3. 동시성을 구현하려면 근본적인 설계 전략을 재고해야 한다.
2. 동시성 방어 원칙
    1. 단일 책임 원칙, SRP
    2. 자료 범위를 제한하라
    3. 자료 사본을 사용하라
    4. 스레드는 가능한 독립적으로 구현하라
3. 라이브러리를 이해하라
    1. 다중 스레드 환경에서 안전하고 선응이 좋은 클래스를 확인하고 사용하라
4. 실행 모델을 이해하라
    1. 기본 용어
        1. 한정된 자원 (Bound Resource) : 크기나 숫자가 제한적이다. ex) 데이터베이스 커넥션, 읽기 쓰기 버퍼
        2. 상호 배제 (Mutual Exclusion) : 한번에 한 스레드만 공유 자료, 공유 자원을 사용할 수 있는 경우
        3. 기아 (Starvation) : 한 스레드나 여러 스레드가 굉장히 오랫동악 혹은 영원히 자원을 기다리는 경우
        4. 데드락 (Deadlock) : 여러 스레드가 서로 끝나기를 기다리며 무한히 서로 기다려 어느쪽도 진행하지 못하는 상황
        5. 라이브락 (Livelock) : 락을 거는 단계에서 각 스레드가 서로 방해하는 상황
    2. 실행 모델
        1. 생산자-소비자 (Producer-Consumer)
            
            생산자 스레드가 정보를 생성해 buffer, queue 에 넣고, 하나 이상의 소비자 스레드가 대기열에서 정보를 가져와 사용한다. 대기열은 한정된 자원이며, 시그널을 통해 자원을 공유한다.
            
        2. 읽기-쓰기 (Readers-Writers)
            
            읽기 스레드를 위한 주된 정보원으로 공유 자원을 사용하고, 쓰기 스레드가 이 공유 자원을 이따금 갱신한다. 이 경우는 버퍼의 갱신을 균형적이고 효과적으로 진행할 수 있는 해법이 필요하다.
            
        3. 식사하는 철학자들 (Dining Philosophers)
            
            둥근 식탁에 여러 명의 철학자(스레드)가 있고 커다란 스파게티 한 접시 (스레드+자원=작업)가 있다. 철학자의 왼쪽에만 포크가 놓여있으며 양손으로만 스파게티를 먹을 수 있다. 다른 철학자(스레드)가 포크(자원)를 사용하는 중이라면 포크를 내려놓을 때까지 스파게티를 먹지 못한다 (작업 수행 불가). 기업 애플리케이션은 여러 프로세스가 자원을 얻으려 경쟁하기 때문에 주의해서 설계해야한다.
            
    3. 동기화하는 메소드 사이에 존재하는 의존성을 이해하라
        1. 공유 객체 하나에는 메서드 하나만 사용하라. 
        2. 공유 객체 하나에 여러 메서드가 필요한 상황엔 세 가지 방법을 고려할 수 있다. 
            1. 클라이언트에서 잠금 : 클라이언트에서 첫 번째 메서드를 호출하기 전에 서버를 잠근다. 마지막 메서드를 호출할 때까지 잠금을 유지한다.
            2. 서버에서 잠금 : 서버에다 "서버를 잠그고 모든ㄷ 메서드를 호출한 후 잠금을 해제하는 메소드를 구현한 뒤, 클라이언트에서 이 메소드를 호출한다.
            3. 연결 서버 : 잠금을 수행하는 중간 단계를 수행한다. 
    4. 동기화하는 부분을 작게 만들어라
        1. 락으로 감싼 모든 코드 영역은 한번에 한 스레드만 실행 가능하다. 따라서 스레드를 지연시키거나 부하를 가중시키지 않기 위해 `synchronized` 문을 남발하지 마라.
        2. 임계 영역(critical section, 동시 사용을 막아야만 프로그램이 올바로 동작하는 보호받는 코드 영역)은 반드시 보호해야 한다.
    5. 올바른 종료 코드는 구현하기 어렵다
        1. 종료 코드를 개발 초기부터 고민하고 동작하게 초기부터 구현하라. 생각보다 오래 걸린다. 생각보다 어려우므로 이미 나온 알고리즘을 검토하라.
    6. 스레드 코드 테스트하기
        1. 문제를 노출하는 테스트 케이스를 작성하라. 프로그램 설정과 시스템 설정과 부하를 바꿔가며 자주 돌려라. 테스트가 실패하면 원인을 추적하라. 다시 돌렸더니 통과하더라는 이유로 그냥 넘어가면 절대로 안 된다.
    7. 결론
        1. 다중 스레드 코드는 올바로 구현하기 어렵다. 따라서 무엇보다 `SRP`를 준수한다. POJO 를 사용해 스레드를 아는 코드와 모르는 코드로 분리하고 스레드 코드를 테스트하는 경우는 스레드만 테스트한다. 
        2. 동시성 오류를 일으키는 잠정적인 원인을 철저히 이해한다.