## 디자인 패턴과 프로그래밍 패러다임

- 라이브러리
  
  - 공통으로 사용할 수 있는 특정한 기능들을 모듈화한 것을 의미한다. 폴더명, 파일명 등에 대한 규칙이 없고 프레임워크에 비해 자유롭다



<br> 

- 프레임워크
  
  - 공통으로 사용될 수 있는 특정한 기능들을 모듈화한 것을 의미한다. 폴더명, 파일명 등에 대한 규칙이 있으며 라이브러리에 비해 좀 더 엄격하다.



<br>



### 디자인 패턴

- 디자인 패턴
  
  - 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 '규약' 형태로 만들어 놓은 것



<br>





- 싱글톤 패턴
  
  - 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
  
  - 데이터베이스 연결 모듈에 많이 사용
  
  - 하나의 인스턴스를 만들고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 인스턴스를 생성할 때 드는 비용이 줄어드는 장점
  
  - TDD(Test Driven Development)를 할 때 좋지 않다는 단점
    
    - TDD : 테스트 코드를 먼저 작성하는 개발 방법론
    
    - TDD의 경우 단위 테스트를 주로 하는데, 이는 테스트가 서로 독립적이어야하며 테스트를 어떤 순서로든 실행할 수 있어야한다는 특징
    
    - 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현 ⇒ 각 테스트마다 독립적인 인스턴스를 만들기 어려움
  
  - 모듈 간의 결합을 강하게 만들 수 있다는 단점
    
    - 의존성 주입(DI, Dependency Injection)을 통해 모듈 간의 결합을 조금 더 느슨하게 만들어 해결 가능
    
    - 의존성이란 종속성이라고도 하며 A가 B에 의존성이 있다는 것은 B의 변경 사항에 A 또한 변해야 한다는 뜻
    
    - 의존성 주입은 상위 모듈은 하위 모듈의 어떠한 것도 가져오지 않아야 한다. 또한 둘다 추상화에 의존해야 하며, 이 때 추상화는 세부 사항에 의존하지 말아야 한다.
    
    - 의존성 주입에 의해 클래스 수가 늘어나 복잡성이 증가될 수 있고 약간의 런타임 발생
  
  ```javascript
  const URL = 'mongodb://localhost:27017/kundolapp'
  const createConnection = url => ({"url" : url})
  class DB {
      constructor(url) {
          if (!DB.instance) {
              DB.instance = createConnection(url)
          }
          return DB.instance
      }
      connect() {
          return this.instance
      }
  }
  
  const a = new DB(URL)
  const b = new DB(URL)
  console.log(a === b) //true
  ```



<br>



- 팩토리 패턴
  
  - 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴 
  
  - 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴
  
  - 상위 클래스와 하위 클래스 분리 ⇒ 느슨한 결합, 유연성 향상, 유지보수성 증가
  
  ```java
  abstract class Coffee {
      public abstract int getPrice();
  
      @Override
      public String toString() {
          return "Hi this coffee is " + this.getPrice();
      }
  }
  
  
  class CoffeeFactory {
      public static Coffee getCoffee(String type, int price) {
          if ("Latte".equalsIgnoreCase(type)) { 
              return new Latte(price);
          } else if ("Americano".equalsIgnoreCase(type)) {
              return new Americano(price);
          } else {
              return new DefalutCoffee();
          }
      }
  }
  
  
  class DefaultCoffee extends Coffee {
      private int price;
      
      public DefaultCoffee() {
          this.price = -1;
      }
      
      @Override
      public int getPrice(){
          return this.price;
      }
  }
  
  
  class Latte extends Coffee {
      private int price;
  
      public Latte(int price) {
          this.price = price;
      }
  
      @Override
      public int getPrice() {
          return this.price;
      }
  }
  
  class Americano extends Coffee {
      private int price;
      
      public Americano(int price) {
          this.price = price;
      }
  
      @Override
      public int getPrice() {
          return this.price;
      }
  }
  
  
  public class HelloWorld {
      public static void main(String[] args) {
          Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
          Coffee ame= CoffeeFactory.getCoffee("Americano", 3000);
          System.out.println("Factory latte:" + latte);
          System.out.println("Factory americano:" + ame);
      }
  }
  
  
  /*
  Factory latte: Hi this coffee is 4000
  Factory americano: Hi this coffee is 3000
  */
  ```



