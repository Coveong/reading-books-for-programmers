# 10장. 상속과 코드 재사용

# 상속과 중복 코드

중복 코드는 우리를 주저하게 하고 동료들을 의심하게 한다.

## DRY 원칙

중복코드는 코드를 수정하는 데 필요한 노력을 몇 배로 증가시킨다.

1. 중복 코드를 찾는다.
2. 일관되게 수정한다.
3. 정상적으로 돌아가고 동일한 결과가 발생하는지 개별적으로 테스트한다. 

→ 수정과 테스트에 드는 비용을 증가시킨다.

### 중복이란?

요구사항이 변경됐을 때 두 코드를 함께 수정해야 한다면 중복이다.

생긴 것이 유사하다는 것은 중복의 징후일 뿐이다. 코드 변경에 반응하는 방식에 따라 중복 여부를 결정하는 것이다.

### DRY 원칙

Don't Repeact Yourself (반복하지 마라)

모든 지식은 시스템 내에서 단일하고, 애매하지 않고, 정말로 믿을 만한 표현 양식을 가져야 한다.

## 중복과 변경

### 타입 코드 사용하기

```java
enum PhnoeType { REGULAR, NIGHTLY }
```

낮은 응집도, 높은 결합도의 문제에 봉착한다.

## 상속을 이용해서 중복코드 제거하기

- 야간 요금제 (NightDiscountPhone)이  요금제(Phone)를 상속하면?
    - 부모의 `calculateFee()`를 호출하고, 10시 이후의 통화 요금제를 빼주도록 구현

    → 직관적이지 못하고 코드를 이해하기 어렵다.

- 상속은 결합도을 높인다.

## 강하게 결합된 Phone과 NightlyDiscountPhone

- NightlyDiscountPhone은 Phone의 `calculateFee()`를 오버라이딩한다.
    - 만약 세금을 부과하는 요구사항이 추가되면?
    - Phone : 세율(txtRate)이라는 인스턴스 변수를 추가하고, `calculateFee()`에 txtRate를 끼얹어여한다.
    - NightlyDiscountPhone : 동일하게 부모의 세율을 빼야한다.

    → 너무 강하게 결합되어있어서 또 다른 중복코드가 발생한다.

```java
상속을 위한 경고1
자식 클래스의 메서드 안에서 super 참조를 이용해 부모 클래스의 메서드를 직접 호출할 경우 두 클래스는 강하게 결합된다.
super 호출을 제거할 수 있는 방법을 찾아 결합도를 제거하라.
```

# 취약한 기반 클래스 문제

부모 클래스의 변경에 의해 자식 클래스가 영향을 받는 현상

### 상속과 캡슐화

상속은 자식 클래스가 부모 클래스의 구현 세부사항에 위존하도록 만들기 때문에 캡슐화를 약화시킨다.

객체지향의 기반 → 캡슐화를 통한 변경의 통제

상속이 캡슐화를 깨트리는 이유 → 상속 계층의 상위에 위치한 클래스에 가해지는 작은 변경만으로도 상속 계층에 속한 모든 자손들이 급격하게 요동칠 수 있다.

## 불필요한 인터페이스 상속 문제

상속을 잘못 사용한 대표적인 사례들

### Stack

Vector(List의 초기 버전)의 자식 클래스

Vector : get, add, remove 오퍼레이션 제공

Stack : push, pop 오퍼레이션 제공

→ Stack은 Vector의 오퍼레이션 사용 가능 (자식이기 때문에)

= **Stack의 규칙(맨 마지막 위치에서 요소 추가/제거 가능) 위반**

### Properties

키와 값의 타입으로 String만 가질 수 있는 HashMap의 자식 클래스

→ Hashtable의 인터페이스에 포함되어 있는 put 오퍼레이션을 이용하면 다른 타입도 Properties에 저장 가능

**= Properties의 규칙(키와 값의 타입으로 String만 가질 수 있음) 위반**

```java
상속을 위한 경고 2
상속받은 부모 클래스의 메서드가 자식 클래스의 내부 구조에 대한 규칙을 깨트릴 수 있다.
```

## 메서드 오버라이딩의 오작용 문제

ex. InstrumentedHashSet

```java
상속을 위한 경고 3
자식 클래스가 부모 클래스의 메서드를 오버라이딩할 경우 부모 클래스가 자신의 메서드를 사용하는 방법에
자식 클래스가 결합될 수 있다.
```

## 부모 클래스와 자식 클래스의 동시 수정 문제

상속은 기본적으로 부모 클래스의 구현을 재사용한다는 기본 전제를 따르기 때문에 자식 클래스가 부모 클래스의 내부에 대해 속속들이 알도록 강요한다. 

코드 재사용을 위한 상속은 부모 클래스와 자식 클래스를 강하게 결합시키기 때문에 함께 수정해야하는 상황 역시 빈번하게 발생할 수 밖에 없다.

