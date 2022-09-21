# Clean Code - Robert C. Martin : PART 10

### PART 10 : 클래스

코드의 표현력과 그 코드로 이루어진 함수에 아무리 신경 쓸지라도 좀더 차원높은 단계까지 신경쓰지 않으면 깨끗한 코드를 얻기는 어렵다.

1. 클래스 체계
    1. 선언 순서
        1. 변수 목록 → `static public` 상수가 있다면 맨 처음으로 나온다.
        2. 비공개 인스턴스 변수. 공개 변수가 필요한 경우는 거의 없다
        3. 공개 함수가 나오며, 비공개 함수는 자신을 호출하는 공개 함수 직후에 넣는다.
    2. 캡슐화
        1. 변수와 유틸리티 함수는 가능한 공개하지 않는 편이 낫지만 반드시 숨겨야 한다는 법칙도 없다.
        2. 캡슐화를 풀어주는 결정은 언제나 최후의 수단이다.
2. 클래스는 작아야 한다.
    1. 첫번 째 규칙은 크기다. 클래스는 작아야 하며, **'작게'** 가 기본이다. 
    2. 얼마나 작아야 하냐고 묻는다면? **책임**이 작게.
    3. 단일 책임 원칙 (Single Responsibility Principle, SRP)
        1. 클래스나 모듈을 변경할 이유는 단 하나뿐이어야 한다. 하나만 책임져야 한다.
        2. '돌아가는 소프트웨어'보다 '깨끗하고 체계적인 소프트웨어' 에 초점을 맞춰라.
        3. 규모가 어느 정도 수준에 이르는 시스템은 논리가 많고 복잡하다. 이런 복잡성을 다루기 위해서는 쳬계적인 정리가 필수다. 그래야 개발자가 무엇이 어디에 있는지 쉽게 찾을 수 있다. 
        4. 변경을 가할때 직접 영향이 미치는 클래스만 이해해도 충분할 정도로 좋은 체계를 가져야 한다.
        5. 큰 클래스 몇 개가 아닌, 작은 클래스 여럿으로 이뤄진 시스템이 바람직하다.
        6. 작은 클래스는 각자 맡은 책임이 하나며, 변경할 이유도 하나다. 또한 다른 작은 클래스와 협력해 시스템에 필요한 동작을 수행한다.
3. 응집도 (Cohesion)
    1. 클래스는 인스턴스 변수 수가 작아야 한다.
    2. 각 클래스 메서드는 클래스 인스턴스 변수를 하나 이상 사용해야 한다. 
        1. 메소드가 변수를 더 많이 사용할수록 응집도는 높아지지만, 이렇게 응집도가 높은 클래스는 가능하지도 바람직하지도 않다.
        2. 하지만 우리는 응집도가 높은 클래스를 선호한다. 그 이유는 응집도가 높다는 것은 클래스에 속한 메소드와 변수가 서로 의존하며 논리적인 단위로 묶이기 때문이다.
        3. '함수를 작게, 매개변수 목록을 짧게' → 이 전략을 따르다 보면 때때로 몇몇 메소드만 사용하는 인스턴스 변수가 아주 많아지게 된다. (일부에서만 필요한 인스턴스 변수를 선언하게 된다는 뜻) 이는 새로운 클래스로 쪼개야 한다는 신호다.
            
            응집도가 높아지지 않도록 적절하게 분리해줘야 한다.
            
    3. 응집도를 유지하면 작은 클래스 여럿이 나온다.
        1. 큰 함수 중 일부를 쪼개는 경우, 만약 일부가 큰 함수에 정의된 변수 일부를 사용한다면 변수를 함수의 인수로 옮기는 것은 옳지 못하다.
            
            그렇다면 이 변수(함수 내부에 정의된 변수)를 클래스 인스턴스 변수로 승격시켜야 할까? 이 경우는 새 함수는 인수가 필요없어지게 되고 그만큼 함수를 쪼개기 쉬워진다.
            
            하지만 이렇게 하는 경우는 클래스에 몇몇 함수만 사용하는 인스턴스 변수가 점점 늘어나기 때문에 응집력을 잃는다. 그렇다면 몇몇 함수가 몇몇 변수를 사용하니 이 부분을 독자적인 클래스로 분리할 수 있는 기회를 얻는다. 
            
            결국 클래스가 응집력을 잃는다면 쪼개는 것이 맞다. 이렇게 큰 클래스를 쪼개다보면 프로그램은 체계가 잡히고 구조가 투명해진다.
            
        2. 함수를 리팩토링하게 되면 프로그램이 길어진다. 대신 다른 장점들이 생긴다. 181~184p
            1. 좀 더 길고 서술적인 변수 이름을 사용한다.
            2. 코드에 주석을 추가하는 수단으로 함수 선언과 클래스 선언을 활용한다.
            3. 가독성을 높이고자 공백을 추가하고 형식을 맞추었다.
4. 변경하기 쉬운 클래스
    1. 깨끗한 시스템은 클래스를 체계적으로 정리해 변경에 수반하는 위험을 낮춘다.
    2. 클래스 일부에서만 사용되는 비공개 메소드는 코드를 개선할 잠재적인 여지를 선사한다.
        1. 본문 186~188p 참조, 하나의 클래스에 묶여있는 메소드들을 수정하는 예제이다. 각 메소드들을 하나의 클래스, 하나의 내부 메소드로 변경한 뒤 추상 클래스 개념을 활용하여 각 클래스로 구별하는데 본질적인 목적이 제대로 이해되지 않는다. 추가적으로 확인이 필요하다.
    3. 변경으로부터 격리
        1. 구체적인 클래스는 상세한 구현(코드)을 포함하며, 추상 클래스는 개념만 포함한다고 배웠다. 그리고 상세한 구현에 의존하는 클라이언트 클래스는 구현이 바뀌면 위험에 빠진다. 따라서 우리는 인터페이스와 추상 클래스를 사용해 구현이 미치는 영향을 격리한다.
        2. 시스템의 결합도를 낮춰 유연성과 재사용성을 높혀라. 결합도가 낮다는 것은 각 시스템 요소가 다른 요소와 변경으로부터 격리되어잇다는 의미이다.
        3. 결합도를 최소로 줄이면 자연스럽게 **DIP (Dependency Inversion Principle)** 를 따르게 된다