<br>



- 전략 패턴
  
  - 전략 패턴(strategy pattern) = 정책 패턴(policy pattern)
  
  - 객체의 행위를 바꾸고 싶은 경우 직접 수정하지 않고 전략이라고 부르는 **캡슐화한 알고리즘**을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴
  
  - 예시로는 네이버페이, 카카오페이 등 다양한 방법으로 결제 진행
  
  - passport
    
    - Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리
    
    - 여러 가지 '전략' 기반으로 인증



<br>



- 옵저버 패턴
  
  - 어 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 패턴 디자인
  
  - 주로 이벤트 기반 시스템에 사용하고 MVC(Model - View - Controller) 패턴에도 사용
  
  - 모델에서 변경 사항이 생겨  update( ) 메서드로 옵저버인 뷰에 알려주고 이를 기반으로 컨트롤러 등이 작동
  
  - 예시로는 트위터



<br>



- 상속
  
  - 상속(extends)은 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용
  
  - 자식 클래스에서 추가 및 확장을 할 수 있는 것
  
  - 재사용성, 중복성의 최소화
  
  - 일반 클래스, 추상 클래스 기반 구현



<br>



- 구현
  
  - 구현(implements)은 부모 인터페이스(interface)를 자식 클래스에서 재정의하여 구현하는 것
  
  - 상속과 달리 반드시 부모 클래스의 메서드를 재정의하여 구현
  
  - 인터페이스 기반 구현



<br>



- 프록시 객체
  
  - 어떠한 대상의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체
  
  - 자바 스크립트에서 프록시 객체
    
    - target : 프록시할 대상
    
    - handler : 프록시 객체의 target 동작을 가로채서 정의할 동작들이 정해져 있는 함수



<br>



- 프록시 패턴
  
  - 프록시 패턴(proxy pattern)은 대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴
  
  - 객체의 속성, 변환등을 보완
  
  - 보안, 데이터 검증, 캐싱, 로깅에 사용
  
  - 프록시 서버로도 활용



<br>



- 프록시 서버
  
  - 서버와 클라이언트 사이에 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램
  
  - 캐싱: 캐시 안에 정보를 담아두고 캐시 안에 있는 정보를 요구하는 요청에 대해 원격 서버에 요청하지 않고 캐시 안에 있는 데이터를 활용하는 것
  
  - nginx : 비동기 이벤트 기반의 구조와 다수의 연결을 효과적으로 처리 가능한 웹 서버, 주로 Node.js 서버 앞단의 프록시 서버로 활용
  
  - CloudFlare : 프록시 서버로 사용하는 CDN 서비스, DDOS 공격 방어와 HTTPS 구축도 가능



<br>



- DDOS 
  
  - 짧은 기간 동안 네트워크에 많은 요청을 보내 네트워크를 마비시켜 웹사이트의 가용성을 방해하는 사이버 공격 유형



<br>



- CDN (Content Delivery Network)
  
  - 각 사용자가 인터넷에 접속하는 곳과 가까운 곳에서 콘텐츠를 캐싱 또는 배포하는 서버 네트워크
  
  - 웹 서버로부터 콘텐츠를 다운하는 시간을 줄일 수 있음



<br>



- CORS와 프론트엔드의 프록시 서버
  
  - CORS(Cross - Origin - Resource - Sharing)은 서버가 웹 브라우저에서 리소스를 로드할 때 다른 origin을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘
    
    - origin : 프로토콜과 호스트 이름, 포트의 조합



<br>



- 이터레이터 패턴
  
  - 이터레이터(iterator)를 사용하여 컬렉션(collection)의 요소들에 접근하는 디자인 패턴



<br>



- 노출모듈 패턴(revealing module pattern)
  
  - 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴



<br>



- MVC 패턴
  
  - MVC 패턴은 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴
  
  - 모델 : 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻한다. 뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신
  
  - 뷰 : 사용자 인터페이스 요소. 모델을 기반으로 사용자가 볼 수 있는 화면
  
  - 컨트롤러 : 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할로 이벤트 등 메인 로직을 담당



<br>



