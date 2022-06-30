# **3. 취업**

## **1. 5개 이하의  public 메서드만 노출하세요.**

작은 객체 - 유지보수 가능하고 응집력이 높고, 테스트 하기 용이함

( 작아질수록 옆에서 말한 내용이 좋아진다.)

- 클래스가 더 작을수록 실수 가능성이 적어지기 떄문에 우아하다.
- 작은 클래스는 코드양이 적고 에러찾기 쉽고 수정하기 쉽기 때문에 유지보수성이 좋다.
- 모든 시나리오를 쉽게 재현하고 시나리오 수도 적기 때문에 테스트가 쉽다.
- 메서드와 프로퍼티가 가까이 있기 때문에 응집도가 높다.

클래스의 크기를 정하는 기준을 public 메서드의 개수를 사용하기를 권한다.

(저자의 기준은 5개라고 합니다.)

## **2. 정적 메서드를 사용하지 마세요.**

```java
class WebPage {
	public static String read(String uri) { }
}
//이제 read() 로 쉽게 사용 가능
String html = WebPage.read("http://www.java.com");
```

하지만 저자는 read() 메서드를 반대한다. → 정적 메서드 대신 객체를 사용

```java
String Html = new WebPage("http://wwww.java.com").read();
```

### 2.1 객체 대 컴퓨터 사고(object vs computer thinking)

```c
int max(int a, int b) {
	if( a > b ) {
	return a;
	}
		return b;
}
//순차적으로 잘 실행된다. 순차지향 프로그래밍 언어 C
```

순차적인 사고 방식을 가리켜 **‘컴퓨터 입장에서 생각하기’**

```lisp
(defun max (a b)
(if (> a b) a b ))
//위 코드와같은 함수 프로그래밍 언어
```

위는 CPU로 분리되어 있따. 즉 컴퓨터가 아니라 함수처럼 사고하고 있다는 것.

 cpu에게 할 일을 지시 하는 것이 아니라 **정의**한다.

컴퓨터 처럼 생각하기에서에서는 명령의 실행 흐름을 제어할 책임이 우리에게 있다.

객체지향적으로 생각하기에서 우리는 그저 누가 누구인지만 정의 하고 객체들이 필요할떄 스스로 상호작용하도록 한다.

정적 메서드는  oop와 아무 상관 없으며 ,객체지향언어 에서  절차적 코드를 작성하도록 부추길 뿐이다.

### 2.2 선언형 스타일 대 명령형 스타일(declartive vs. imperative style)

 

명령형 프로그래밍 -  프로그램의 상태를 변경하는 문장을 사용해서 계산 방식을 서술

선언형 프로그래밍 - 제어 흐름을 서술하지 않고 계산 로직을 서술

선언형 프로그래밍이 더 강력하다. 이유는 후술.

```java
public static int between(int l, int r, int x){
	return Math.min(Math.max(l, x), r);
}
//math = 정적 메서드, between() = 명령형 스타일 연산자
```

계산은 필요한 시점에 즉시 수행되기 때문에  between은 명령형 스타일 이다.

```java
// 선언형
class Between implement Number{
...
Between(Number left, Nubmer right, Number x) {
this.num = new Min();
@Override
public int inValue(){
	reutrn this.num.intValue();
	}
}
//use
Nmber y = new Between(5,9, 13);
```

cpu에게 계산하라고 하지 않았다. 단지 between()이 무엇인지 정의하고 invalue()값 계산 시점을 결정한다.

선언형의 장점

- 직접 성능 최적화를 제어할수 있기 떄문에 더 **빠르다**.
- 다형성
- 객체사이의 결합도를 낮출 뿐만 아니라 , 이 작업을 우아하게 처리할 수도 있다.→ 낮은 결합도 → 유지보수로 이어짐.
- 표현력
- 선언 방식은 결과를 이야기 하는데 명령형은 수행 가능 방식을 얘기함.
- 알고리즘과 실행(명령형) 대신 객체와 행동(선언형)의 관점에서 사고를 시작하면 뭐가 조금더 편하게 되는지 알게됨.
- 응집도
- 어떤 코드에 결합되있냐를 명령형은 코드가 조금만 커져도 알기 어려운 반면  결합성이 다르기 때문에 선언형은 쉽게 변경가능하고 보기 편하다.

