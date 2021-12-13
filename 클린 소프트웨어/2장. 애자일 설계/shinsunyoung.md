# 2. 애자일 설계

# 7. 애자일 설계란 무엇인가?

설계는 다양한 매체로 표현될 수 있지만, 최종적인 구현은 소스 코드가 된다. 결국, 소스 코드가 바로 설계다.

## 설계의 악취: 부패하고 있는 소프트웨어의 냄새

1. 경직성 : 시스템을 변경하기 어렵다. 하나를 변경하려면 다른 부분도 많이 변경해야 한다.
2. 취약성 : 변경을 하면 개념적으로 관련 없는 부분이 망가진다.
3. 부동성 : 시스템을 다른 시스템에서 재사용할 수 있는 컴포넌트로 구분하기 어렵다.
4. 점착성 : 옳은 동작을 하는 것이 잘못된 동작을 하는 것보다 더 어렵다.
5. 불필요한 복잡성 : 직접적인 효용이 전혀 없는 기반구조가 설계에 포함되어 있다.
6. 불필요한 반복 : 단일 추상 개념으로 통합할 수 있는 반복적인 구조가 설계에 포함되어 있다.
7. 불투명성 : 읽고 이해하기 어렵다. 그 의도를 잘 표현하지 못한다.

## 무엇이 소프트웨어의 부패를 촉진하는가?

계속되는 요구사항 변경 때문에 설계가 실패한다면, 우리의 설계와 방식에 문제가 있는 것이다.

이런 변경이 대해서도 탄력적인 설계를 만드는 방식을 찾아야 하고, 그것이 부패하지 않도록 보호할 수 있는 방식을 사용해야 한다.

## Copy 프로그램

> 키보드에서 프린터로 문자를 복사하는 프로그램을 만들어라!
> 

### 초기설계

                              Copy
    
              —————————

  char⬆      |                            | ⬇️char

Read Keyboard                   Write Printer

```java
void Copy() {
	int c;
	while ((c = Rdkbd()) != EOF)
			WrtPrt(c);
}
```

잘 동작한다.

### 요구사항 변경

> Copy 프로그램이 종이테이프 판독기에서도 문자를 읽을 수 있었으면 좋겠다.
> 

```java
bool ptFlag = false; // 종이테이프 판독을 위한 플래그 값
void Copy() {
	int c;
	while ((c (ptFlag ? RdPt() : = Rdkbd())) != EOF)
			WrtPrt(c);
}
```

종이테이프 판독기에서 입력을 받길 원하면 true,  끝나면 꼭 false로 바꿔야한다.

### 한 치를 주니 ...

> Copy 프로그램으로 종이테이프 천공기에 출력을 하고 싶다.
> 

```java
bool ptFlag = false; 
bool punchFlag = false; // 천공기 출력을 위한 플래그 값

void Copy() {
	int c;
	while ((c (ptFlag ? RdPt() : = Rdkbd())) != EOF)
			punchFlag ? WrtPunch(c) : WrtPrt(c);
}
```

### 변화를 예상하라

두 번의 변경만에 경직성, 취약성, 부동성, 복잡성, 반복, 불투명성의 신호를 보여주게 되었다.

요구사항은 언제나 바뀐다. 만약 설계가 요구사항 변경 때문에 퇴화한다면, 우리는 애자일 방식대로 일하고 있지 않은 것이다.

## Copy 프로그램의 애자일 설계

탄력적으로 수용할 수 있게 설계를 해보자

```java
class Reader {
	public : virtual int read() = 0;
}

class KeyboardReader : public Reader {
	public : virtual int read() {
		return RdKbd();
	}
}

KeyboardReader GdefaulthReader;

void Copy(Reader & reader = GdefaulthReader) {
	int c;
	while ((c = reader.read()) != EOF)
		WrtPrt(c);
}
```

애자일 팀은 개방 폐쇄 원칙을 따른다.

## 애자일 개발자는 해야 할 일을 어떻게 알았는가?

초기 설계는 Copy 모듈이 KeyboardReader와 PrinterWriter에 직접 의존했다. 

Copy 모듈은 상위 수준의 모듈은, 애플리케이션의 정책을 결정하고 문자를 복사하는 방법을 알고 있다.

→ 하위 수준의 세부 사항이 바뀔 때, 상위 수준의 정책이 영향을 받게 된다.

즉, Copy 모듈에서 입력 장치로 향하는 의존성을 거꾸로 뒤집어 Copy가 입력 장치에 의존하지 않도록 해야한다.

