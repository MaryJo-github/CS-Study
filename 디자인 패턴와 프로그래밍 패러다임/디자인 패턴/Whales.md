## Section 1.1 디자인 패턴

- 정의: 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 `규약` 형태로 만들어 놓은 것

### 1.1.1 싱글톤 패턴
<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/df6b6dd7-b781-44c1-818a-91fbcd79c5ce" width = "400">

- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
- 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용
- 보통 데이터베이스 연결 모듈에 많이 사용
- 장점: 인스턴스를 생성할 때 드는 비용이 줄어든다.
- 단점:
    - 의존성이 높아진다.
        - 의존성
            - 종속성 이라고도 하며
            - A가 B에 의존성이 있다 → B의 변경 사항에 대해 A 또한 변해야 한다.
    - TDD(Test Driven Development)를 할 때 걸림돌.
        
        → TDD를 할 때 주로 단위 테스트를 하는데 단위 테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다. 
        
        → 싱글톤은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 `독립적인` 인스턴스를 만들기 어렵다.
        
- 의존성 주입(DI, Dependency Injection)
    <img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/d8be21be-94e8-4e13-9a07-4f46bb4749b4" width = "400">
    
    - 모듈 간의 결합을 조금 더 느슨하게 만들어 해결 가능
    - 메인 모듈이 ‘직접’ 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 `의존성 주입자`(dependency injector)가 이 부분을 가로채 메인 모듈이 ‘간접’적으로 의존성을 주입하는 방식
    - 이를 통해 메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어지게 된다.
    → 디커플링이 된다
    - 장점
        - 모듈들을 쉽게 교체할 수 있는 구조가 됨 → 테스팅하기 쉽고, 마이그레이션하기도 수월
        - 구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 줌 → 애플리케이션 의존성 방향이 일관되고, 애플리케이션을 쉽게 추론 가능, 모듈 간의 관계들이 조금 더 명확해짐
    - 단점
        - 모듈들이 더욱더 분리 → 클래스 수 증가 → 복잡성 증가 → 약간의 런타임 패널티 생성
    - 원칙
        - 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
        - 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다.

### 1.1.2 팩토리 패턴

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/0ce1b7a0-3742-446a-971f-072640cb43cb" width = "400">

- 객체를 사용하는 코드에서 **객체 생성 부분**을 떼어내 추상화한 패턴이자
- 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고,
하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴
- 장점
    - 상위 클래스와 하위 클래스가 분리 → 느슨한 결합
    - 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없다. → 유연성 증가
    - 객체 생성 로직이 따로 떼어져 있다. → 한 곳만 고칠 수 있다. 유지보수성 증가
- 예
    - 라떼, 아메리카노, 우유 레시피 가 들어있는 하위 클래스가 컨베이어 벨트를 통해 전달
        → 상위 클래스인 바리스타 공장에서 이 레시피를 토대로 우유를 생산
    
    (⇒ 하위 클래스를 먼저 만들고 상위를 만드네?)
    
    <img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/e5080e6f-c44f-40cf-88f0-7c8af5ecf499" width = "500">

    - CoffeeFactory 라는 상위 클래스가 중요한 뼈대를 결정
    LatteFactory 하위 클래스가 구체적 내용 결정
    - 의존성 주입이라고도 볼 수 있다.
    CoffeeFactory에서 LatteFactory의 인스턴스를 생성하는 것이 아닌 LatteFactory에서 생성한 인스턴스를 CoffeeFactory에 주입하고 있기 때문

### 1.1.3 전략 패턴 (Strategy Pattern)

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/a46f48d7-fc10-4f17-b0da-e94b5a87776a" width = "400">

- 정책 패턴 (policy pattern)이라고도 함
- 객체의 행위를 바꾸고 싶은 경우 ‘직접’ 수정하지 않고 전략이라고 부르는 `캡슐화한 알고리즘`을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴

### 1.1.4 옵저버 패턴

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/712b6471-6ca0-42ed-afaa-bbb9a7af0829" width = "300"> </br>
객체와 주체가 분리되어 있는 옵저버 패턴

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/65e9fc6e-3168-40da-b3e9-09ccdc6637ab" width = "300"> </br>
객체와 주체가 합쳐진 옵저버 패턴

