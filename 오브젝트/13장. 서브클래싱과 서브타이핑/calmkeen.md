# 13. 서브클래싱과 서브타이핑



상속의 두가지 용도

1. 타입 계층을 구현하는 것
   - 타입 계층 안에서 부모 클래스는 일반적인 개념을 구현하고 자식 클래스는 특수한 개념을 구현한다.
   - 타입 계층의 관점에서 부모 클래스는 자식 클래스의 **일반화(generalization)** 이고 자식 클래스는 부모 클래스의 **특수화(specialization)** 다.
2. 코드 재사용
   - 간단한 선언만으로 부모 클래스의 코드를 재사용할 수 있다.



※ 상속을 사용하는 일차적 목표는 코드 재사용 x 타입 계층 구현 O

- 코드 재사용을 목표로하면 클래스를 강하게 결합시켜 변경 진화를 방해해 유연하지 못한 설계가 된다.



## 01. 타입



### 개념 관점의 타입

우리가 인지하는 세상의 사물의 종류

어떤 대상이 타입으로 분류될 때 그 대상을 타입의 **인스턴스**라고 부른다.

일반적으로 타입의 인스턴스를 **객체**라고 부른다.



타입의 구성요소

- 심볼 (symbol)
  - 타입에 이름을 붙인것.
- 내연(intension)
  - 타입의 저의로서 타입에 속하는 객체들이 가지는 공통적인 속성이 행동
- 외연(extension)
  - 타입에 속하는 객체들의 집합이다.



### 프로그래밍 언어 관점의 타입



사용 목적

1. 타입에 수행될 수 있는 유효한 오퍼레이션의 집합을 정의한다.
   - 모든 객체지향 언어들은 객체의 타입에 따라 적용 가능한 연산자의 종류를 제한함으로써 프로그래머의 실수를 막아준다는 것
2. 타입에 수행되는 오퍼레이션에 대해 미리 약속된 문맥을 제공한다.
   - 객체를 생성하는 방법에 대한 문맥을 결정하는 것은 바로 객체의 타입이다.



### 객체지향 패러다임 관점의 타입



- 개념 관점에서 타입이란 공통의 특징을 공유하는 대상들의 분류
- 프로그래밍 언어 관점에서 타입이란 동일한 오퍼레이션을 적용할 수 있는 인스턴스들의 집합



그렇다면 객체지향 관점에서는?

- 객체의 퍼블릭 인터페이스가 객체의 타입을 결정한다. 동일한 퍼블릭 인터페이스를 제공하는 객체들은 동일한 타입으로 분류된다.



퍼블릭 인터페이스 : 객체가 수신할 수 있는 메시지의 집합을 가리키는 용어



객체에 관한 중요한 것

- 객체에게 중요한 것은 속성이 아니라 행동이라는 사실이다.

- 객체를 바라볼 때는 항상 객체가 외부에 제공하는 행동에 초점을 맞춰야 한다.



## 02. 타입 계층 



### 타입 사이의 포함관계



타입은 공통적인 특성을 가진 객체들을 포함하는 집합이다.

![image-20210418122722231](https://user-images.githubusercontent.com/78361650/115144132-e51e6100-a085-11eb-9c4a-ebda80f773f7.png)

다른 타입을 포함하는 타입은 포함되는 타입보다 좀 더 일반화된 의미를 표현할 수 있다.

**포함하는 타입은 외연 관점에서는 더 크고 내연 관점에서는 더 일반적이다. / 포함되는 타입은 외연 관점에서는 더 작고 내연 관점에서는 더  특수하다.**





#### 일반화와 특수화

일반화는 다름 타입을 완전히 포함하거나 내포하는 타입을 식별하는 행위 / 결과이다.

특수화는 다른 타입안에 전체적으로 포함되거나 완전히 내포되는 타입을 식별하는 행위 /결과이다.



일반적인 타입을 슈터파입이라 부르고  특수한 타입을 서브타입이라고 부른다.

- 슈퍼타입
  - 집합이 다른 집합의 모든 멤버를 포함
  - 타입 정의가 다른 타입보다 더 일반적
- 서브타입
  - 집합에 포함되는 인스턴스들이 더 큰 집합에 포함
  - 타입 정의가 다른 타입보다 좀 더 구체적



### 객체지향 프로그래밍과 타입 계층



객체의 타입을 결정하는 것은 퍼블릭 인터페이스이다.



퍼블릭 인터페이스관점에서의 슈퍼타입과 서브타입



- **슈퍼타입**이란 서브타입이 정의한 퍼블릭 인터페이스를 일반화시켜 상대적으로 범용적이고 넓은 의미로 정의한 것
- **서브타입**이란 슈퍼타입이 정의한 퍼블릭 인터페이스를 특수화시켜 상대적으로 구체적으고 좁은 의미로 정의한 것



## 03 서브 클래싱과 서브타이핑



### 언제 상속을 사용해야 하는가?



상속 관계가 is -a 관계를 모델링하는가?

클라이언트 입장에서 부모 클래스의 타입으로 자식 클래스를 사용해도 무방한가?



### is-a 관계

is-a 관계를 모델링할 경우에만 상속을 사용해야 한다. 하지만 이것도 쉽게 배신될 수 있다.



ex) 펭귄은 새다. 새는 날수 있다. ??? 모순