### 2.3 유틸리티 클래스(Utility Class)

- 클래스의 인스턴스가 생성되는 걸 방지하기  위해 private ctor 사용

유틸리티 클래스는 어떤 것의 팩토리가 아니기 때문에 진짜 클래스라고 부를수 없다.

유틸리티 클래스는 절차적인 프로그래머들이 oop라는 것에서 나쁜 것을 모아둔 집합체이다.

유틸리티 클래스는 끔찍한 안티 패턴이다.

### 2.4 싱글톤(singleton)패턴

```java
class Math {
	private static Math INSTANCE = new Math();
	private Math() {}
	public static Math getInstance() { return Math.INSTANCE; }
public int max(int a,int b){
...
}
//대표적 싱글톤 math
```

싱글톤 패턴이 발명된 이유 = 상태를 캡슐화 할수 있기 때문에

싱글톤은 사실 끔찍한 안티 패턴이다.

싱글톤과 유틸리티 클래스의 차이점을 묻는 질문에 많은 사람은 상태 유지라고 하지만 동일한 일을 수행하는 유틸 클래스이다. 정말 차이는 싱글톤은 분리 가능 의존성으로 연결되어 있지만 유틸 클래스는 하드코딩 되어 있다는 점.

```java
math.getInstance().max()
//singleton math calss use
```

math 클래스에 결합되어 있다.

모킹이나 테스트하기 쉬운 코드로 바꾸려면

```java
Math math = new fakeMath();
math.setInstance(math)
```

위와 같이 하면 되는데 이런 이유때문에 싱글톤이 유틸 클래스 보다 좋다고 하는데  논리적 관점에서 보면 전역변수 그 이상도 이하도 아니다.

그럼 어떻게 해야 할까요? → 캡슐화

### 2.5 함수형 프로그래밍

이상적인 oop언어에는 클래스와 함께 함수가 포함되어야 한다.

작은 프로시저로 동작하는 java 의 메서드가 아니라 하나의 출구만 포함하는 FP 패러다임에 기반하는 함수를 포함해야한다.

### 2.6 조합 가능한 데코레이터

조합 가능한 데코레이터는 다른 객체를 감싸는 개체이다.

```java
names = new Sorted(
	new Unique(
		new Capitalized(
			new Replaced(
				new FileNames(
					new Directory("/var/users/*.xml");
				)
			), 
			"([^.]+)\\.xml",
			"$1"
			)
		)
	)
);
```

순수하게 객체지향적이면서도 선언형이다.

긴 메서드와 복잡한 프로시저의 사용을 최대한 자제해야 한다.

## 3. 인자의 값으로 NULL을 절대 허용하지 마세요.

당신이 어딘가에서 NULL을 사용하고 있다면 큰 실수를 하고 있는 것입니다.

OOP에서 존재하지 않는 인자 문제는 Null obejct를 이용해 해결해야 한다.

전달할 것이 없다면 비어있는 것처럼 행동하는 개체를 전달하면 된다.

항상 객체를 전달하되, 전달한 객체에게 무리한 요청을 한다면 응답을 거부하도록 구현해야 한다.

필자가 선호하는 방식은 NULL을 무시하는 것이다.

메서드는 실행 중 인자에 접근하면 NumllPointerException이 던져지고 메서드 호출자는 자신이 실수했다는 걸확인한다.는 식으로 구현한다.

중하지 않은 NULL로 코드를 오염시키지 말자. 메서드 인자로 절대 NULL을 허용하지 마라.

### 4. 충성스러우면서 불변이거나, 아니면 상수이거나