- 주체가 어떤 객체(subject)의 상태 변화를 관찰하다가
상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려준다.
- 주체: 객체의 상태 변화를 보고 있는 관찰자
- 옵저버: 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 ‘추가 변화 사항’이 생기는 객체들을 의미

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/defdd5de-e2f3-49d9-8413-c450c2abb049" width = "400">

트위터의 옵저버 패턴

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/98a4631d-685a-4a75-b9e0-6c66ae2b948e" width = "400">

옵저버 패턴 구조

### 1.1.5 프록시 패턴과 프록시 서버

- 프록시 패턴(proxy pattern)
    
    <img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/acea5671-9dbf-4b50-ac16-89cc9409cebe" width = "400">

    - 대상 객체(subject)에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴
    - 객체의 속성, 변환 등을 보완하며 보완, 데이터 검증, 캐싱, 로깅에 사용한다.
    - 프록시 객체로 쓰이기도 하지만 프록시 서버로도 활용된다.
- 프록시 서버(proxy server)
    - 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램
        
        <img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/fc1f7f2c-2ea3-4776-a2d4-500f2acabf00" width = "400">

        nginx를 이용한 프록시 서버
        
    - nginx를 프록시 서버로 둬서 실제 포트를 숨길 수 있고,
    정적 자원을 gzip 압축하거나, 메인 서버 앞단에서의 로깅도 가능

### 1.1.6 이터레이터 패턴 (Iterator Pattern)

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/8af7a3c1-e263-4637-9442-a1b532a45dea" width = "400">

- 이터레이터(iterator)를 사용하여 컬렉션(collection)의 요소들에 접근하는 디자인 패턴
- 순회 가능한 여러 가지 자료형의 구조와는 상관없이
이터레이터라는 하나의 인터페이스로 순회가 가능

### 1.1.7 노출모듈 패턴(Revealing module pattern)

- 즉시 실행 함수(함수를 정의하자마자 바로 호출하는 함수)를 통해 private, public 같은 접근 제어자를 만드는 패턴
- 자바스크립트는 접근 제어자가 없어서 노출 모듈 패턴을 사용한다.

### 1.1.8 MVC 패턴

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/5c9d8ba0-5ee0-420a-953f-75e051330fb6" width = "500">

- 애플리케이션의 구성 요소를 세 가지 Model, View, Controller로 구분
- 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발 가능
- 재사용성과 확장성 용이
- 앱이 복잡해질수록 모델과 뷰의 관계가 복잡
- View에서 데이터를 생성하거나 수정하면 Controller를 통해 Model을 생성하거나 갱신

Model

- 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등

View

- 사용자 인터페이스 요소
- 모델을 기반으로 사용자가 볼 수 있는 화면
- 모델이 가지고 있는 정보는 따로 저장하지 않는다.
- 변경이 일어나면 컨트롤러에 이를 전달해야 한다.

Controller

- 하나 이상의 Model과 하나 이상의 View를 잇는 다리 역할
- 이벤트 등 메인 로직을 담당
- 모델과 뷰의 생명 주기 관리
- 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요서에 해당 내용에 대해 알려줌

### 1.1.9 MVP 패턴

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/a51d9a08-b226-40b9-8e41-6c59309139dd" width = "400">

- MVC 패턴으로부터 파생
- Controller가 프레젠터(Presenter)로 교체된 패턴
- 뷰와 프레젠터는 일대일 관계이기 때문에 MVC 패턴보다 더 강한 결합을 지님

### 1.1.10 MVVM 패턴

<img src = "https://github.com/MaryJo-github/CS-Study/assets/124643545/b6289db2-d44a-4427-951c-ecd1d98d290d" width = "500">

- MVC의 Controller가 뷰모델(View Model)로 바뀐 패턴
- 뷰모델 → 뷰를 더 추상화한 계층
- MVC와 다르게 커맨드(여러 가지 요소에 대한 처리를 하나의 액션으로 처리할 수 있게 하는 기법)와
데이터 바인딩을 가지는 것이 특징
- 뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원하며 UI를 별도의 코드 수정 없이 재사용 가능
- 단위 테스팅하기 쉽다.