## 가능한 한 좋은 상태로 설계 유지하기

설계는 명료한 상태로 유지되어야 한다.





# 8. 단일 책임 원칙(SRP)

## 단일 책임 원칙(SRP)

> 한 클래스는 단 하나의 변경 이유만을 가져야 한다.

볼링 게임을 생각해보자. Game 객체는 두 가지 책임을 가지고 있었다.

1. 현재 프레임을 기억하는 것
2. 스코어를 계산하는 것

→ 2번을 Scorer가 맡게 바꾸었다.

책임은 변경의 축이다. 한 클래스가 하나 이상의 책임을 맡는다면, 그 클래스를 변경할 하나 이상의 이유가 있을 것이다.

예를 들어 Rectangle이라는 클래스가

1. 화면에 직사각형을 그리는 것
2. 직사각형의 넓이를 계산하는 것

의 두 가지 책임을 가지고 있으면, SRP를 위반하는 것이다.

차라리 GeometricRanctangle이라는 객체를 만들어서 넓이를 계산하는 책임을 넘겨주자!

## 책임이란 무엇인가?

```java
interface Modem {
	public void dial(String pno);
	public void hangup();
	public void send(char c);
	public char recv();
}
```

언뜻보면 문제가 없지만, 사실 문제가 있다.

dial, hangup → 모뎀의 연결을 관리

send, recv → 데이터를 주고받으며 통신

```java
interface DataChannel {
	public void send(char c);
	public char recv();
}

interface Connection {
	public void dial(String pno);
	public void hangup();
}
```

이렇게 나누는게 낫다.

단, 서로 다른 시간에 두 가지 책임의 변경을 유발하는 방식으로 바뀌지 않는다면 분리할 필요는 없다. (오히려 불필요한 복잡성이 될 수도 있다.)

변경의 축은 변경이 실제로 일어날 때만 변경의 축이다.

## 영속성

![](img/s_1.png)

이 두 책임은 어울리지 않는다.

업무 규칙은 자주 바뀌고, 영속성은 자주 바뀌지 않는 경향이 있지만, 바뀌는 이유도 완전히 다르다.

업무 규칙과 영속성 서브 시스템을 묶는 것은 문제를 부르는 것이다.

## 결론

책임을 찾고 하나씩 분리하는 것이 설계에서 하는 일의 대부분이다. 이후에 논의할 나머지 원칙들에서도 어떤 식으로든 이 문제로 돌아온다.





# 9. 개방 폐쇄 원칙 (OCP)

## 개방 폐쇄 원칙 (OCP)

> 소프트웨어 개체(클래스, 모듈, 함수 등)는 확장에는 열려 있어야 하고, 수정에는 닫혀 있어야 한다.

## 상세 설명

1. 확장에 대해 열려 있다.
   - 애플리케이션의 요구사항이 변경될 때, 변경에 맞게 새로운 행위를 추가해 모듈을 확장할 수 있다.
2. 수정에 대해 닫혀 있다.
   - 모듈의 행위를 확장하는 것이 소스 코드나 바이너리 코드의 변경을 초래하지 않는다.

어떻게 소스 코드를 변경하지 않고도 그 모듈의 행위를 바꾸는 일이 가능할까?

어떻게 모듈을 변경하지 않을 채로, 모듈이 하는 일을 변경할 수 있을까?

## 해결책은 추상화다

모듈은 추상화를 조작할 수 있다. 고정된 추상화에 의존하기 때문에 수정에 대해 닫혀 있을 수 있다. 그 모듈의 행위는 추상화의 새 파생 클래스를 만듦으로써 확장이 가능하다.

Client → Server

[개방 폐쇄 원칙에 어긋난다.]

→ Client 객체가 다른 서버 객체를 사용하게 하려면, Client 클래스가 새로운 서버 클래스를 지정해도록 변경해야 한다.

Client → (interface) Client Interface ← Server

[개방 폐쇄 원칙을 따른다.]

→ Client 객체가 다른 서버의 클래스를 사용하게 하려면, ClientInterface 클래스의 새 파생 클래스를 생성하면 된다. Client 클래스는 변경되지 않은 채로 남는다.

테템플릿 메서드 패턴 기반 클래스는 개방 폐쇄 원칙을 따른다.

## Shape 애플리케이션

