# 06. 메시지와 인터페이스



훌륭한 객체지향 코드 -> 클래스가 아니라 객체를 지향해야 한다.( 객체가 수행하는 책임에 초점을 맞추자!)



## 01. 협력과 메시지



#### 클라이언트 - 서버 모델



메세지를 전송하는 객체를 클라이언트 , 메세지를 수신하는 객체를 서버라고 부른다.

협력은 클라이언트가 서버의 서비스를 요청하는 단방향 상호작용이다.



#### 메시지와 메시지 전송



**메시지**는 객체들이 협력하기 위해 사용할  수  있는 유일한 의소소통 수단이다.

한 객체가 다른 객체에게 도움을 요청하는것 : 메시지 전송, 메시지 패싱

메시지는 오퍼레이션명(operation name)과 인자(argument)으로 구성



#### 메시지와 메서드

메시지를 수신했을 때 실제로 실행되는 함수 또는 프로시저를 **메서드**라고 부른다.

코드상에서 동일한 이름의 변수에게 동일한 메시지를 전송하더라도 객체의 타입에 따라 실행되는 메서드가 달라질 수 있다.

객체는 메시지와 메서드라는 두 가지 서로 다른 내겸을 실행 시점에 연결해야 하기 때문에 컴파일 시점과 실행 시점의 의미가 달라 질 수 있다.



#### 퍼블릭 인터페이스와 오퍼레이션

- **퍼블릭 인터페이스** :객체가  의사소통을 위해 외부에 공개하는 메시지의 집합  

- **오퍼레이션** :프로그래밍 언어의 관점에서 퍼블릭 인터페이스에  포함된 메시지 
- **메서드** : 메세지를 수신했을 때 실제로 실행되는 코드



