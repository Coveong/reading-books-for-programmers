# 3장. 취업

# 1. 5개 이하의 public 메서드만 노출하세요

public 메서드가 많을수록 클래스도 커진다. (이때 public 메서드는 protected 메서드도 포함)

클래스가 커질수록 유지보수성은 저하된다.

클래스가 작으면 유지보수 하기가 쉬워지고 응집도도 높아진다. 또한 테스트하기도 쉽고, 모든 사용 시나리오를 쉽게 재현할 수 있으며, 시나리오의 수도 많지 않다.

# 2. 정적 메서드를 사용하지 마세요

정적 메서드 대신 객체를 사용하는 것이 더 낫다.

정적 메서드는 유지보수하기 어렵게 만든다.

## 2.1 객체 대 컴퓨터 사고

객체지향적으로 생각하기에서 우리는 그저 누가 누군지 (is a)만 정의하고, 객체들이 필요할 때 스스로 상호작용 하도록 제어를 위임한다.

즉, `Number x = new Max(5, 9)` 라는 코드가 있을 때, 최대값을 계산하는 코드가 아닌, `x`가 5와 9의 최댓값이라는 사실을 정의할 뿐이다.

→ Max 클래스의 객체 안에서 포함된 것이 무엇이고, 어떻게 구현하는지는 관심사가 아니다.

OOP에서의 정적 메서드는 절자적인 코드를 작성하도록 부추길 뿐이다.

`int x = Math.max(5, 9)`

## 2.2 선언형 스타일 대 명령형 스타일

- 명령형 프로그래밍
    - 프로그래밍 상태를 변경하는 문장을 사용해서 계산 방식을 서술
- 선언형 프로그래밍
    - 제어 흐름을 서술하지 않고 계산 로직을 표현

→ 선언형 프로그래밍이 더 강력한 기법이지만, 절차적인 프로그래머가 이해하기에는 명령형 프로그래밍이 더 쉽다.

정적 메서드 방식, 객체 방식 모두 실제 if문을 감싸고 있을 뿐이다. 하지만 이 둘의 차이점은 다른 클래스, 객체, 메서드가 이 기능을 사용하는 방법에 있다.

ex. 두 개의 정수로 구성된 간격이 있고, 간격 사이에 존재하는 또 다른 정수가 있다고 가정했을 때, 정수가 간격에 포함되는지 여부를 확인하고 싶다.

```java
public static int between(int l, int r, int x) {
	return Math.min(Math.max(l, x), r);
}
```

기존의 정적 메서드들을 사용하는 또 다른 정적 메서드를 추가해야 한다. 

계산은 필요한 그 시점에 즉시 수행되기 때문에 명령형 스타일의 연산자이다.

```java
int y = Math.between(5, 9, 13); // 9를 반환 
// 메서드를 호출한 시점에 CPU가 즉시 결과를 계산
```

```java
class Between implements Number {
	private final Number num;
	
	Between(Number left, Number right, Number x) {
		this.num = new Min(new Max(left, x), right);
	}

	@Override
	public int intValue() {
		return this.num.intValue();
	}
}
```

```java
Number y = new Between(5, 9, 13); // 아직 실행되지 않음
```

계산하기 전까지 실행하지 않기 때문에 선언형 스타일이다. 

즉, 무엇인지만 정의하고 intValue()가 호출되는 시점에 값을 결정한다. 

CPU는 메서드가 실행되기 전까지는 값을 알 수 없다. → 선언형 스타일

> 선언형 방식이 명령형 방식보다 더 좋은 이유가 뭔가요?
> 
1. 속도
    - 실행 경로가 직선적이고 단순한 경우라면 명령형 방식이 빠른게 많다.
    - 하지만 다수의 정적 메서드를 호출해야되는 경우에는 선언형이 더 빠르다.
    
    ```java
    // 명령형
    public void doIt() {
    	int x = Math.between(5, 9, 13); // x의 값이 필요한지 여부와 무관하게 무조건 x를 계산
    	if (/* x가 필요한가? */) {
    		sout("x=" + x);
    	}
    }
    
    ---
    
    // 선언형
    public void doIt() {
    	Integer x = new Between(5, 9, 13); 
    	if (/* x가 필요한가? */) {
    		sout("x=" + x);
    	}
    }
    ```
    
    - 즉, 실행 관점에서 선언형 방식이 더 최적화되기 때문에 더 빠르다. 통제할 수 있는 코드가 많기 때문에 유지보수하기도 쉽다.
