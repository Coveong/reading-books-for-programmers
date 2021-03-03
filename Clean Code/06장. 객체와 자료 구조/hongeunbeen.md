# 6장 객체와 자료 구조

# 자료 추상화

```java
//구현을 외부로 노출
public class Point{
	public double x;
	public double y;
}
```

→ 구현을 외부로 노출

- 변수를 private으로 선언해도 각 값마다 조회 함수, 설정 함수 제공 → 구현을 외부로 노출하는 것

```java
//구현을 완전히 숨김
interface Point {
  double getX();
  double getY();
  void setCartesian(double x, double y); // 직교좌표계
  double getR();
  double getTheta();
  void setPolar(double r, double theta); // 극좌표계
}
```

→ 구현을 완전히 숨김에도 불구하고 인터페이스는 자료 구조를 명백하게 표현

- 클래스 메서드가 접근 정책을 강제

## 구현을 감추기 위한 추상화

변수 사이에 함수라는 계층을 넣는다고 구현이 저절로 감춰지는 것 아님

→ 구현을 감추려면 추상화가 필요함

조회 함수, 설정 함수로 변수 다룸 → 진정한 의미의 클래스 X

추상 인터페이스 제공해 사용자가 구현 모른 채 자료의 핵심 주작 → 징정한 의미의 클래스 O

```java
interface Vehicle {
  double getFuelTankCapacityInGallons();
  double getGallonsOfGasoline();
} // 구체적인 설계
```

→ 자동차 연료 상태 구체적인 숫자 값으로 제시

- 각 함수는 각 변수값을 읽어 반환할 뿐이라는 사실 알 수 있음

```java
interface Vehicle {
  double getPercentFuelRemaining();
} // 추상적인 설계
```

→ 자동차 연료 상태 백분율이라는 추상적인 개념으로 제시

- 이 연료 상태 백분율의 정보가 어디서 오는지 알 수 없음

자료를 세세하게 공개하기 보단 추상적인 개념으로 표현하는 편이 좋다.

→ 인터페이스나 조회, 설정 함수만으론 추상화 X

→ 아무 생각 없이 조회, 설정 함수를 추가하는 방법 주의하기!!

# 자료/객체 비대칭

객체는 자료 구조 사이에 벌어진 차이를 보여줌

- 객체 → 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개
- 자료구조 → 자료를 그래도 공개하며 별다른 함수 제공 X

객체와 자료구조의 정의는 본질적으로 상반적!

```java
// 절차적인 도형 클래스로 아무 메서드도 제공하지 않는다.(자료 구조)
class Square {
  Point topLeft;
  double side;
}

class Rectangle {
  Point topLeft;
  double height;
  double width;
}

class Circle {
  Point center;
  double radius;
}

public class Geometry {
  public final double PI = 3.14159265358;
  
  public double area(Object shape) throws NoSuchShapeException {
    if (shape instanceof Square square) {
      return square.side * square.side;
    } else if (shape instanceof Rectangle rectangle) {
      return rectangle.height * rectangle.width;
    } else if (shape instanceof Circle circle) {
      return PI * circle.radius * circle.radius;
    }
    
    throw new NoSuchShapeException();
  }
}
```

→ 새 함수를 추가하고 싶다면 도형 클래스에 아무 영향 없이 함수를 추가할 수 있음

```java
//객체 지향적인 도형 클래스
interface Shape {
  double area();
}

class Square implements Shape {
  Point topLeft;
  double side;
  
  public double area() {
    return side * side;
  }
}
```

→ 새 함수를 추가하고 싶다면 모든 도형 클래스를 수정해야 함

위 두개의 코드는 상호 보완적인 특징 존재

- 자료 구조를 사용하는 절차적인 코드

    → 기존 자료 구조를 변경하지 않으면서 새 함수 추가 쉬움

    → 새로운 자료 구조를 추가하기 어려움

- 객체 지향 코드

    → 기존 함수를 변경하지 않으면서 개 클래스 추가하기 쉬움

    → 모든 클래스 수정으로 새로운 함수 추가하기 어려움

→ 객체 지향 코드에서 어려운 변경은 절차적인 코드에서 쉬우며, 절차적인 코드에서 어려운 변경은 객체 지향 코드에서 쉽다.

