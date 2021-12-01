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