2. 다형성
    - 객체지향 프로그래밍에서 객체는 일급 시민이지만 정적 메서드는 그렇지 않다.
    - 생성자의 인자로 객체를 전달할 수 있지만, 정적 메서드를 전달하는 것은 불가능하다.
        - 객체는 다른 객체와 연결되고, 서로 대화하며 데이터를 주고 받는다.
        - **객체를 다른 객체로부터 완전히 분리하기 위해서는 메서드나 생성자 어디에서도 new 연산자를 사용하지 말아야 한다.**
    - ex. 숫자가 간격 안에 포함되는지 여부를 판단하는 알고리즘을 수행해야 할 때라면?
    
    ```java
    // 선언형 
    class Between implements Number {
    	private final Number num;
    	
    	Between(Number left, Number right, Number x) {
    		this.num = new Min(new Max(left, x), right);
    	}
    
    	// 이것만 추가하면 다른 알고리즘과 함께 조합해서 사용할 수 있다.
    	Between(Number number) {
    		this.num = number;
    	}
    }
    
    // 이렇게!
    Integer x = new Between(new IntegerWithMyOwnAlgorithm(5, 9, 13));
    
    ---
    
    // 명령형
    int y = Math.between(5, 9, 13); // 분리 못함. 유일한 방법은 between 메서드를 전체적으로 다시 구현허는 방법뿐
    ```
    
    → 객체 사이의 결합도를 낮출 수 있기 때문에 유지보수성이 좋아진다.
    
3. 표현력
    - 선언형 : 결과를 이야기한다.
    - 명령형 : 수행 가능한 한 가지 방법을 이야기한다.
        - 결과를 예상하기 위해서는 머릿속에서 코드를 ‘실행’해야하기 때문에 덜 직관적이다.
    
    ```java
    // 명령형
    Collection<Integer> evens = new LinkedList<>();
    for (int number : numbers) {
    	if (number % 2 == 0) {
    		evens.add(number);
    	}
    }
    // 코드의 실행 경로를 추적해야한다.
    // CPU가 실행해야 하는 일을 사람도 동일하게 수행해야 한다.
    
    ---
    
    // 선언형
    Collection<Integer> evens = new Filtered(
    	numbers,
    	new Pedicate<Integer>() {
    		@Override
    		public boolean suitable(Integer number) {
    			return number % 2 == 0;
    		}
    	}
    );
    // 코드보다는 영어를 읽는 느낌이 강하다.
    // Filtered 클래스가 어떻게 생성하는지 모르지만, 컬렉션이 "필터링"되었다는 사실은 안다.
    
    // 또는 아래처럼 압축할 수도 있다.
    Collection<Integer> evens = new Filtered(
    	numbers,
    	{ Integer number -> number % 2 == 0 }
    );
    ```
    
    - **알고리즘과 실행** 대신 **객체와 행동**에 관점에서 사과해야한다.
    - 명령형 스타일은 알고리즘과 실행을 다루는 방법
    - 선언형 스타일은 객체와 행동에 관한 방법
4. 코드 응집도
    - 위의 예제를 보면 컬렉션의 “계산"을 책임지는 모든 코드들은 한 곳(Filtered)에 뭉쳐있다. 하지만 선언형 메서드는 아니다.

- 명령형 + 선언형 ⇒ 명령형 스타일을 따르게 된다.

```java
🧐 만약 이미 선언형 메서드로 짜져있는 외부 라이브러리를 사용하게 된다면?
```

객체가 직접 처리할 수 있도록 정적 메서드를 감싸는 클래스를 만든다.

```java
class FileLines implements Iterable<String> {
	private final file file;
	public Iterator<String> iterator() {
		return Arrays.asList(FileUtils.readLines(this.file)).iterator();
	}
}

// 사용할땐
Iterable<String> lines = new FileLines(f);
// -> 정적 메서드는 FileLines 클래스 안에만 남게 된다.
```

## 2.3 유틸리티 클래스

얘도 별로 안좋다.

그나마 인스턴스를 생성하는 것을 방지하기 위해 private ctor을 만들자.

## 2.4 싱글톤 패턴

정적 메서드 대신 사용하는 개념

