# Clean Code - Robert C. Martin : PART 17

### PART 17 : 냄새와 휴리스틱

이 장에서는 다양한 프로그램을 검토하고 리팩터링하며 맡았던 기분 나쁜 냄새들을 정리한다. 아래는 파트별 쓸데없는 것들의 집합체이다.

### 주석

1. **부적절한 정보**
    1. 변경 이력은 소스코드만 번잡하게 만든다. 일반적으로 작성자, 최종 수정일, SPR 번호 등과 같은 메타 정보만 넣어라
2. **쓸모없는 주석**
3. **중복된 주석**
4. **성의없는 주석**
    1. 신중한 단어 선택, 주절대지 않고 당연한 소리를 반복하지 않으며 간결하고 명료하게 작성한다.
5. **주석 처리된 코드**
    1. 즉각 지워라. 걱정할 필요 없다.

### 환경

1. **여러 단계로 빌드해야한다**
    1. 빌드는 간단히 한단계로 끝내야 한다. 온갖 JAR, XML, 기타 시스템에 필요한 파일을 찾느라 여기저기 뒤적거리지 말고 한 명령으로 전체를 체크아웃하고 빌드할 수 있어야 한다.
2. **여러 단계로 테스트 해야한다**
    1. 모든 단위 테스트는 한 명령으로 돌려야 한다. IDE 에서 버튼 하나로 모든 테스트를 돌리는게 가장 이상적이다.

### 함수

1. **너무 많은 인수**
    1. 인수 개수는 작을수록 좋다. 아예 없으면 더욱더 좋다.
2. **출력 인수**
3. **플래그 인수**
4. **죽은 함수**
    1. 아무도 호출하지 않는 함수는 삭제한다. 죽은 코드는 낭비다. 삭제해도 소스 코드 관리 시스템이 기억하므로 걱정하지 마라

### 일반

1. **한 소스 파일에 여러 언어를 사용한다**
    1. 소스 파일 하나에서는 언어 하나만 사용하라.
2. **당연한 동작을 구현하지 않는다**
3. **경계를 올바로 처리하지 않는다**
    1. 스스로 머릿속에서 한번 돌려보고 끝내지마라. 모든 경계 조건을 찾아내고, 모든 경계 조건을 테스트하는 테스트 케이스를 작성하라.
4. **안전 절차 무시**
    1. 실패한 테스트 케이스를 일단 제껴두려는 생각은 절대 하지 마라.
5. **중복**
    1. 가장 중요하다. 코드에서 중복을 발견할 때마다 추상화할 기회로 간주하라. 중복된 코드를 하위 루틴이나 다른 클래스로 분리하라.
6. **추상화 수준이 올바르지 못하다**
    1. 추상화로 개념을 분리할 때는 철저해야 한다.
7. **기초 클래스가 파생 클래스에 의존한다**
    1. 개념을 기초 클래스와 파생 클래스로 나누는 가장 흔한 이유는 고차원 기초 클래스 개념을 저차원 파생 클래스 개념으로부터 분리해 독립성을 보장하기 위해서다.
    2. 즉 기초 클래스가 파생 클래스를 사용한다면 뭔가 문제가 있다는 말이다.
8. **과도한 정보**
    1. 잘 정의된 모듈은 인터페이스가 아주 작다. 결합도가 낮다는 말이다.
    2. 클래스가 제공하는 메서드 수는 작을수록 좋다. 함수가 아는 변수 수도 작을수록 좋다. 클래스에 들어있는 인스턴스 변수 수도 작을수록 좋다.
    3. 자료를 숨겨라. 유틸리티 함수를 숨겨라. 상수와 임시변수를 숨겨라. 메서드나 인스턴스 변수가 넘쳐나는 클래스를 피해라.
    4. 인터페이스를 매우 작고 깐깐하게 만들어라. 정보를 제한해 결합도를 낮추는게 중요하다
9. **죽은 코드**
    1. 실행되지 않는 코드를 말한다.
        1. 불가능한 조건을 확인하는 if문ㅁ
        2. throw 문이 없는 try문에서의 catch 블록
        3. 아무도 호출하지 않는 유틸리티 함수
        4. switch/case 문에서 불가능한 case조건
        5. 발견하면 적절한 장례식이 필요하다. 시스템에서 제거해라.
10. **수직 분리**
    1. 변수와 함수는 사용되는 위치에 가깝게 정의한다.
    2. 비공개 함수는 처음으로 호출한 직후에 정의한다. 처음으로 호출된 이후 조금만 아래로 내려가도 쉽게 눈에 띄어야 한다.