```java
// shape.h
enum ShapeType {circle, square}
struct Shpae {
	ShapeType itsType;
}

// circle.h
struct Circle {
	ShapeType itsType;
	double itsRadius;
	Point itsCenter;
}

void DrawCircle(struct Circle*)

// square.h
struct Square{
	ShapeType itsType;
	double itsSide;
	Point itsTopLeft;
}

void DrawSquare(struct Square*);

// drawAllShapes.cc
typedef struct Shape *ShapePointer;

void DrawAllShapes(ShapePointer list[], int n) {
	int i;
	for (i = 0; i < n; i++) {
		struct Shape* s = lists[i];
		switch (s -> itsType) {
			case square:
				DrawSquare((struct Square*) s);
				break;
			case circle:
				DrawCircle((struct Circle*) s);
				break;
		}
	}
}
```

[OCP를 따르지 않는다.]

- 새롭게 그려야할 모든 도형에 대해 함수를 수정해야한다.
  - Triangle이 추가되면 Shape, Square, Circle, DrawAllShapes를 재컴파일, 재배포를 해야한다.
- 이런 함수에는 switch/case나 if/else 같은 사슬이 없어야 한다.

### OCP 따르기

```java
class Shape {
	public : virtual void Draw() const = 0;
}

class Square : public Shape {
		public : virtual void Draw() const
}

class Circle : public Shape {
		public : virtual void Draw() const
}

void DrawAllShapes(vector<Shape*> & list) {
	vector<Shape*> :: iterator i;
	for (i = list.begin(); i != list.end(); i++)
		(*i) -> Draw();
}
```

[OCP를 따른다.]

- 새로운 도형을 그리고 싶다면 Shape의 새로운 파생 클래스를 만들면 된다.
- 경직성이 없다.
  - 수정해야하는 소스 모듈도 없고, 하나만 제외하면 재빌드되어야 하는 바이너리 모듈도 없다.
- 부동성이 없다.
  - 재사용이 가능하다.

## 그래, 거짓말했다

사실 이 예는 터무니 없는 소리이다. Circle이 모두 Square 앞에 그려지도록 결정한다고 했을때, 이런 변경에 대해 닫혀 있지 않다.

## 예상과 '자연스러운' 구조

모듈이 얼마나 닫혀 있든지, 열려있든지 간에 변경은 항상 존재한다. 모든 상황에서 자연스러운 모델은 없다.

폐쇄는 완벽할 수 없기 때문에, 전략적이여야 한다.

## '올가미' 놓기

추상화가 실제로 필요할 때까지 기다렸다가 올가미를 놓는 편이 차라리 낫다.

첫 번째 총알은 그냥 맞고, 그 총에서 쏘는 다른 총알에 대해서는 확실히 보호하자!

## 폐쇄를 위해 '데이터 주도적' 접근 방식 사용하기

Shpae의 파생 클래스가 서로에 대해 아는 것을 막는다면, 테이블 주도적 접근 방식을 사용할 수 있다.

## 결론

많은 면에서 OCP는 객체 지향 설계의 심장이다. 어설픈 추상화를 피하는 일은 추상화 자체만큼이나 중요하다.



# 10. 리스코프 치환 원칙(LSP)

## 리스코프 치환 원칙(LSP)

> 서브타입(subtype)은 그것의 기반 타입(base type)으로 치환 가능해야 한다.

> 타입 S의 각 객체 a1과 타입 T의 각 객체 o2가 있을 때, T로 프로그램 P를 정의했음에도 불구하고, o2를 o1로 치환할 때 P의 행위가 변하지 않으면, S는 T의 서브타입이다.

## LSP 위반의 간단한 예

LSP 위반은 잠재적인 OCP 위반이다.

## 정사각형과 직사각형, 좀 더 미묘한 위반

직사각형(rectangle) 클래스가 있다고 생각해보자.

이 때 이런 요구가 들어왔다.

> 정사각형(square)도 조작할 수 있게 해주세요.

상속은 IS-A 관계라고 한다.

일반적인 개념상, 모든 정사각형은 직사각형이다. 그러므로 정사각형이 직사각형을 상속받아도 문제가 없다.

하지만 정사각형은 가로, 세로 길이가 따로 필요없다. (모두 같기 때문에)

- 하지만 직사각형은 필요하다. 그러므로 정사각형은 가로, 세로 길이를 같이 상속받아야한다.

→ 자원 낭비로 이어진다.

## 유효성은 본래 갖추어진 것이 아니다

LSP는 '모델만 별개로 보고, 그 모델의 유효성을 충분히 검증할 수 없다'라는 결론을 내린다.

