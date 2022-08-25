| week1 | Chapter 1|
| ---------- | ------------ |
<br>

# 1. 디자인 패턴과 프로그래밍 패러다임
<br>

## 1.1 디자인 패턴

### 1.1.1 싱글톤 패턴
#### * 싱글톤 패턴
	- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
	- 데이터베이스 연결 모듈에 많이 사용
	- TDD(Test Driven Development)를 하기 어려움
		- TDD를 할 때 단위테스트를 주로 하는데 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 독립적인 인스턴스를 만들기 어려움
	- 모듈 간의 결합을 강하게 만들 수 있음
		- 의존성 주입을 통해 모듈 간의 결합을 느슨하게 만들어 해결할 수 있음
			- 의존성을 주입하면 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽고 마이그레이션하기도 수월함
			- 또한 애플리케이션 의존성 방향이 일관되어 애플리케이션을 쉽게 추론할 수 있으며 모듈 간의 관계들이 명확해짐
			- 그러나 모듈들이 더욱 분리되므로 클래스 수가 늘어나 복잡성이 증가될 수 있으며 런타임 페널티가 생길 수도 있음
			- 의존성을 주입할 때는 "상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다. 둘 다 추상화에 의존해야 하며, 추상화는 세부 사항에 의존하지 말아야 한다"는 원칙을 지켜야 함
#### * 자바스크립트의 싱글톤 패턴
```javascript
class Singleton {
	constructor() {
		if (!Singleton.instance) {
			Singleton.instance = this
		}
		return Singleton.instance
	}
	getInstance() {
		return this.instance
	}
	const a = new Singleton()
	const b = new Singleton()
	console.log(a === b) //true
}
```
#### * 데이터베이스 연결 모듈
``` javascript
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
#### * 자바에서의 싱글톤 패턴
```java
class Singleton {
	private static class singleInstanceHolder {
		private static final Singleton INSTANCE = new Singleton();
	}
	public static synchronized Singleton getInstance() {
		return singleInstanceHolder.INSTANCE;
	}
}

public class HelloWorld {
	public static void main(String[] args) {
		Singleton a = Singleton.getInstance();
		Singleton b = Singleton.getInstance();
		System.out.println(a.hashCode());
		System.out.println(b.hashCode());
		if (a == b) {
			System.out.println(true);
		}
	}
}
/*
705927765
705927765
true
*/
```
#### * Node.js, MongoDB, mongoose module에서의 싱글톤 패턴
``` javascript
Mongoose.prototype.connect = function(uri, options, callback) {
	const _mongoose = this instanceof Mongoose ? this : mongoose;
	const conn = _mongoose.connection;
	return _mongoose._promiseOrCallback(callback, cb => {
		conn.openUri(uri, options, err => {
			if (err != null) {
				return cb(err);
			}
			return cb(null, _mongoose_);
		});
	});
};
```
#### * Node.js, MySQL에서의 싱글톤 패턴
``` javascript
// 메인모듈
const mysql = require('mysql');
const pool = mysql.createPool({
	connectionLimit: 10,
	host: 'example.org',
	user: 'kundol',
	password: 'secret',
	database: '승철이디비',
});
pool.connect();

// 모듈 A
pool.query(query, function (error, results, fields) {
	if (error) throw error;
	console.log('The solution is: ', results[0].solution);
});

// 모듈 B
pool.query(query, function (error, results, fields) {
	if (error) throw error;
	console.log('The solution is: ', results[0].solution);
});
```
### 1.1.2 팩토리 패턴
#### * 팩토리 패턴
	- 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴
	- 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴
	- 상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합
	- 상위 클래스에서는 인스턴스 생성 방식에 알 필요가 없기 때문에 유연성 가짐
	- 객체 생성 로직이 떼어져 있기 때문에 유지 보수성 증가
#### * 자바스크립트의 팩토리 패턴
``` javascript
const num = new Object(42)
const str = new Object('abc')
num.constructor.name; // Number
str.constructor.name; // String
```
``` javascript
class Latte {
	constructor() {
		this.name = "latte"
	}
}

class Espresso {
	constructor() {
		this.name = "Espresso"
	}
}

class LatteFactory {
	static createCoffee() {
		return new Latte()
	}
}

class EspressoFactory {
	static createCoffee() {
		return new Espresso()
	}
}

const factoryList = { LatteFactory, EspressoFactory }

class CoffeeFactory {
	static createCoffee(type) {
		const factory = factoryList[type]
		return factory.createCoffee()
	}
}

const main = () => {
	// 라떼 커피를 주문한다.
	const coffee = CoffeeFactory.createCoffee("LatteFactory")
	// 커피 이름을 부른다.
	console.log(coffee.name) // latte
}

