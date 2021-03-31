# 06장. 메시지와 인터페이스

객체지향의 가장 중요한 재료는 클래스가 아닌 **객체들이 주고받는 메시지**이다. 클래스 사이의 정적인 관계에서 메시지 사이의 동적인 흐름으로 초점을 전환해야 한다.

# 협력과 메시지

## 클라이언트-서버 모델

객체 사이의 협력 관계를 설명하기 위해 사용하는 전통적인 메타포는 **클라이언트-서버 모델**이다. 협력은 클라이언트가 서버에게 요청하는 **단방향 상호작용**이다.

- 클라이언트
    - 메시지를 전송하는 객체
- 서버
    - 메시지를 수신하는 객체

큰 책임을 수행하기 위해서는 다른 객체와 협력해야 한다. 협력의 매개체는 메시지이다.

## 메시지와 메시지 전송

### 메시지

- 객체들이 협력하기 위해 사용할 수 있는 **유일한 의사소통 수단**
- 메시지 전송, 메시지 패싱
    - 한 객체가 다른 객체에게 도움을 요청하는 것
- 메시지 전송자
    - 메시지를 전송하는 객체
- 메시지 수신자
    - 메시지를 수신하는 객체

### 메시지의 구성

- 오퍼레이션명
- 인자

### 메시지 전송의 구성

- 오퍼레이션명
- 인자
- 메시지 수신자

```java
condition.isSatisfiedBy(screening);
수신자.오퍼레이션명(인자)
```

## 메시지와 메서드

### 메서드

- 실제로 실행되는 **함수 또는 프로시저**
- 동일한 이름의 변수에게 동일한 메시지를 전송하더라도 객체의 타입에 따라 실행 메서드가 달라질 수도 있다. → 컴파일 시점과 실행 시점의 의미가 달라질 수도 있다.

[메시지 전송자]  —메시지— [메시지 수신자]

## 퍼블릭 인터페이스와 오퍼레이션

### 퍼블릭 인터페이스

- 외부에 공개하는 메시지의 집합

### 오퍼레이션

- 퍼블릭 인터페이스에 포함된 메시지
- 보통 내부의 구현코드는 제외하고 단순히 메시지와 관련된 시그니처를 가르킨다.
    - ex. DiscountCondition 인터페이스에 정의된 isSatisfiedBy()

### 메서드

- 메시지를 수신했을 때 실제로 실행되는 코드
    - ex. PeriodCondition 클래스에 정의된 isSatisfiedBy()
- **오퍼레이션에 대한 구현**이라고 할 수 있다.

## 시그니처

- 오퍼레이션의 이름과 파라미터 목록을 합친 것
- 오퍼레이션은 실행 코드 없이 **시그니처만 정의한 것**이다.

# 인터페이스와 설계 품질

### 좋은 인터페이스의 조건

- 최소한의 인터페이스
- 추상적인 인터페이스
    - '어떻게' 수행하는지가 아닌, '무엇'을 하는지 표현

→ **책임 주도 설계 방법**을 따르면 된다.

### 퍼블릭 인터페이스의 품질에 영향을 미치는 원칙과 기법들

- 디미터 법칙
- 묻지 말고 시켜라
- 의도를 드러내는 인터페이스
- 명령-쿼리 분리

## 디미터 법칙

- 객체의 내부 구조에 강하게 결합되지 않도록 **협력 경로를 제한**하라는 법칙
- "낯선 자에게 말하지 말라"
- "오직 인접한 이웃하고만 말하라"
- "오직 하나의 도트만 사용하라"
    - ex. sunyoung.getName().getFirstName() (X)

클래스 내부의 메서드가 아래 조건을 만족하는 인스턴스에만 메시지를 전송하자!

- this 객체
- 메서드의 매개변수
- this의 속성
- this의 속성인 컬렉션의 요소
- 메서드 내에서 생성된 지역 객체

하지만 무비판적으로 디미터 법칙을 수용하면 퍼블릭 인터페이스 관점에서 객체의 응집도가 낮아질 수도 있다. "원칙의 함정"쪽을 읽어보자!

## 묻지말고 시켜라

- 메시지 수신자의 상태를 기반으로 결정을 내린 후 메시지 수신자의 상태를 바꾸면 안된다. 그것은 수신자가 담당해야할 책임이다. 객체의 외부에서 해당 객체의 상태를 기반으로 결정을 내리는 것은 객체의 캡슐화를 위반한다.
- 상태를 묻는 오퍼레이션을 **행동을 요청하는 오퍼레이션**으로 대체하자

## 의도를 드러내는 인터페이스

### 메서드를 명명하는 두 가지 방법

1. 메서드가 작업을 어떻게 수행하는지 나타내도록 이름을 짓는 것
    - ex. isSatisfiedByPeriod()
    - 좋지 않다. 메서드에 대해 제대로 커뮤니케이션 하지 못하고 캡슐화를 위반하기 때문이다.