```java
class WebPage {
	private final URI uri;

	WebPage(URI path) {
		this.uri = path;
	}

	public String content() { }
}
//webPage가 가변이라고 생각하면 다시 고민해보아라.
```

content로 다른 값이 반환되더라도 불변객체이다.

불변객체라고 메서드를 호출할때마다 상수처럼 매번 동일 데이터가 반환되지는 않는다.

- 식별자 : 다른 객체와 구별할수 있는것.
- 상태 : 불변 객체를 식별하기 위한 조건, 상태를 나타내는 것.
- 행동 : 요청을 수신했을 때 객체가 할 수 있는 작업

[요약]

시스템을 포함한 어떤 종류의 시스템이라도 전체적으로 불변 객체를 이용해서 설게될 수 있고 설계뙤어야한다.

## 5. 절대 getter와 setter를 사용하지 마세요

```java
class Cash {
	private int dollars;
	public int getDollars() {
		return this.dollars;
	}
	public void setDollars(int value) {
		this.dollars = value;
	}
}
//cash는 클래스가 아니라 자료구조이다..?
```

### 5.1 객체 대 자료구조

자료구조

- 투명한 글래스 박스
- 수동적이다.
- 죽어있따.

객체

- 블랙 박스
- 능동적
- 살아있따.

객체지향적이고 선언형을 유지하려면 데이터를 객체안에 감추고 절대로 외부로 노출해서는 안된다.

캡슐화, 자료구조의 복잡성은 객체만 알고있어야한다.

객체가 선택되어야 하는 이유는 절차적인 프로그래밍 스타일을 부추기기 때문이다.

### 5.2 좋은 의도, 나쁜 결과

getter setter는 캡슐화원칙을 위반하기 위해 설계되었다?

제작의도 → 클래스를 자료구조로 바꾸기위해 도입.

getter setter를 사용하면 oop의 캡슐화 원칙을 쉽게 위반 할수 있다.

어떤 로직 정합성 방식변경을 해도 이들은 데이터이고 행동이 아닌 표현이다.

### 5.3 접두사에 관한 모든것

메서드의 이름을 통해 데이터가 표면에 드러날수 있으면 사용자는 이 데이터를 볼수 있다.

## 6. 부  ctor 밖에서는 new를 사용하지 마세요

```java
class Cash{
	private final int dollars;

	public int euro() {
		return new Exchnage().rate("USD", "EUR" ) * this.dollars;
	}
}
```

의존성이 문제이다.

cash class가 Exchage에 직접연결되어 있다.

이 것의 근본적인 원인은  new이다.

```java
class Cash {
	private final int dollars;
	Cash() { this(0); } //부 생성자

	Cash(int value) { this(value, new NYSE()); } //부 생성자

	Cash(int value, Exchange exch) { //주 생성자
		this.dollars = value;
		this.exchange = exch;
	}
}
public int euro(){
return this.exchange.rate() * this.dollar;
}
}
```

객체가 필요한 의존성을 직접 생성하는 대신, 우리가  ctor을 통해 의존성을 주입한다.

객체와 협력하는 의존성을 모두 스스로 제어할수 있게 되었다.

훌륭하게 설계하수 있는 간단한 규칙 하나

- 부 ctor을 제외한 어떤 곳에서도 new를 사용하지 마세요.
- 메서드나 주 생성자 안에 new를 사용하는 순간 뭔가 잘못됨을 떠올려라!

## 7. 인트로스펙션과 캐스팅을 피하세요.

타입 인트로스펙션은 리플렉션 기법 중 하나이다.

리플렉션은 매우 강력한 기법이지만 동시에 코드의 유지보수를 어렵게 한다.

타입에 따라 객체를 차별하게 된다.

해결방법은 메서드 오버로딩이다.

- 클래스의 사용자가 어떤 메서드를 호출할지 결정해야 한다.

[요약]

객체에 대한 기대를 문서에 적지 않은 채로 외부에 노하면 그만큼에 기대만큼 학습할수  없게 되고, 클라이언트와 객체사이가 문제가 생긴다.