어떤 모델의 유효성은 오직 그 고객의 관점에서만 표현된다.

→ 직사각형, 정사각형을 고객이 아닌 프로그래머의 관점에서 봐보자! 하지만 이것들을 모두 예상하려면 불필요한 복잡성이 생길 수도 있다. 취약성의 악취를 맡을 때까지 LSP 위반을 제외한 나머지 처리는 연기하자.

## IS-A는 행위에 대한 것이다

정사각형, 직사각형의 예를 다시 생각해보자.

g의 관점에서 정사각형은 절대로 직사각형 객체가 아니다.

정사각형의 행위가 g가 기대하는 직사각형 객체의 행위와 일치하지 않기 때문이다.

## 결론

이 원칙이 효력을 가질 때, 애플리케이션은 좀 더 유지보수 가능하고, 재사용 가능하고, 견고해진다. LSP는 OCP를 가능하게 만드는 주요 요인 중 하나다.

IS-A라는 용어는 서브타입의 정의가 되기에는 의미가 지나치게 넓다. 서브타입의 진실된 정의는 치환 가능성이다.

# 11. 의존관계 역전 원칙(DIP)

## 의존관계 역전 원칙(DIP)

1. 상위 수준의 모듈은 하위 수준의 모듈에 의존해서는 안 된다. 둘 모두 추상화에 의존해야 한다.
2. 추상화는 구체적인 사항에 의존해서는 안 된다. 구체적인 사항은 추상화에 의존해야 한다.

하위 수준의 구체적인 모듈에 영향을 주어야 하는 것은 정책을 결정하는 상위 수준의 모듈이다. 업무 규칙을 포함하는 상위 수준의 모듈은 구체적인 구현을 포함한 모듈에 우선하면서 동시에 독립적이어야 한다. 상위 수준의 모듈은 어떤 식으로든 하위 수준의 모듈에 의존해서는 안 된다.

## 레이어 나누기

[168p]

Policy Layer → Mechanism Layer → Utility Layer

언뜻보면 괜찮아보이지만, 사실은 별로다. Policy 레이어는 Utility 레이어에 의존하는 다른 것에도 의존한다. 즉, Policy 레이어는 Utility 레이어에 이행적으로 의존한다.

좀 더 괜찮은 방법은,

Policy Layer → (interface) Policy Service Interface

```
                                         👆 Mechanism Layer →(interface) Mechanism Service Interface 

                                                                                                      👆 Utility Layer
```

하위 수준의 레이어는 추상 인터페이스로부터 실체화된다. 상위 수준 레이어는 하위 수준 레이어에 의존하지 않는다.

### 소유권의 역전

DIP가 적용된 경우에는 클라이언트가 추상 인터페이스를 소유하는 경향이 있고, 이들의 서버가 그것에 파생해 나온다는 사실을 알게 된다.

- PoliyLayer는 Mechanism, Utility의 변경에 영향을 받지 않는다.
- PoliyLayer는 Policy Service Interface에 맞는 하위 수준 모듈을 정의하는 어떤 문맥에서든 재사용될 수 있다.

→ 유연하고, 튼튼하고, 이동이 쉬운 구조를 만들 수 있다.

### 추상화에 의존하자

구체 클래스에 의존하지 말고, 어떤 프로그램의 모든 관계는 어떤 추상 클래스나 인터페이스에서 맺어져야 한다.

- 어떤 변수도 구체 클래스에 대한 포인터나 참조값을 가져선 안 된다.
- 어떤 클래스도 구체 클래스에서 파생되어서는 안 된다.
- 어떤 메서드도 그 기반 클래스에서 구현된 메서드를 오버라이드해서는 안 된다.

## 결론

프로그램의 의존성이 역전되어 있다면 객체 지향 설계,

아니라면 절차적 설계이다.

# 12. 인터페이스 분리 원칙(ISP)

## 인터페이스 분리 원칙(ISP)

> 클라이언트가 자신이 사용하지 않는 메서드에 의존하도록 강제되어서는 안 된다

## ATM 사용자 인터페이스 예

UI 인터페이스를 개별적인 인터페이스로 분리하자.

## 결론

비대한 클래스는 클라이언트 간에 기이하고 해가 되는 결합도를 유발한다. 클라이언트 고유의 각 인터페이스는 자신의 특정한 클라이언트나 클라이언트 그룹이 호출하는 함수만 선언한다.

클라이언트는 서로 독립적이어야 한다.