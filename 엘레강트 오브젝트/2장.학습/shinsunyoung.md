# 2장. 학습

# 2.1 가능하면 적게 캡슐화하세요

4개 또는 그 이하의 객체를 캡슐화 하는 것이 좋다.

→ 만약 더 많은 캡슐화를 해야한다면, 리팩토링이 필요하는 신호이다.

```java
// 3개를 캡슐화하는 객체
class Cash {
	private Integer digits;
	private Integer cents;
	private String currency;
}
```

상태 없는 객체는 존재해서는 안되고, 상태는 객체의 식별자여야 한다.

Java의 결함을 해결하기 위해 == 연산자 보다 equals() 메서드를 오버라이드하자.

# 2.2 최소한 뭔가는 캡슐화하세요

```java
class Year {
	int read() {
		// 구현 코드
	}
}
```

프로퍼티가 없는 클래스는 정적 메서드와 유사하다. (섹션 3.2 참고)

아무런 상태와 식별자도 가지지 않고 오직 행동만 포함한다. 

→ 정적 메서드가 존재하지 않고 인스턴스 생성과 실행을 엄격하게 분리하는 순수한 OOP에서는 기술적으로 프로퍼티가 없는 클래스를 만들 수 없다. 

# 2.3 항상 인터페이스를 사용하세요

객체가 많아지면 자연스럽게 객체 사이의 강한 결합도가 심각한 문제로 떠오르게 되며, 유지보수에 악영향을 끼친다.

→ 인터페이스는 다른 객체를 수정하지 않고도 해당 객체를 수정할 수 있게 만들기 위한 도구이다.

클래스 안의 모든 퍼블릭 메서드가 인터페이스를 구현하도록 만들어야 한다.

# 2.4 메서드 이름을 신중하게 선택하세요

빌더의 이름은 **명사**로, 조정자의 이름은 **동사**로 짓는것이 좋다.

### 빌더

뭔가를 만들고 새로운 객체를 반환하는 메서드. 항상 뭔가를 반환한다. (즉, void가 될 수 없다)

```java
int pow(int base, int power);
float speed();
Employee employee(int id);
String parsedCell(int x, int y);
```

### 조정자

객체를 추상화한 실세계 엔티티를 수정하는 메서드. 항상 반환 타입은 void이다.

```java
void save(String content);
void put(String key, Float value);
void remove(Employee emp);
void quicklyPrint(int id);
```

주의할 점

- 빌더와 조정자 사이에는 어떤 메서드도 존재하면 안된다.
    - ex. 뭔가를 조작한 후 반환
    - ex. 뭔가를 만드는 동시에 조작
    
    ```java
    // 저장된 전체 바이트를 반환
    int save(String content);
    
    // map이 변경된 경우 true를 반환
    boolean put(String key, Float value);
    ```
    

## 2.4.1 빌더는 명사다

무엇을 할지를 알려주어여 하며, 만들라고 요청하는 것은 옳지 않다.

```java
// bad
InputStream load(URL url);
String read(File file);
int add(int x, int y);

// good
InputStream stream(URL url);
String content(File file);
int sum(int x, int y);
```

## 2.4.2 조정자는 동사다

값을 반환하지 않는다. 

핵심적인 원칙만 준수한다면 규칙을 완화할 수도 있다. (예를 들어 빌더 패턴을 사용하는 경우에 with로 시작하는 메서드 이름을 사용하는 것)

```java
class Book {
	Book withAuthor(Stirng author)
	Book withPage(Page page)
}
```

## 2.4.3 빌더와 조정자 혼합하기

```java
class Document {
	int write(InputStream content);
}
```

빌더와 조정자가 혼합되어 사용하고 있다.

네이밍을 bytesWriteen()으로 바꾸면 → 메서드의 목적이 바이트 계산이 아니라 문서에 기록하는 것이기 때문에 부적합

```java
class Document {
	OutputPipe output();
}

class OutputPipe {
	void write(InputStream content);
	int bytes();
	long time();
}
```

연산을 수행할 객체(OutputPipe)를 준비하고, 객체는 write()를 호출하면 객체와 관련된 데이터를 모은다. 정보 수집도 가능

## 2.4.4 Boolean 값을 결과로 반환하는 경우

값을 반환하기 때문에 메서드는 빌더에 속하지만, 가독성 측면에서 형용사로 짓는게 좋다.