11. **일관성 부족**
    1. 어떤 개념을 특정 방식으로 구현했다면 유사한 개념도 같은 방식으로 구현한다.
        
        *ex)* 한 함수에서 response 변수에 HttpServletResponse 인스턴스를 저장했다면 이 객체를 사용하는 다른 함수에서도 일관성 있게 동일한 변수 이름을 사용해야 한다*.*
        
12. **잡동사니**
    1. 비어있는 기본 생성자, 아무도 사용하지 않는 변수, 아무도 호출하지 않는 함수, 정보를 제공하지 못하는 주석 등 모두 잡동사니다. 제거해라 모두.
13. **인위적 결합**
    1. 서로 무관한 개념을 인위적으로 결합하지 않는다.
        1. 일반적인 enum은 특정 클래스에 속할 필요가 없다.
        2. 범용 static 함수도 특정 클래스에 속할 필요가 없다. 
    2. 함수, 상수, 변수를 선언할 때는 시간을 들여 올바른 위치를 고민한다. 그저 편한 곳에 선언하고 내버려두면 안된다.
14. **기능 욕심**
    1. 클래스는 자기 것에만 집중해야한다. 메소드를 활용해 다른 클래스의 내용을 조작해서는 안된다.
15. **선택자 (`boolean`) 인수** 
    1. 목적을 기억하기 어렵다
    2. 선택자 인수는 큰 함수를 작은 함수로 쪼개지 않으려는 게으름의 소산이다. 
16. **모호한 의도**
    1. 의도를 최대한 명확히 밝혀라
17. **잘못 지운 책임**
    1. 코드는 독자가 자연스럽게 기대할 위치에 배치한다.
    2. 때로는 독자에게 직관적인 위치가 아닌 개발자에게 편한 함수에 기능을 배치하라.
18. **부적절한 static 함수**
    1. 특정 객체와 관련이 없으면서 모든 정보를 인수에서 가져올 수 있는지, 즉 재정의할 가능성이 존재하는 지 확인하라
19. **서술적 변수**
    1. 프로그램 가독성을 높이는 가장 효과적인 방법은 계산을 여러 단계로 나누고 중간 값으로 서술적인 변수 이름을 사용하는 것이다.
20. **이름과 기능이 일치하는 함수**
    1. 아래 함수는 5일을 더하는 함수인가? 혹은 5시간인가? addDaysTo, increaseByDays 라는 함수명이 add 보다는 훨씬 나을 것이다. 이름만으로 분명하지 않아 구현을 살펴야 한다면 더 좋은 네이밍을 써야 한다.
    
    ```jsx
    Date newDate = date.add(5);
    ```
    
21. **알고리즘을 이해하라**
    1. 올바른 알고리즘을 통과하게 하라. 단순히 테스트 케이스를 모두 통과한다고 장땡이 아니다. 이를 위해선 기능이 뻔히 보일 정도로 함수를 깔끔하고 명확하게 재구성하는 방법이 최고다.
22. **논리적 의존성은 물리적으로 드러내라**
    1. 의존하는 모듈이 상대 모듈에 대해 뭔가를 가정해선 안된다. 모든 정보를 명시적으로 요청하라.
23. **if/else, switch/case 문보다 다형성을 사용하라**
    1. switch문 하나 규칙을 따른다. 선택 유형 하나에는 switch문을 한번만 사용하고, 같은 선택을 수행하는 다른 코드에서는 다형성 객체를 생성해 switch문을 대신해라
24. **표준 표기법을 따르라**
    1. 인스턴스 변수 이름을 선언하는 위치, 클래스,메소드,변수 이름을 정하는 방법 등등. 표준은 표준이다. 모두가 동의한 위치, 모두가 알만한 곳에 작성하라.
25. **매직 숫자는 명명된 상수로 교체하라**
26. **정확하라**
    1. 뭔가를 결정할 때는 정확히 결정한다. 결정을 내리는 이유와 예외를 처리할 방법을 분명히 알아야하며 대충 결정해서는 안된다.
    2. 코드에서 모호성과 부정확은 의견차나 게으름의 결과다. 어느 쪽이든 제거하라.
27. **관례보다 구조를 사용하라**
    1. enum 변수가 멋진 switch/case문보다 추상 메소드가 있는 기초 클래스가 더 좋다. 관례보다 구조적으로 강제할 수 있는 방법을 찾아라.
28. **조건을 캡슐화하라**
    
    ```java
    // BAD
    if (shouldBeDeleted(timer))
    
    // GOOD
    if (timer.hasExpired() && !timer.isRecurrent())
    ```
    