2. '어떻게'가 아니라 '무엇'을 하는지 드러내는 것
    - ex. isSatisfiedByPeriod() → DiscountCondition 인터페이스로 묶고 → isSatisfiedBy()

    ```jsx
    public class PeriodCondition implements DiscountCondition {
    	public boolean isSatisfiedBy(Screening screening) {...}
    	public boolean isSatisfiedBy(Screening screening) {...}
    }
    ```

    - 이해하기 쉽고 유연한 코드를 만들 수 있다.
    - **의도를 드러내는 선택자(Intention Revealing Selector)**라고 한다.

하나의 구현을 가진 메시지의 이름을 일반화하도록 도와주는 간단한 훈련 방법
매우 다른 두 번째 구현을 상상하라. 그리고 해당 메서드에 동일한 이름을 붙인다고 상상해보라. 그렇게 하면 아마도 가장 추상적인 이름을 메서드에 붙일 수 있을 것이다.

### 의도를 드러내는 인터페이스

의도를 드러내는 선택자를 인터페이스 레벨로 확장하는 방법

구현과 관련된 모든 정보를 캡슐화하고 객체의 퍼블릭 인터페이스에는 협력과 관련된 의도만을 표현하는 것

수행 방법에 관해서는 언급하지 말고 결과와 목적만을 포함하도록 클래스와 오퍼레이션의 이름을 부여하라

## 함께 모으기

### 디미터 법칙 위반

디미터 법칙을 위반하는 설계는 **인터페이스와 구현의 분리 원칙**을 위반한다.

### 묻지 말고 시켜라

```jsx
// 물어보고 시키는 중 (👎)
public class Audience {
	public Long setTicket(Ticket ticket) {
		if (bag.hasInvitation()) {
			bag.setTicket(ticket);
			return 0L;
		} else {
			bag.setTicket(ticket);
			bag.minusAmount(ticket.getFee());
			return ticket.getFee();
		}
	}
}

// 묻지 말고 시키는 중 (👍)
public class Audience {
		public Long setTicket(Ticket ticket) {
			return bag.setTicket(ticket);
		}
}

public class Bag {
		public Long setTicket(Ticket ticket) {
		if (hasInvitation()) {
			this.ticket = ticket;
			return 0L;
		} else {
			this.ticket = ticket;
			minusAmount(ticket.getFee());
			return ticket.getFee();
		}
	}
}
```

### 결과

디미터 법칙 + 묻지 말고 시켜라 스타일 = 자율적인 객체로 구성된 유연한 협력을 얻게 된다.

### 인터페이스에 의도를 드러내자

- Theater -`setTicket()`→ TicketSeller
    - Audience에게 티켓을 판매하는 결과를 기대하고 있다.
    - 즉, `sellTo()`가 더 적절한 이름이다.
- TicketSeller -`setTicket()`→ Audience
    - Audience가 티켓을 사는 행동을 기대하고 있다.
    - 즉 `buy()`가 더 적절한 이름이다.
- Audience -`setTicket()`→ Bag
    - 티켓을 보관하는 것이 목적이다.
    - 즉,  `hold()`가 더 적절한 이름이다.
- 오퍼레이션은 클라이언트가 객체에게 무엇을 원하는지를 표현해야 한다.
    - 객체 자신이 아닌 **클라이언트의 의도를 표현하는 이름을 가져야한다**.

# 원칙의 함정

원칙이 현재 상황에 부적합하다고 판단되면 과감하게 원칙을 무시하면 된다. 원칙을 아는 것 보다 언제 원칙이 유용하고 언제 유용하지 않은지 판단할 수 있는 능력을 길러야한다.

## 디미터 법칙은 하나의 도트(.)를 강제하는 규칙이 아니다.

```jsx
// 디미터 법칙을 준수한 코드이다!
IntStream.of(1, 15, 20, 3, 9)
	.filter(x -> x > 10)
	.distinct()
	.count();
```

기차 충돌처럼 보이는 코드라도 객체 내부 구현의 정보를 외부로 노출시키지 않는다면 디미터 법칙을 준수한 것이다.

## 결합도와 응집도 충돌

묻지말고 시켜라 + 디미터 법칙을 무작정 준수하다가는 객체가 상관 없는 책임들을 떠맡을 수도 있다.