**분별있는 프로그래머라면 모든 것이 객체라는 생각은 버리고 때로는 단순한 자료 구조와 절차적인 코드가 적합한 상황인지 판단!**

# 디미터 법칙

디미터 법칙 - 모듈이 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙

→ 객체는 조회 함수로 내부 구조를 공개하면 안된다!!

→ 하나의 코드 Line 에는 점(.) 하나만 찍어라!!

## 기차 충돌(train wreck)

```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

→ 여러 객차가 한 줄로 이어진 기차처럼 보임 = 코드가 조잡함을 의미

- ctxt 객체가 Option을 포함하고, Options가 ScratchDir을 포함하고, ScratchDir가 AbsolutePath를 포함하고 있다는 것을 외부로 공개한 셈이기 때문

```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

→ 이렇게 함수를 나누어 표현해도 함수 하나가 아는 지식이 굉장히 많음 = 함수는 많은 객체를 탐색해야 함

- 위 코드가 자료 구조 → 디미터 위반X(내부 구조를 노출해야 함)

    ```java
    final String outputDir = ctxt.options.scratchDir.absolutePath;
    //자료 구조는 무조건 함수 없이 공개 변수만 포함하는 것이므로 문제 없음
    //-> 하지만 단순한 자료 구조에도 함수 정의하라는 프레임 워크 표준 존재!
    ```

- 위 코드가 객체 → 디미터 위반(내부 구조를 숨겨야 함)

## 잡종 구조

절반은 객체 + 절반은 자료 구조 ⇒ 잡종 구조

→ 중요한 기능을 수행하는 함수 존재, 공개 변수나 공개 조회,설정 함수 존재(비공개 변수 노출)

→ 양쪽 세상에서 단점만 모아놓은 구조이므로 되도록 피하는 편이 좋다.

## 구조체 감추기

객체라면 내부 구조를 감춰야 함으로 기차 충돌 같은 코드로 작성하면 안됌

```java
ctxt.getAbsolutePathOfScratchDirectoryOption();
// ctxt 에게 공개해야 할 메서드 많음

ctx.getScratchDirectoryOption().getAbsolutePath();
//객체가 아니라 자료구조를 반환한다고 가정

String outFile = outputDir + "/" + className.replace('.','/') + ".class";
FileOutputStream fout = new FileOutputStream(outFile);
BufferedOutputStream bos = new BufferedOutputStream(fout);
//마지막 코드에서 임시 파일을 생성하기 위한 목적이라는 사실 알 수 있음

BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
//ctxt 객체에 임시파일을 생성하라고 객체에게 맡김
```

# 자료 전달 객체(DTO)

전형적인 형태는 공개 변수만 있고 함수가 없는 클래스 → DTO

- 데이터베이스와 통신 시 사용
- 소켓에서 받은 메시지 구분 분석 시 사용

→ 데이터베이스에 저장된 가공되지 않은 정보를 애플리케이션 코드에서 사용할 객체로 변환

일반적인 형태 bean 구조

```java
@RequiredArgsConstructor
@Getter
public class Address {
  private final String street;
  private final String streetExtra;
  private final String city;
  private final String state;
  private final String zip;
}
```

→ 빈은 비공개 변수를 조회/설정 함수로 조작가능 (별다른 이익 제공X)

## 활성 레코드

DTO의 특수한 형태로 공개 변수가 있거나 비공개 변수에 조회, 설정 함수가 있는 자료 구조

→ 데이터베이스 테이블이나 다른 소스에서 자료를 직접 변환한 결과

활성 레코드는 자료 구조로 취급함

→ 비즈니스 규칙을 담으면서 내부 자료를 숨기는 객체는 따로 생성

→ 활성 레코드에 비즈니스 규칙 메서드 추가해 자료 구조를 객체 취급 X

# 결론

- 객체→ 동작을 공개하고 자료를 숨김
    - 기존 동작을 변경하지 않으면서 새 객체 타입 추가 쉬움 BUT 기존 객체에 새 동작 어려움
- 자료 구조 → 별다른 동작 없이 자료를 노출
    - 새 동작을 추가 쉬움 BUT 기존 함수에 새 자료 구조 추가 어려움

→ 새로운 자료 타입을 추가하는 유연성 = 객체 적합

→ 새로운 동작을 추가하는 유연성 = 자료 구조와 절차적인 코드 적합