![image-20210329004749510](https://user-images.githubusercontent.com/78361650/115144318-03d12780-a087-11eb-9cc6-d0801059eabb.png)



>  관계



#### 시그니쳐

오퍼레이션(or method)의 이름과 파라미터 목록을 합친것

오퍼레이션은 실행 코드 없이 시그니쳐만.

메서드는 이 시그니처에 구현을 더한것



객체가 수신할수 있는 메시지가 객체의 퍼블릭 인터페이스와 그안에 포함될 오퍼레이션을 결정한다.

-> 결국 메시지가 객체의 품질을 결정



## 02. 인터페이스와 품질



#### 디미터 법칙

- 협력하는 객체의 내부 구조에 대한 결합으로 인해 발생하는 설계 문제를 해결하기 위해 제안된 원칙
- 내부 구조가 전송자에게 노출되지 않으며, 메시지 전송자는 수신자의 내부에  결합되지 않는다.
- 서버와 클라이언트 사이에 낮은 결합도를 유지할 수 있다.
  - 디미터의 법칙을 따르기 위해서는 조건을 만족하는 대상에게만 메시지를 전송하도록 프로그래밍 해야한다.
    - ex
      - this
      - method의 매개변수
      - this의 속성
      - this의 속성인 컬렌셔의 요소
      - 메서드 내에 생성된 지역 객체



디미터 법칙 위반의 경우 생기는것

> 기차 충돌
>
> > 내부 구현이 외부로 노출됐을 때 나타는형태
> >
> > 메시지 수신자의 캡슐화가 무너지고 전송자가 수신의 내부 구현에 강하게 결합ㄴ된다.



#### 묻지말고 시켜라

- 디미터의 법칙같은 스타일의 메시지 작성을 장려하는 원칙을 가리키는 용어

- 훌륭한 객체의 상태에 관해 묻지말고 시키는 것



#### 의도를 드러내는 인터페이스

1. 메서드가 작업을 어떻게 수행하는지를 나타내도록 이름을 짓는 것
2.  어떻게가 아니라 무엇을 하는지는 드러내는 것

**의도를 드러내는 선택자** :어떻게 하느냐가 아니라에 따라 메서드의 이름을 짓는 패턴

**의도를 드러내는 인터페이스** : 의도를 드러내는 선택자의 확장 레벨로 정보를 캡슐화 하고 객체의 퍼블릭 인터페이스에는 협력과 관련된 의도만을 표현



## 03. 원칙의 함정



1. 디미터의 법칙은 도트를 강제하는 규칙이 아니다.(디미터의 법칙을 "오직 하나의 도트만 사용하라"라고 한답니다.)
   - 기차 충돌처럼 보여도 구현에 대한 정보가 외부로 노출되지 않으면 디미터의 법칙을 준수한 것
2. 결합도와 응집도의 충돌
   - 객체 내부 구조를 숨겨야 하므로 디미터 법칙을 따르는 것이 좋지만 자료구조라면 당연히 내부를 노출해야하므로 생각을 해봐야함. **묻는 대상이 객체인지 자료구조인지**



## 04. 명령- 쿼리 분리 원칙

퍼블릭 인터페이스에 오퍼레이션을 정의할 때 참고할 지침을 제공



- 루틴
  - 어떤 절차를 묶어 호출가능하도록 부여
    구분방법
    - 프로시저
    - 함수
  - 루틴 장석에 제약
    - 프로시저는 부수효과를 발생시킬 수 있지만 값을 반환할수 없다.
    - 함수는 값을 반환할 수 있지만 부수효과를 발생시킬 수 없다.

- 명령(Command)/ 쿼리(Query)
  - 객체의 인터페이스 측면에서 프로시저와 함수를 부르는 또다른 이름

명령-쿼리 분리 원칙의 규칙

- 객체의 상태를 변경하는 명령은 반환값을 가질수 없다.
- 객체의 정보를 반환하는 쿼리는 상태를 변경할 수 없다.



명령 쿼리 분리원칙에 따라 작성된 객체 인터페이스는 -> 명령-쿼리 인터페이스



#### 반복일정의 명령과 쿼리 분리하기

- 책 참고 (다량의 코드 설명은 책을 참고하시기 바랍니다.)

#### 명령- 쿼리 분리와 참조 투명성

- 참조의 투명성이란?
  - 어떠한 표현식 e가 있을 때 e의 값으로 e 가 나타나는 모든 위치를 교체하더라도 결과가 달라지지 안는 특성
- 참조의 투명성의 장점
  - 모든함수를 이미 알고 있는 하나의 결과값으로 대체할 수 있기 때문에 식을 쉽게 계산 할 수 있다.
  - 모든 곳에서 함수의 결과값이 동일하기 떄문에 식의 순서를 변경하더라도 각 식의 결과는 달라지지 x



> 명령형 프로그래밍
>
> > 부수효과를 기반으로 하는 프로그래밍
> >
> > > 부수효과 - 수학의 경우 값을 초기화 하면 변경이 불가능 하지만 프로그램은 대입문을 통해 가능하다.
> >
> > 최근 함수형 프로그래밍은 이에 포함하지 않는다. 그에 참조 투명성의 장점을 극대화 할 수 있다.



#### 책임에 초점을 맞춰라

메시지를  먼저 선택하는 방식은 디미터 법칙,. 묻지말고 시켜라 스타일 , 의도를 드러내는 인터페이스, 명령-쿼리 분리원칙에 긍정적인 영향을 준다.



디미터의 법칙 

- 결합도를 낮추고, 수신 객체를 알지 못하고 메시지를 선택해 내부 구조 고민이 업으며 메시지가 객체를 선택해 디미터의 법칙을 위반할 위험이 적다.

묻지말고 시켜라

- 협력을 구조화 하며, 클라이언트 관점이기 때문에 필요정보를 물을 필요가 없다.

의도를 드러내는 인터페이스

-  메시지를 먼저 선택 -> 클라이언트 관점에서 이름 선택 ->  자연스럽게 의도가 드러난다.

명령 - 쿼리 분리 법칙

- 메시지를 선택한다는 행위가 객체와 인터페이스를 고민하는 행우이기 때문에 예측 가능한 협력을 하기 위해 분리하게 된다.



**휼룽한 메시지의 출발은 책임 주도 설계 원칙을 따르는 것이다.**

**우리에게 중요한 것은 협력에 적합한 객체가 아니라 협력에 적합한 메시지이다.**











