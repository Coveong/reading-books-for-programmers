# 의존성 역전 원칙(DIP)

DIP에서 말하는 유연성이 극대화된 시스템: 소스 코드 의존성이 추상에 의존하며 구체에는 의존하지 않는 시스템

이때 의존하지 않도록 피하고자 하는 것은 '변동성이 큰 구체적인 요소'. (안정성이 보장된 환경은 제외하는 편이다)

## 안정된 추상화

안정된 소프트웨어 아키첵처란 변동성이 큰 구현체에 의존하는 일은 지양하고, 안정된 추상 인터페이스를 선호나는 것.

* 변동성이 큰 구체 클래스를 참조하지 말라
* 변동성이 큰 구체 클래스로부터 파생하지 말라
* 구체 함수를 오버라이드하지 말라
* 구체적이며 변동성이 크다면 절대로 그 이름을 언급하지 말라

## 팩토리

변동성이 큰 구체 클래스를 생성할 때 소스 코드 의존성을 피하기 위해 자바 등의 객체 지향 언어에서 추상 팩토리를 사용하곤 한다.

이때 제어 흐름은 소스 코드 의존성과는 반대 방향으로 역전된다 == 의존성 역전

## 구체 컴포넌트

대다수의 시스템에서 구체적인 의존성을 가지는 컴포넌트가(DIP를 위배하는) 하나 정도는 존재할 수 있다.(이는 일반적인 일이다.)

흔히 이 컴포넌트를 메인(Main) 이라고 부른다.

# 결론

이 책에서 DIP는 계속 언급될 것이다.