- MVP 패턴
  
  - MVC 패턴으로부터 파생되었으며 MVC에서 C에 해당하는 컨트롤러가 프레젠터(presenter)로 교체된 패턴
  
  - 뷰와 프레젠터는 일대일 관계이기 때문에 MVC 패턴보다 더 강한 결합을 지닌 디자인 패턴
  
  - 예시로는 리액트



<br>



- MVVM 패턴
  
  - MVC의 C에 해당하는 컨트롤러가 뷰모델(view model)로 바뀐 패턴
  
  - 커맨드와 데이터 바인딩을 가지는 것이 특징
  
  - 뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원하며 UI를 별도의 코드 수정 없이 재사용할 수 있고 단위 테스팅하기 쉽다는 장점
  
  - 예시로는 뷰



<br>



- 커맨드
  
  - 여러 가지 요소에 대한 처리를 하나의 액션으로 처리할 수 있게 하는 기법



<br>



- 데이터 바인딩
  
  - 화면에 보이는 데이터와 웹 브라우저의 메모리 데이터를 일치시키는 기법으로, 뷰 모델을 변경하면 뷰가 변경된다.



<br>



### 프로그래밍 패러다임

- 프로그래밍 패러다임
  
  - 프로그래머에게 프로그래밍의 관점을 갖게 해주는 역할을 하는 개발 방법론



<br>



- 선언형과 함수형 프로그래밍
  
  - 선언형 프로그래밍(declarative programming) : '무엇을' 풀어내는가에 집중하는 패러다임. "프로그램은 함수로 이루어진 것이다."라는 명제가 담겨있는 패러다임
  
  - 함수형 프로그래밍(functional programming) : 선언형 패러다임의 일종으로 순수 함수들을 블록처럼 쌓아 로직을 구현하고 고차 함수를 통해 재사용성을 높인 프로그래밍 패러다임



<br>



- 순수 함수
  
  - 출력이 입력에만 의존하는 것
  
  ```javascript
  const pure = (a, b) => {
      return a + b
  }
  ```



<br>



- 고차 함수
  
  - 함수가 함수를 값처럼 매개변수로 받아 로직을 생성할 수 있는 것
  
  - 일급 객체 : 고차함수를 쓰기 위해서는 해당 언어가 일급 객체라는 특징이 필요하며 그 특장은 다음과 같다
    
    - 변수나 메서드에 함수를 할당할 수 있다.
    
    - 함수 안에 함수를 매개변수로 담을 수 있다.
    
    - 함수가 함수를 반환할 수 있다.



<br>



- 객체지향프로그래밍
  
  - 추상화 : 복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려내는 것
  
  - 캡슐화 : 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것
  
  - 상속성 : 상위 클래스의 특성을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것
  
  - 다형성 : 하나의 메서드나 클래스가 다양한 방법으로 동작하는 것. 대표적으로 오버라이딩과 오버로딩
    
    

<br>



- 오버로딩
  
  - 같은 이름을 가진 메서드를 여러 개 두는 것
  
  - 메서드의 타입, 매개변수의 유형과 개수 등으로 여러 개를 둘 수 있으며 컴파일 중에 발생할 수 있는 '정적' 다형성



<br>



- 오버라이딩
  
  - 주로 메서드 오버라이딩을 말하며 상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의 하는 것
  
  - 런타임 중에 발생하는 '동적' 다형성



<br>



- 설계 원칙
  
  - 객체지향 프로그래밍을 설계할 때는 SOLID 원칙을 지켜야 한다.
  
  - 단일 책임 원칙(SRP) : 모든 클래스는 각각 하나의 책임만 가져야 한다.
  
  - 개방-폐쇄 원칙(OCP) : 유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야 하는 원칙. 기존의 코드는 잘 변경하지 않으면서도 확장은 쉽게 할 수 있어야 한다.
  
  - 리스코프 치환 원칙(LSP) : 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다
  
  - 인터페이스 분리 원칙(ISP) : 하나의 일반적인 인터페이스보다 구체적인 여러 개의 인터페이스를 만들어야 한다
  
  - 의존 역전 원칙(DIP) : 자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향을 받지 않게 하는 원칙



<br>



- 절차형 프로그래밍
  
  - 코드의 가독성이 좋으며 실행 속도가 빠르다.
  
  - 모듈화하기 어렵고 유지 보수성이 떨어진다.
  
  - 예시로는 포트란(fortran)