```java
class Math {
	private static Math INSTANCE = new Math();
	private Math() {}

	// 인스턴스에 접근할 수 있는 유일한 방법
	public statuc Math getInstance() {
		return Math.INSTANCE;
	}

	public int max(int a, int b) {
		if (a < b) {
			return b;
		}

		return a;
	}
}
```

안티패턴이다.

정적 메서드와 다르지 않다.

싱글톤 패턴은 정적 메서드와 다르게 상태를 캡슐화 할 수 있다고 하지만 별 다를게 없다. 전역 변수 그 이상도 이하도 아니다.

중요한건 캡슐화이다.

# 3. 인자의 값으로 NULL을 절대 허용하지 마세요

각각의 객체가 자신의 행동을 온전히 책임져야 한다.

null을 인자의 값으로 허용하는 순간 하나의 분기가 필요해진다.

```java
if (mask == null) {
	// ...
} else {

}
```

Mask 객체에게 이야기하는 대신, 이 객체를 피하고 무시합니다. 즉, 존중하지 않는 것이다.

- null 체크 후 예외를 던지거나,
- 그냥 null을 무시하고 NPE가 발생하게 하자

# 4. 충성스러우면서 불변이거나, 아니면 상수이거나

- 객체
    - 실제 엔티티의 대표자 (디스크의 파일, 웹 페이지, 바이트 배열 …)
    
    ```java
    // f는 디스크에 저장되어 있는 파일을 대표한다. 상태는 /tmp/test.txt 
    File f = new File("/tmp/test.txt");
    ```
    
    - 기본적으로 모든 객체는 식별자, 상태, 행동을 포함한다.
    - 불변 객체와 가변 객체의 중요한 차이는
        - 불변 객체는 **식별자**가 존재하지 않으며,
        - 불변 객체의 **상태를 절대 변경할 수 없다.**
        
        → 불변 객체의 식별자는 객체의 상태와 동일하다.
        
        → 즉, equals(), hashCode()는 필요 없는 메서드이다.
        

# 5. 절대 getter와 setter를 사용하지 마세요

getter와 setter가 존재하는 클래스는 객체가 아닌 자료구조이다. 

자료구조는 속이 보이는 글래스 박스(glass box)이지만, 객체는 블랙 박스(black box)이다.

캡슐화를 하기 위해 만들었지만, 결국에는 캡슐화를 하는 척하기 위해 만들어진 것일 뿐이며, 달라지는게 없다.

네이밍에도 문제가 있다. prefix가 get/set이 되었기 때문에 객체가 아닌 자료구조로 보는 것

- getDollars()
    - 데이터 중에 dollars를 찾은 후 반환하세요
- dollars()
    - 얼마나 많은 달러가 필요한가요?

# 6. 부 ctor 밖에서는 new를 사용하지 마세요

```java
class Cash {
	private final int dollars;

	public int euro() {
		return new Exchange().rate("USD", "EUR") * this.dollars;
	}
}
```

Cash 클래스가 Exchange 클래스에 직접 연결되어있다.

둘 사이의 결함을 끊기 위해서 새로운 객체를 ctor의 인자로 전달받아 private 프로퍼티 안에 캡슐화하자

```java
class Cash {
	private final int dollars;
	private final Exchange exchange;

	Cash(int value, Exchange exch) {
		// ...
	}

	public int euro() {
		return this.exchange.rate("USD", "EUR") * this.dollars;
	}
}
```

→ ctor의 두 번째 인자에 Exchange 인스턴스를 전달해야하고, Cash에서 직접 생성하지 않는다. Cash는 Exchange에게 의존은 하지만, 의존성을 제어하는 주체가 Cash에서 우리 자신으로 바뀌었다.

new를 합법적으로 사용할 수 있는 곳은 부 ctor뿐이다.

```java
class Cash {
	private final int dollars;
	private final Exchange exchange;

	Cash() { // 부 ctor
		this(0);
	}

	Cash(int value) { // 부 ctor
		this(value, new NYSE());
	}

	Cash(int value, Exchange exch) { // 주 ctor
		// ...
	}

	public int euro() {
		return this.exchange.rate("USD", "EUR") * this.dollars;
	}
}
```

# 7. 인트로스펙션과 캐스팅을 피하세요

instanceOf와 클래스 캐스팅 둘 다 사용하지 말자

객체의 인자의 타입에 따라 차별하는 것과 마찬가지이다.