main()
```
#### * 자바의 팩토리 패턴
``` java
abstract class Coffee { 
    public abstract int getPrice(); 
    
    @Override
    public String toString(){
        return "Hi this coffee is "+ this.getPrice();
    }
}

class CoffeeFactory { 
    public static Coffee getCoffee(String type, int price){
        if("Latte".equalsIgnoreCase(type)) return new Latte(price);
        else if("Americano".equalsIgnoreCase(type)) return new Americano(price);
        else{
            return new DefaultCoffee();
        } 
    }
}
class DefaultCoffee extends Coffee {
    private int price;

    public DefaultCoffee() {
        this.price = -1;
    }

    @Override
    public int getPrice() {
        return this.price;
    }
}
class Latte extends Coffee { 
    private int price; 
    
    public Latte(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
}
class Americano extends Coffee { 
    private int price; 
    
    public Americano(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
} 
public class HelloWorld{ 
     public static void main(String []args){ 
        Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
        Coffee ame = CoffeeFactory.getCoffee("Americano",3000); 
        System.out.println("Factory latte ::"+latte);
        System.out.println("Factory ame ::"+ame); 
     }
} 
/*
Factory latte ::Hi this coffee is 4000
Factory ame ::Hi this coffee is 3000
*/
```
### 1.1.3 전략 패턴
#### * 전략 패턴
	- 객체의 행위를 바꾸고 싶은 경우 직접 수정하지 않고 캡슐화한 알고리즘을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴
#### * 자바의 전략 패턴
``` java
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;
interface PaymentStrategy { 
    public void pay(int amount);
} 

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;
    
    public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate){
        this.name=nm;
        this.cardNumber=ccNum;
        this.cvv=cvv;
        this.dateOfExpiry=expiryDate;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount +" paid using KAKAOCard.");
    }
} 

class LUNACardStrategy implements PaymentStrategy {
    private String emailId;
    private String password;
    
    public LUNACardStrategy(String email, String pwd){
        this.emailId=email;
        this.password=pwd;
    }
    
    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard.");
    }
} 

class Item { 
    private String name;
    private int price; 
    public Item(String name, int cost){
        this.name=name;
        this.price=cost;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
} 

class ShoppingCart { 
    List<Item> items;
    
    public ShoppingCart(){
        this.items=new ArrayList<Item>();
    }
    
    public void addItem(Item item){
        this.items.add(item);
    }
    
    public void removeItem(Item item){
        this.items.remove(item);
    }
    
    public int calculateTotal(){
        int sum = 0;
        for(Item item : items){
            sum += item.getPrice();
        }
        return sum;
    }
    
    public void pay(PaymentStrategy paymentMethod){
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}  

public class HelloWorld{
    public static void main(String []args){
        ShoppingCart cart = new ShoppingCart();
        
        Item A = new Item("kundolA",100);
        Item B = new Item("kundolB",300);
        
        cart.addItem(A);
        cart.addItem(B);
        
        // pay by LUNACard
        cart.pay(new LUNACardStrategy("kundol@example.com", "pukubababo"));
        // pay by KAKAOBank
        cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789", "123", "12/01"));
    }
}
/*
400 paid using LUNACard.
400 paid using KAKAOCard.
*/
```
#### * Node.js, passport에서의 전략 패턴
``` javascript
var passport = require('passport')
    , LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
    function(username, password, done) {
        User.findOne({ username: username }, function (err, user) {
          if (err) { return done(err); }
            if (!user) {
                return done(null, false, { message: 'Incorrect username.' });
            }
            if (!user.validPassword(password)) {
                return done(null, false, { message: 'Incorrect password.' });
            }
            return done(null, user);
        });
    }
));
```
### 1.1.4 옵저버 패턴
#### * 옵저버 패턴
	- 주체가 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴
	- 이벤트 기반 시스템, MVC 패턴에 주로 사용
#### * 자바에서의 옵저버 패턴
	- 상속: 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용하며 자식 클래스에서 추가 및 확장을 할 수 있는 것
	- 구현: 부모 인터페이스를 자식 클래스에서 재정의하여 구현하는 것으로 상속과는 달리 반드시 부모 클래스의 메서드를 재정의하여 구현해야 함
	- 상속과 구현의 차이: 상속은 일반 클래스, abstract 클래스를 기반으로 구현하며 구현은 인터페이스를 기반으로 구현
``` java
import java.util.ArrayList;
import java.util.List;