```java
boolean empty();
boolean readable(); 
```

# 2.5 퍼블릭 상수를 사용하지 마세요

객체들은 어떤 것도 공유하면 안 된다. 독립적이어야 하고 닫혀 있어야 한다.

```java
public class Constants {
	public static final String EOL = "\r\n";
}
```

→ 이렇게 쓰는 것도 ㄴㄴ. 결합도가 높아지고 응집도가 낮아진다.

### 2.5.1 결합도 증가

Constants 클래스에서는 이 값이 어떻게 쓰이고 있는지 알 수 있는 방법이 없다.

많은 객체들이 다른 객체를 사용하는 상황에서 서로를 어떻게 사용하는지 알 수 없다면, 객체들은 매우 강하게 결합되어 있는 것이다.

### 2.5.2 응집도 저하

객체가 자신의 문제를 해결하는데 덜 집중한다.

Constants.EOL은 자기 자신이 무엇인지 알지 못한다.

→ 기능을 공유할 수 있는 새로운 클래스를 만드는 것이 좋다.

```java
class EOLString {
	private final String origin;
	
	@Override
	String toString() {
		return String.format("%s\r\n", origin);
	}
}

// 사용
out.write(new EOLString(rec.toString()));
```

달라진 점

- 계약을 통해 추가되었기 때문에 언제라도 분리가 가능하다. 즉, 유지보수성이 증가한다.
- 계약에 따라 행동하며 내부에 계약의 의미를 캡슐화한다.

열거형도 똑같다. 사용 안하는게 좋다. 대신 클래스를 사용해라

# 2.6 불변 객체로 만드세요

불변 객체는 어떤 식으로든 자기 자신을 수정할 수 없다.

항상 원하는 상태를 가지는 새로운 객체를 생성해서 반환한다.

## 2.6.1 식별자 가변성

불변 객체는 식별자가 변하지 않으므로 식별자 가변성 문제가 없다.

## 2.6.2 실패 원자성

완전하고 견고한 상태의 객체를 가지거나 / 실패하거나 둘 중 하나의 상태만 가진다.

## 2.6.3 시간적 결합

불변 객체를 사용하면 코드 전반적으로 구문 사이에 존재하는 시간적인 결합을 제거할 수 있다.

즉, 객체는 완전한 상태로 초기화된다.

## 2.6.4 부수효과 제거

객체를 아무도 수정할 수 없기 때문에 의도치 못한 사고가 일어나지 않는다.

## 2.6.5 NULL 참조 없애기

불변 객체로 만들면 객체 안에 NULL을 포함시키는 것이 불가능하기 때문에 작고, 견고하고, 응집도 높은 객체를 생성할 수 있다.

## 2.6.6 스레드 안전성

실행 시점에 상태를 수정할 수 없기 때문에 스레드 세이프하다.

## 2.6.7 더 작고 더 단순한 객체

뭐든 단순한 객체가 좋다.

자바에서는 250줄 이내, 루비에서는 100줄 이내로 유지하는 것이 best

불변 객체는 일반적으로 가변 객체보다 크기가 작다.

# 2.7 문서를 작성하는 대신 테스트를 만드세요

깔끔하고 유지보수 가능한 단위 테스트를 추가하면 클래스를 더 깔끔하게 만들고 유지보수성을 향상시킬 수 있다.

# 2.8 목 객체 대신 페이크 객체를 사용하세요

페이크 클래스를 사용하면 테스트를 더 짧게 만들 수 있기 때문에 유지보수성이 눈에 띄게 향상된다.

모킹은

- 장황하고, 읽기 어렵다.
- 가정을 사실로 전환하기 때문에 유지보수가 어렵다.

# 2.9 인터페이스를 짧게 유지하고 스마트를 사용하세요

공통적으로 사용하는 메서드를 담고 있는 클래스를 만든다. 데코레이터 패턴과 유사하지만 스마트 클래스는 객체에 새로운 메서드를 추가하는 것, 데코레이터는 이미 존재하는 메서드를 좀 더 강력하게 만드는 점

```java
interface Exchange {
	float rate(String source, String target);
	final class Smart {
		private final Exchange origin;
		public float toUsd(String source) {
			return this.origin.rate(source, "USD");
		}
	}
}
```