어휘적인 정의가 아니라 기대되는 행동에 따라 타입 계층을 구성해야 한다.

따라서 타입 계층의 의미는 행동이라는 문맥에 따라 달라질수 있다. 

행동호환성이 중요하다!



### 행동 호환성



타입들 사이에 행동이 호환될 경우에만 타입으로 묶어야한다.

행동호환성의 여부를 판단하는 기준은 클라이언트의 관점이다.



위 펭귄예제와 같은 방법의 해결방법

- 인터페이스 분리 원칙 : 인터페이스를 클라이언트의 기대에 따라 분리함으로써 변경에 의해 영향을 제어하는 설계 원칙





### 서브 클래싱과 서브 타이핑

상속을 사용하는 두가지 목적 

- 서브 클래싱 

  - 다른 클래스의 코드를 재사용할 목적을 가리킨다.

- 서브타이핑

  - 타입 계층을 구성하기 위해 상속을 사용하는 경우를 가리킨다.

  

  

자식 클래스와 부모 클래스 사이의 행동 화환성은 부모 클래스에 대한 자식 클래스의 대체 가능성을 포함한다.

서브타이핑 관계가 유지되기 위해서는 행동호환성을 만족시켜야한다.

**대체 가능성과 행동 호환성은 올바른 상속을 구축하기 위한 지침이다!**



## 04 리스코프 치환 원칙



정의 : 서브타입은 그것의 기반 타입에 대해 대체 가능해야 한다는 것으로 클라이언트가 차이점을 인식하지 못한 채 클래스의 인터페이스를 통해 서브클래스를 사용할 수 있어야 한다.는 것



### 클라이언트와 대체 가능성

라이코프 치환 원칙은 클라이언트와 격리한 채로 본 모델은 의미 있게 검증하는 것이 불가능하다.라고 말한다.

행동 호환성과 리스코프 치환 원칙에서 제일 중요한것은 대체 가능성을 결정하는 것은 클라이언트라는 사실이다.



결론적으로 상속이 서브타이핑을 위해 사용될 경우에만 is-a관계이다. 서브클래싱을 구현하기 위해 사용한것은 is-a관계가 아니다.



![image-20210418130711555](C:\Users\user.DESKTOP-MH5KDIR.000\AppData\Roaming\Typora\typora-user-images\image-20210418130711555.png)

의존성 역전/ 리스코프 치환/ 개방 -폐쇄 가 조합된 유연한 설계의 예



## 05 계약에 의한 설계와 서브타이핑



클라이언트와 서버 사이의 협력을 의무와 이익으로 구성된 계약의 관점에서 표현하는 것을 계약에 의한 설계(Design By Contract, DBC)라고 부른다.



3가지 구성 요소

- 사전조건 : 클라이언트가 정상적으로 메서드를 실행하기 위해 만족시켜야 하는 것
- 사후조건 : 메서드가 실행된 후에 서버가 클라이언트에게 보장해야 하는 것
- 클래스 불변식 : 메서드 실행 전과 실행 후에 인스턴스가 만족시켜야 하는 것



알아둘 것

서브타입에 더 강력한 사전조건을 정의할 수 없다.

서브타입에 슈퍼타입과 같거나 더 강한 사후조건을 정의할 수 있다.

서브타입에 더 약한 사후조건을 정의할 수 없다.





