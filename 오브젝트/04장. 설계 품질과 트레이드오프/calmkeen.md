# 설계 품질과 트레이드오프



객체지향 설계의 핵심은 역할, 책임, 협력이다.

중에  가장 중요한 것은 책임이다.

객체지향 설계란 올바른 객체에게 올바른 책임을 할당하면서 낮은 결합도와 높은 응집도를 가진 구조를 창조하는 활동이다.

- 두가지  관점
  - 객체지향의 설계의 핵심은 책임이다.
  - 책임을 할당하는 작업이 응집도와 결합도 같으 설계 품질과 깊게 연관되어 있다.



결합도와 응집도를 합리적인 수준으로 유지하려면 개체의 상태가 아니라 행동에 초점을 맞추는 것이다.

## 데이터 중심의 영화 예매 시스템

객체 분할

- 상태를 분할의 중심축으로 삼는 방법
- 책임을 분할의 중심축으로 삼는 방법

> > 여기서는 상태와 데이터를 같은 의미로 사용한다.

휼륭한 객체 지향 설계 = 데이터가 아니라 책임에 초점

- 객체의 상태는 구현에 속한다.
- 구현은 불안정하기 때문에 변하기 쉽다.
- 인터페이스에 의존하는 객체에게 변경의 영향이 퍼지게 된다. -> 데이터에 초점을 맞추는 설계는 변경에 취약



#### 데이터 준비하자

데이터 중심 설계에서 흔이 볼 수 있는 패턴 - 저장 인스턴스 변수와 인스턴스 종류에 따라 배타적으로 사용될 인스턴스 변수를 하나의 클래스 안에 함께 포함시키는 방식

- 외부의 객체를 오염시키는 것을 막는 가장 간단한 방법
  - 접근자와 데이터를 변경하는 수정자를 추가하는 것



## 설계 트레이드오프

#### 캡슐화

변경될 가능성이 높은 부분 -> *구현*

상대적을 안정된 부분 -> *인터페이스*

중요 : 객체설계시 구현과 인터페이스를 분리하고 외부에서는 인터페이스에만 의존하도록!!

- 정리
  - 캡슐화란 변경 가능성이 높은 부분을 내부로 숨기는 추상화 기법
  - 객체설계의 핵심은 캡슐화이다.



#### 응집도와 결합도

- 응집도
  - 모듈에 포함된 내부 요소들이 연관돼 있는 정도
    - 목적을 위해 협력 -> 높은 응집도
    - 서로 다른 목적 -> 낮은 응집도
- 결합도
  - 의존성의 정도를 나타내며 다른 모듈에 대해 얼마나 많은 지식을 갖고 있는지를 나타내는 척도
    - 서로를 자세히 안다 -> 높은 결합도
    - 꼭 필요한 지식만 안다 -> 낮은 결합도



좋은 설계란 응집도와 낮은 결합도를 가진 모듈로 구성된 설계를 의미한다.



## 데이터 중심의 영화 예매 시스템의 문제점

대표적인 문제

- 캡슐화 위반
- 높은 결합도
- 낮은 응집도



#### 캡슐화 위반

설계할때 협력관계를 고민하지 않으면 과도한 접근자와 수정자를 가지게 된다.

이런 전략은  결과적으로 내부 구현이 퍼블릭 인터페이스에 그대로 노출될수 밖에 없어 그결과 캡슐화의 원칙을 위반하는 변경에 취약한 설계가 된다.



#### 높은 결합도

데이터 중심의 설계는 시스템을 하나의 거대한 의존성 덩어리로 만들어 버리기 때문에 변경이 발생하면 시스템이 요동친다.



#### 낮은 응집도

어떤 요구사항 변경을 수용하기 위해 하나 이상의 클래스를 수정해야 하는 것은 설계 응집도가 낮다는 것이다.



## 자율적인 객체를 향해

#### 캡슐화를 지켜라

- 객체는 스스로의 상태를 책임져야 하며 외부에서는 인터페이스에 정의된 메서드를 통해서만 상태에 접근할 수 있어야 한다.

자주 있는 캡슐화의 문제

- 코드 중복
- 변경에 취약

해결 방법 : 캡슐화를 강화하면 자연스럽게 두가지 방법이 다 해결됨. 내부를 조정하는 로직을 캡슐화(책임 이동)



#### 스스로 자신 데이터를 책임지는 객체

- 이 객체가 어떤 데이터를 포함해야 하는가?
- 이 객체가 데이터에 대해 수행해야 하는 오퍼레이션은 무엇인가?



## 데이터 중심 설계의 문제점

- 데이터 중심의 설계는 본질적으로 너무 이른 시기에 데이터에 관해 결정하도록 강요한다.
  - 이 객체가 포함해야 하는 데이터가 무엇인가? 에 대해서 생각해라

- 데이터 중심의 설계에서는 협력이라는 문맥을 고려하지 않고 객체를 고립시킨 채 오퍼레이션을 결정한다.
  - 필요한 책임을 결정하고 이를 수행할 적절한 객체를 결정해라.
  - 내부가 아닌 외부에 맞춰져 있어야 한다.