29. **부정 조건은 피하라**
    1. 부정 조건은 긍정 조건보다 이해하기 어렵다. 가능하면 긍정 조건으로 표현하자
        
        ```java
        // BAD
        if (!buffer.shouldNotCompact())
        
        // GOOD
        if (buffer.shouldCompact())
        ```
        
30. **함수는 한 가지만 해야 한다**
    
    ```java
    // BAD
    public void pay() {
    	for (Employee e : employees) {
    		if (e.isPayday()) {
    			Money pay = e.calculatePay();
    			e.deliverPay(pay);
    		}
    	}
    }
    
    // GOOD
    public void pay() {
    	for (Employee e : employees) 
    		payIfNecessary(e);
    }
    
    private void payIfNecessary(Employee e) {
    	if (e.isPayday())
    		calculateAndDeliverPay(e);
    }
    
    private void calculateAndDeliverPay(Employee e) {
    	Money pay = e.calculatePay();
    	e.deliverPay(pay);
    }
    ```
    
31. **숨겨진 시간적인 결합**
    1. 함수를 짤 때는 함수 인수를 적절히 배치해 함수가 호출되는 순서를 명백히 드러내야 한다.
32. **일관성을 유지하라**
    1. 코드 구조를 잡을 때는 이유를 고민하라.
33. **경계 조건을 캡슐화하라**
34. **함수는 추상화 수준을 한 단계만 내려가야 한다**
    1. 함수 내 모든 문장은 추상화 수준이 동일해야 한다.
35. **설정 정보는 최상위 단계에 둬라**
    1. 최상위 단계에 둬야 할 기본값 상수나 설정 관련 함수를 저차원 함수에 숨겨선 안된다. 
36. **추이적 탐색을 피하라**
    1. 모듈은 주변 모듈을 모를수록 좋다. A가 B를 사용하고 B가 C를 사용한다 하더라도 A가 C를 알아야 할 필요는 없다는 뜻이다. 이를 디미터의 법칙이라 부른다.

### 자바

1. **긴 import 목록을 피하고 와일드카드를 사용하라**
2. **상수는 상속하지 않는다**
3. **상수 대 Enum**
    1. public static final int 라는 옛날 기교를 더 이상 사용할 필요가 없다. 

### 이름

1. **서술적인 이름을 사용하라**
    1. 이름을 성급하게 정하지 말고, 서술적인 이름을 신중하게 골라라.
    2. 소프트웨어 가독성의 90%는 이름이 결정한다. 그래야 예상한 대로 돌아갈 수 있다.
2. **적절한 추상화 수준에서 이름을 선택하라**
    1. 구현을 드러내는 이름을 피하고, 작업 대상 클래스나 함수가 위치하는 추상화 수준을 반영하는 이름을 선택하라.
3. **가능하다면 표준 명명법을 사용하라**
    1. 기존 명명법을 사용하는 이름이 이해하기 더 쉽다. 
4. **명확한 이름**
    1. 함수나 변수의 목적을 명확히 밝히는 이름을 선택한다.
5. **긴 범위는 긴 이름을 사용하라**
    1. 이름 길이는 범위 길이에 비례해야 한다.
6. **인코딩을 피하라**
7. **이름을 부수 효과를 설명하라**
    1. 함수, 변수, 클래스가 하는 일을 모두 기술하는 이름을 사용한다. 여러 작업을 수행하는 함수에 동사 하나만 달랑 사용하는 것은 옳지 않다.

### 테스트

1. **불충분한 테스트**
    1. 테스트 케이스는 잠재적으로 깨질만한 부분을 모두 테스트해야한다. 테스트 케이스가 확인하지 않는 조건이나 검증하지 않는 계산이 있다면 테스트는 불완전하다는 것을 명심하자.
2. **커버리지 도구를 사용하라**
    1. 커버리지 도구는 테스트가 빠뜨리는 공백을 알려준다. 
3. **사소한 테스트를 건너뛰지 마라**
    1. 짜기 쉬운 사소한 테스트가 제공하는 가치는 구현에 드는 비용을 넘을 정도로 중요하다.
4. **무시한 테스트는 모호함을 뜻한다**
5. **경계 조건을 테스트하라**
6. **버그 주변은 철저히 테스트하라**
7. **실제 패턴을 살펴라**
    1. 합리적인 순서로 정렬된 꼼꼼한 테스트 케이스는 실패 패턴을 드러낸다.
8. **테스트 커버리지 패턴을 살펴라**
9. **테스트는 빨라야 한다**