```java
상속을 위한 경고 4
클래스를 상속하면 결합도로 인해 자식 클래스와 부모 클래스의 구현을 영원히 변경하지 않거나,
자식 클래스와 부모 클래스를 동시에 변경하거나 둘 중 하나를 선택할 수밖에 없다.
```

# Phone 다시 살펴보기

## 추상화에 의존하자

부모 클래스, 자식 클래스 모두 추상화에 의존하도록 해야한다.

코드 중복을 제거하기 위해 상속을 도입할 때 따르는 두 가지 원칙

- 두 메서드가 유사하게 보인다면 차이점을 메서드로 추출하라. 메서드 추출을 통해 두 메서드를 동일한 형태오 보이도록 만들 수 있다.
- 부모 클래스의 코드를 하위로 내리지 말고 자식 클래스의 코드를 상위로 올려라. 부모 클래스의 구체적인 메서드를 자식 클래스로 내리는 것보다 자식 클래스의 추상적인 메서드를 부모 클래스로 올리는 것이 재사용성과 응집도 측면에서 더 뛰어난 결과를 얻을 수 있다.

## 차이를 메서드로 추출하라

변하는 것으로부터 변하지 않는 것을 분리하라

- Phone과 NightlyDiscountPhone에서 다른 부분을 별도의 메서드로 추출
    - calculateFee() 로직이 서로 다으니, 동일한 이름을 가진 메서드로 추출한다.
    - 하나의 Call에 대한 통화요금이니까, calculateCallFee()로 정한다.

    ```java
    pubic Class Phone {
    	...
    	public Money calculateFee() {
    		Money result = Money.ZERO;
    		
    		for (Call call : calls) {
    			result = result.plus(calculateCallFee());
    		}
    		
    		return result;
    	}

    	private Money calculateCallFee() {
    		// Phone의 로직
    	}
    }

    pubic Class NightlyDiscountPhone {
    	...
    	public Money calculateFee() {
    		Money result = Money.ZERO;
    		
    		for (Call call : calls) {
    			result = result.plus(calculateCallFee());
    		}
    		
    		return result;
    	}

    	private Money calculateCallFee() {
    		// NightlyDiscountPhone의 로직
    	}
    }
    ```

## 중복 코드를 부모 클래스로 올려라

```java
public abstract class AbstractPhone {}
public class Phone extends AbstractPhone { ... }
public class NightlyDiscountPhone extends AbstractPhone { ... }
```

공통 코드를 추상 클래스로 옮기면 되는데, 인스턴스 변수 보다는 메서드를 먼저 이동시키는게 편하다.

→ 컴파일 에러를 통해 필요한 메서드나 인스턴스 변수를 알 수 있기 때문에!

```java
public abstract class AbstractPhone {
	private List<Call> calls = new ArrayList<>(); // 2

	public Money calculateFee() { // 1
		Money result = Money.ZERO;
		
		for (Call call : calls) {
			result = result.plus(calculateCallFee());
		}
		
		return result;
	}

	abstract protected Money calculateCallFee(Call call); // 3
}
```

이제 Phone과 NightlyDiscountPhone는 각자의 요금제를 처리하는데 필요한 인스턴스 변수와 메서드만 존재한다.

## 추상화가 핵심이다

각 클래스는 서로 다른 변경의 이유를 가진다.

- AbstractPhone
    - 전체 통화 목록을 계산하는 방법이 바뀔 경우에만 변경된다.
- Phone
    - 일반 요금제의 통화 한 건을 계산하는 방식이 바뀔 경우에만 변경된다.
- NightlyDiscountPhone
    - 심야 할인 요금제의 통화 한 건을 계산하는 방식이 바뀔 경우에만 변경된다.

⇒ 결합도가 낮아지고 응집도가 높아진다.

상속 계층이 코드를 진화시키는 데 걸림돌이 된다면 추상화를 찾아내고, 상속 계층 안의 클래스들이 그 추상화에 의존하도록 코드를 리팩터링하라.

차이점을 메서드로 추출하고 공통적인 부분은 부모 클래스로 이동하라

## 의도를 드러내는 이름 선택라기

```java
public abstract class Phone {}
public class RegularPhone extends Phone { ... }
public class NightlyDiscountPhone extends Phone { ... }
```

## 세금 추가하기

책임을 아무리 잘 분리하더라도 인스턴스 변수의 추가는 종종 상속 계층 전반에 걸친 변경을 유발한다. (txtRate 같이)

# 차이에 의한 프로그래밍

기존 코드와 다른 부분만을 추가함으로써 애플리케이션의 기능을 확장하는 방법이다.

목표 → 중복 코드 제거, 코드 재사용

상속은 코드 재사용과 관련된 대부분의 경우에 우아한 해결법이 아니다. 합성이 더 좋은 방법이다.