```jsx
// 🤔 아뉘!! Screening의 내부 상태를 이렇게 가져다쓰면 안되는거 아닌가?! 캡슐화 위반이잖아!
public class PeriodCondition implements DiscountCondition {
	public boolean isSatisfiedBy(Screening screening) {
		return screening.getStartTime().getDayOfWeek().equals(dayOfWeek) &&
			startTime.compareTo(screening.getStartTime().toLocalTime()) <= 0 &&
			endTime.compareTo(screening.getStartTime().toLocalTime()) >= 0 &&
	}
}

// 🤗 음 ~ 만족스럽군
public class Screening {
	public boolean isDiscountable(DayOfWeek dayOfWeek, LocalTime startTime, LocalTime endTime) {
		return whenScreend.getDayOfWeek().equals(dayOfWeek) &&
			startTime.compareTo(whenScreend.toLocalTime()) <= 0 &&
			endTime.compareTo(whenScreend.toLocalTime()) >= 0 &&
	}
}

public class PeriodCondition implements DiscountCondition {
	public boolean isSatisfiedBy(Screening screening) {
		return screening.isDiscountable(dayOfWeek, startTime, endTime);
	}
}
```

이렇게 하면 Screeing(상영)이 기간에 따라 할인 조건을 판단하는 책임을 떠안게 된다. 

→ 상관 없는 책임을 떠맡게 된다.

이 책임은 PeriodCondition에게 판단하도록 맡기는 것이 훨씬 본질적이다. 심지어 저렇게 리팩토링을 하게 되면 isDiscountable()이 PeriodCondition이 인스턴스 변수 (startTime, endTime)를 인자로 받게 되기 때문에 인스턴스 변수 목록이 변경되면 영향을 받는다. 즉, 두 객체의 결합도가 높아지게 된다.

이럴 때에는 캡슐화를 향상 시키는 것 보다, 응집도를 높이고 결합도를 낮추는 것이 전체적인 관점에서 더 좋은 방법이다.

### 디미터 법칙 적용은 언제?

묻는 대상이 객체인지, 자료 구조인지에 따라 달려있다.

- 객체 : 디미터 법칙을 따르자!
    - 내부 구조를 숨겨야하므로
- 자료 구조 : 디미터 법칙을 적용하지 말자
    - 내부 구조를 변경할 필요가 없다.

→ 객체에게 항상 시키는 것이 가능하지 않다. 가끔씩은 물어야한다. **경우에 따라 다르다.**

# 명령-쿼리 분리 원칙

- 루틴
    - 어떤 절차를 묶어 호출 가능하도록 이름을 부여한 기능 모듈
    - 프로시저
        - 정해진 절차에 따라 내부의 상태를 변경하는 루틴의 한 종류
        - 부수효과를 발생시킬 수 있지만 값을 반환할 수 없다.
        - **명령(Command)**이라고도 한다.
    - 함수
        - 어떤 절차에 따라 필요한 값을 계산해서 반환하는 루틴의 한 종류
        - 값을 반환할 수는 있지만 부수효과는 발생시킬 수 없다.
        - **쿼리(Query)**라고도 한다.

### 명령-쿼리 분리 원칙의 요지

- 객체의 **상태를 변경**하는 **명령**은 반환값을 가질 수 없다.
- 객체의 **정보를 반환**하는 **쿼리**는 상태를 변경할 수 없다.

→ 질문이 답변을 수정해서는 안 된다.

## 반복 일정의 명령과 쿼리 분리하기

- 명령과 쿼리를 뒤섞으면 실행 결과를 예측하기 어려워진다.
    - ex. 겉으로 봤을때에는 쿼리처럼 보이는 메서드(`isSatisfied()`)가 부수효과를 가지면 이해하기 어렵고, 잘못 사용하기 쉽고, 버그를 양산할 수 있다.
- 명령과 쿼리를 정확하게 분리하자

## 명령-쿼리 분리와 참조 투명성

명령과 쿼리를 분리하면 **참조 투명성**의 장점을 제한적이나마 누릴 수 있다.

### 참조 투명성?

어떤 표현식 e가 있을 때 e의 값으로 나타나는 모든 위치를 교체하더라도 결과가 달라지지 않는 특정

### 불변성?

어떤 값이 변하지 않는 성질 = 부수효과가 발생하지 않는다.

### 명령형 프로그래밍과 함수형 프로그래밍

- 명령형 프로그래밍
    - 부수효과를 기반으로 하는 프로그래밍 방식
- 함수형 프로그래밍
    - 부수효과가 존재하지 않는 프로그래밍 방식

## 책임에 초점을 맞춰라

메시지를 먼저 선택하고 그 후에 메시지를 처리할 객체를 선택하라.

- 디미터 법칙
- 묻지 말고 시켜라
- 의도를 드러내는 인터페이스
- 명령-쿼리 분리 원칙

### 계약에 의한 설계

협력을 위해 두 객체가 보장해야 하는 실행 시점의 제약을 인터페이스에 명시할 수 없다.

→ 협력을 위해 클라이언트와 서버가 준수해야 하는 제약을 코드 상에 명시적으로 표현하고 강제화한 방법 (부록 A 참고)