interface Subject {
    public void register(Observer obj);
    public void unregister(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer {
    public void update(); 
}

class Topic implements Subject {
    private List<Observer> observers;
    private String message; 

    public Topic() {
        this.observers = new ArrayList<>();
        this.message = "";
    }

    @Override
    public void register(Observer obj) {
        if (!observers.contains(obj)) observers.add(obj); 
    }

    @Override
    public void unregister(Observer obj) {
        observers.remove(obj); 
    }

    @Override
    public void notifyObservers() {   
        this.observers.forEach(Observer::update); 
    }

    @Override
    public Object getUpdate(Observer obj) {
        return this.message;
    } 
    
    public void postMessage(String msg) {
        System.out.println("Message sended to Topic: " + msg);
        this.message = msg; 
        notifyObservers();
    }
}

class TopicSubscriber implements Observer {
    private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic) {
        this.name = name;
        this.topic = topic;
    }

    @Override
    public void update() {
        String msg = (String) topic.getUpdate(this); 
        System.out.println(name + ":: got message >> " + msg); 
    } 
}

public class HelloWorld { 
    public static void main(String[] args) {
        Topic topic = new Topic(); 
        Observer a = new TopicSubscriber("a", topic);
        Observer b = new TopicSubscriber("b", topic);
        Observer c = new TopicSubscriber("c", topic);
        topic.register(a);
        topic.register(b);
        topic.register(c); 
   
        topic.postMessage("amumu is op champion!!"); 
    }
}
/*
Message sended to Topic: amumu is op champion!!
a:: got message >> amumu is op champion!!
b:: got message >> amumu is op champion!!
c:: got message >> amumu is op champion!!
*/ 
```
#### * 자바스크립트에서의 옵저버 패턴
	- 프록시 객체: 어떠한 대상의 기본적인 동작의 작업을 가로챌 수 있는 객체
		- target: 프록시할 대상
		- handler: 프록시 객체의 target 동작을 가로채서 정의할 동작들이 정해져 있는 함수
``` javascript
// 프록시 객체
const handler = {
	get: function(target, name) {
		return name === 'name' ? `${target.a} ${target.b}` : target[name]
	}	
}
const p = new Proxy({a: 'KUNDOL', b: 'IS AUMUMU ZANGIN'}, handler)
console.log(p.name) // KUNDOL IS AUMUMU ZANGIN
```
``` javascript
// 프록시 객체를 이용한 옵저버 패턴
function createReactiveObject(target, callback) { 
    const proxy = new Proxy(target, {
        set(obj, prop, value){
            if(value !== obj[prop]){
                const prev = obj[prop]
                obj[prop] = value 
                callback(`${prop}가 [${prev}] >> [${value}] 로 변경되었습니다`)
            }
            return true
        }
    })
    return proxy 
} 
const a = {
    "형규" : "솔로"
} 
const b = createReactiveObject(a, console.log)
b.형규 = "솔로"
b.형규 = "커플"
// 형규가 [솔로] >> [커플] 로 변경되었습니다
```
	- 프록시 객체의 get() 함수가 속성과 함수에 대한 접근을 가로채며 has() 함수는 in 연산자의 사용을 가로챔
	- set() 함수는 속성에 대한 접근을 가로챔
### 1.1.5 프록시 패턴과 프록시 서버
#### * 프록시 패턴
	- 대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴
	- 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용
#### * 프록시 서버
	- 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램
	- Nginx
		- 비동기 이벤트 기반의 구조와 다수의 연결을 효과적으로 처리 가능한 웹 서버
		- 익명 사용자의 직접적인 서버로의 접근을 차단하고 간접적으로 한 단계 더 거침으로써 보안성을 강화
	- CloudFlare
		- 전 세계적으로 분산된 서버가 있고 이를 통해 어떠한 시스템의 콘텐츠 전달을 빠르게 할 수 있는 CDN 서비스
		- DDOS 공격 방어: 의심스러운 트래픽, 시스템을 통해 오는 트래픽을 자동으로 차단해서 DDOS 공격으로 부터 보호, 공격에 대한 방화벽 대시보드도 제공
		- HTTPS 구축: 별도의 인증서 설치 없이 HTTPS 구축 가능
	- CORS와 프론트엔드의 프록시 서버
		- 프론트엔드 서버 앞단에 프록시 서버를 놓아 CORS 에러 해결
### 1.1.6 이터레이터 패턴
#### * 이터레이터 패턴
	- iterator을 사용하여 collection의 요소들에 접근하는 디자인 패턴
	- 순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 이터레이터라는 하나의 인터페이스로 순회 가능
#### * 자바스크립트에서의 이터레이터 패턴
``` javascript
const mp = new Map() 
mp.set('a', 1)
mp.set('b', 2)
mp.set('cccc', 3) 
const st = new Set() 
st.add(1)
st.add(2)
st.add(3) 
const a = []
for(let i = 0; i < 10; i++)a.push(i)

for(let aa of a) console.log(aa)
for(let a of mp) console.log(a)
for(let a of st) console.log(a) 
/* 
a, b, c 
[ 'a', 1 ]
[ 'b', 2 ]
[ 'c', 3 ]
1
2
3
*/
```
### 1.1.7 노출모듈 패턴
#### * 노출모듈 패턴
	- 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴
	- CJS 모듈
### 1.1.8 MVC 패턴
#### * MVC 패턴
	- 모델, 뷰, 컨트롤러로 이루어진 디자인 패턴
	- 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발할 수 있음
	- 재사용성과 확장성이 용이, 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해짐
	- 모델: 애플리케이션의 데이터인 데이터베이스, 상수, 변수
	- 뷰: 모델을 기반으로 사용자가 볼 수 있는 화면
	- 컨트롤러: 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할로 이벤트 등 메인 로직 담당
	- 대표적인 라이브러리 ex) React.js
### 1.1.9 MVP 패턴
#### * MVP 패턴
	- 모델, 뷰, 프레젠터로 이루어진 디자인 패턴
	- 뷰와 프레젠터가 1:1 관계이기 때문에 MVC 패턴보다 더 강한 결합을 가짐
### 1.1.10 MVVM 패턴
#### * MVVM 패턴
	- 모델, 뷰, 뷰모델로 이루어진 디자인 패턴
	- 뷰모델은 뷰를 더 추상화한 계층
	- MVVM 패턴은 MVC 패턴과는 다르게 커맨드와 데이터 바인딩을 가지는 것이 특징
	- 뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원하며 UI를 별도의 코드 수정 없이 재사용할 수 있고 단위 테스팅하기 쉬움
	- 대표적인 프레임워크 ex) Vue.js

<br>

## 1.2 프로그래밍 패러다임
### 1.2.1 선언형과 함수형 프로그래밍
#### * 선언형 프로그래밍, 함수형 프로그래밍
	- 선언형 프로그래밍: 무엇을 풀어내는가에 집중하며 프로그램은 함수로 이루어진 것이다 라는 명제가 담겨있는 패러다임
	- 함수형 프로그래밍: 선언형 패러다임의 일종으로 작은 순수 함수들을 블록처럼 쌓아 로직을 구현하고 고차 함수를 통해 재사용성을 높인 프로그래밍 패러다임
		- 자바스크립트는 단순하고 유연한 언어이고 함수가 일급 객체이기 때문에 함수형 프로그래밍 방식 선호
		- 순수함수: 출력이 입력에만 의존하는 것
		- 고차함수: 함수가 함수를 값처럼 매개변수로 받아 로직을 생성할 수 있는 것
			- 일급객체: 고차 함수를 쓰기 위해서는 해당 언어가 일급 객체라는 특징을 가져야 함
				- 변수나 메서드에 함수를 할당할 수 있음
				- 함수 안에 함수를 매개변수로 담을 수 있음
				- 함수가 함수를 반환할 수 있음
### 1.2.1 객체지향 프로그래밍
#### * 객체지향 프로그래밍
	- 데이터를 객체로 취급하여 객체 내부에 선언된 메서드를 활용하는 방식
	- 설계에 많은 시간이 소요되며 처리 속도가 다른 프로그래밍 패러다임에 비해 상대적으로 느림
	- 추상화: 복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려 냄
	- 캡슐화: 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉
	- 상속성: 상위 클래스의 특성을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것
	- 다형성: 하나의 메서드나 클래스가 다양한 방법으로 동작하는 것
		- 오버로딩: 같은 이름을 가진 메서드를 여러 개 두는 것, 정적 다형성
		- 오버라이딩: 상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의 하는 것, 동적 다형성
#### * 설계 원칙
	- 단일 책임 원칙: 모든 클래스는 각각 하나의 책임만 가져야 함
	- 개방-폐쇄 원칙: 유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야 함
	- 리스코프 치환 원칙: 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 함
	- 인터페이스 분리 원칙: 하나의 일반적인 인터페이스보다 구체적인 여러 개의 인터페이스를 만들어야 함
	- 의존 역전 원칙: 자신보다 변하기 쉬운 것에 의존 하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향받지 않게 하는 원칙
### 1.2.1 절차형 프로그래밍
#### * 절차형 프로그래밍
	- 로직이 수행되어야 할 연속적인 계산 과정으로 이루어져 있음
	- 코드의 가독성이 좋으며 실행 속도가 빠름
	- 모듈화 하기 어렵고 유지보수성이 떨어짐
<br>