# 02. 이상한 나라의 객체

# 객체지향과 인지 능력

소프트웨어 세계 역시 인간이 인지할 수 있는 다양한 객체들이 모여 이뤄져있다는 믿음에서 출발한다.

단, 객체지향은 현실 세계를 모방하는 것이 아닌 창조하는 것이다.

현실세계와 다르게 사람의 손길이 없어도 상관이 없는 것들이 있다. (ex. 알아서 불키는 전등, 스스로 금액 인출하는 통장)

# 객체, 그리고 이상한 나라

## 앨리스 객체

- 앨리스는 상태를 가지며 상태는 변경 가능하다.
- 앨리스의 상태를 변경시키는 것은 앨리스의 행동이다.
    - 행동의 결과는 상태에 의존적이며 상태를 이용해 서술할 수 있다.
    - 행동의 순서가 결과에 영향을 미친다.
- 앨리스는 어떤 상태여도 유일하게 식별할 수 있다.

## 객체, 그리고 소프트웨어 나라

### 객체

식별 가능한 개체 또는 사물일 수도, 구체적인 사물일 수도, 추상적일 수도 있다.

**구별 가능한 식별자, 특징적인 행동, 변경 가능한 상태**를 가진다.

## 상태

### 왜 상태가 필요한가

객체가 주변 환경과의 상호작용에 어떻게 반응하는가는 그 시점까지 객체에 어떤 일이 발생했느냐에 좌우된다.

ex. 비행기를 타려면 티켓 발권을 해야한다. 엘레베이터를 이용하려면 층의 버튼을 먼저 눌러야한다.

하지만 모든 과거를 기억할 수 없다. **상태**를 이용하면 괴거에 얽매이지 않고 현재를 기반으로 객체의 행동 방식을 이해할 수 있다. 복잡성과 인지 부조화를 줄일 수 있는 중요한 개념이다.

### 상태와 프로퍼티

단순한 값들은 객체가 아니다. 주로 객체의 특성을 표현하는데 사용된다.

때로는 단순한 값이 아니라 객체를 사용해 다른 객체를 표현할 때 사용한다.

- 프로퍼티
    - 객체의 상태를 구성하는 모든 특징
    - (ex. 키, 위치, 음료(다른 객체))
    - 정적이다.
- 프로퍼티 값
    - 동적이다.
- 링크
    - 객체와 객체 사이의 의미 있는 연결
    - 링크가 있어야 요청을 주고 받을 수 있다. (즉, 협력이 가능하다)

- 객체의 상태

상태는 특정 시점에 객체가 가지고 있는 **정보의 집합**으로, 객체의 구조적 특징을 표현한다.

객체의 상태는 정적 프로퍼티, 동적 프로퍼티로 구성된다.

객체의 프로퍼티는 단순한 값과 다른 객체를 참조하는 링크로 구분할 수 있다.

## 행동

### 상태와 행동

객체의 행동은 객체의 상태를 변경시키지만, 결과는 객체의 상태에 의존적이다.

(ex. 음료를 마신 후의 앨리스의 키는 그 전보다 작아야한다.)

- 상태와 행동 사이의 관계
    - 객체의 행동은 상태에 영향을 받는다.
    - 객체의 행동은 상태를 변경시킨다.
- 상태라는 개념을 이용해 정의한 행동
    - 상호작용이 현재의 상태에 어떤 방식으로 의존하는가
    - 상호작용이 어떻게 현재의 상태를 변경시키는가

### 협력과 행동

객체가 다른 객체와 협력하는 유일한 방법은 다른 객체에게 **요청**을 보내는 것이다.

- 객체의 행동으로 발생하는 결과
    - 객체 자기 자신의 상태 변경
    - 행동 내에서 협력하는 다른 객체에 대한 메시지 전송
        - ex. 내가 음료수를 마셨으니 음료수 양을 줄여주세요!

- 행동

외부의 요청 또는 수신된 메시지에 응답하기 위해 동작하고 반응하는 활동

행동의 결과로 자신의 상태를 변경하거나 다른 객체에게 메시지를 전달할 수 있다.

객체는 행동을 통해 다른 객체와 협력에 참여하므로 행동은 외부에 가시적이어야 한다.

### 상태 캡슐화

- 현실세계
    - '사람'은 능동적인 상태 변경이 가능하지만, '음료수'는 스스로 아무것도 할 수 없다.
- 객체지향 세계
    - 모든 객체는 자신의 상태를 스스로 관리하는 자율적인 존재이다.

- 캡슐화
    - 메세지 송신자는 수신자에 대해 아~무것도 모른다.
    - 객체가 노출하는 것은 **행동**뿐이며, 외부에서 접근하는 유일한 방법 역시 행동이다.
- 캡슐화를 해야하는 이유
    - 객체의 자율성을 높이고 협력을 단순하고 유연하게 만들 수 있다.

## 식별자

### 프로퍼티

객체를 구별할 수 있는 수단

단순한 값은 프로퍼티를 가지지 않는다.

### 값

변하지 않는 양을 모델링한다. (불변 상태)

같은지 여부는 상태가 같은지를 이용해 판단한다. → 동등성

별도의 식별자가 필요없다.

값 객체(value object)라고 부른다.

### 객체

시간에 따라 변경되는 상태를 포함하며, 행동을 통해 상태를 변경한다. (가변 상태)

식별자를 기반으로 같은지 판단한다. → 동일성

상태가 변하기 때문에 상태를 통해 같은지 판별할 수 없다.

참조 객체(reference object), 엔티티(entity)라고도 부른다.

# 기계로서의 객체

기계의 버튼을 눌러 원하는 것을 확인할 수 있다.

기계의 내부를 뜯어서 원하는 것을 확인하지는 않는다! → 객체에 접근할 수 있는 것은 **행동**뿐이다.

# 행동이 상태를 결정한다

상태를 결정하고 행동을 결정하면 안 된다!

1. 캡슐화가 저해된다.
2. 객체를 협력자가 아닌 고립된 섬으로 만든다.
3. 객체의 재사용성이 저하된다.

→ **행동**에 초점을 맞춰야한다.

# 은유와 객체

## 두 번째 도시 전설

> 객체지향이란 현실 세계의 모방이다.

객체지향의 세계의 객체들은 스스로 행동을 할 수 있다. → 현실세계에서 단순화하거나 추상화 한 것이 아니다.

## 의인화

객체지향은 현실을 모습을 조금 참조할 뿐, 궁극적인 목적은 전혀 다른 새로운 세계를 창조하는 것이다.

## 은유

현실세계와 객체지향 세계 사이의 관계를 좀 더 정확하게 설명할 수 있는 단어!

한 종류의 사물을 다른 종류의 사물 관점에서 이해하고 경험한다.

효과적을 사용하면 표현적 차이를 줄일 수 있고, 이해하기 쉽고, 유지보수가 용이한 소프트웨어를 만들 수 있다.

## 이상한 나라를 창조하라

모방이 아닌 새로운 세계를 창조하면 된다!
