# 이론

> 지금까지 무엇을, 언제, 어떻게 정리해야 하는지 알아보았으니, 이제 코드를 정리해야 하는 이유에 대해 이야기하겠습니다.
>
> 이론을 이해하면 추측으로 답해야 할 때를 대비해 판단력을 키울 수 있습니다. 또한, 동료 괴짜들과의 논쟁을 좀 더 건설적으로 할 수도 있죠.

- 소프트웨어 설계란 무엇인가?
- 소프트웨어 설계가 소프트웨어 개발과 운영 비용을 어떻게 좌우하는가? 반대로, 소프트웨어 개발과 운영 비용이 소프트웨어 설계를 어떻게 좌우하는가?
- 소프트웨어 구조에 투자할 때와 투자하지 않을 때의 장단점은 무엇인가?
- 소프트웨어 구조를 변경할지 여부와 방법을 결정할 때 어떤 경제적, 인간적 원칙을 사용할 수 있는가?

## Chapter 22 요소들을 유익하게 관계 맺는 일
소프트웨어 설계는 인간관계 속에서 벌어지는 활동이다.

즉, **요소들을 유익하게 관계 맺는 일**이다.

### 요소
- 요소에는 경계가 있다. 어디서 시작하고 끝나는지 알 수 있다.
- 요소는 하위 요소를 포함한다.
- 프로그래밍 세계에서의 요소:
  - 토큰
  - 식(expression)
  - 문(statement)
  - 함수
  - 객체/모듈
  - 시스템

### 관계 맺기
- 요소들은 서로 관계를 지닌 존재들이다.
- 프로그래밍 세계에서의 관계들:
  - 호출
  - 발행(Publish)
  - 대기(Listen)
  - 참조
 
### 유익하게
설계할 때, 시스템에는 필요하지만, 기계를 위한 명령어가 아닌 중간 요소를 만들면 그 중간 요소들이 서로에게 도움이 되기 시작한다.

### 요소들을 유익하게 관계 맺는 일
설계란: 설계를 구성하는 요소들과 그들의 관계, 그리고 그 관계에서 파생되는 이점이다.

즉, 시스템의 구조는 다음과 같다.
- 요소 계층 구조
- 요소 사이의 관계
- 이러한 관계가 만들어내는 이점

## Chapter 23 구조와 동작
시스템의 구조는 동작에 영향을 미치지 않지만, 미래의 기회를 만든다.
> 옵션은 소프트웨어로 만들어내는 경제적인 마법이며, 주로 확장할 기회를 만듭니다.
> 소프트웨어에서 1천개의 알림을 보낼 수 있다면, 노력에 따라 10만개도 보낼 수 있을 것입니다.

가역성(되돌릴 수 있는 능력)의 측면에서 구조 변경과 동작 변경은 다르다.

## Chapter 24 경제 이론: 시간 가치와 선택 가능성
소프트웨어 설계는 '먼저 벌고 나중에 써야 한다'와 '물건이 아닌 옵션을 만들어야 한다'는 두 가지 옵션을 조화시켜야 한다.

## Chapter 25 오늘의 1달러가 내일의 1달러보다 크다
소프트웨어 시스템의 경제적 가치를 할인된 미래 현금 흐름의 합으로 설명할 수 있다.

> 이 책의 범위에서 돈의 시간 가치는 코드 정리르 먼저 하기보다는 나중에 하는 것을 권장합니다.
> 지금 당장 돈을 벌고 나중에 코드를 정리하는 행동 변화를 실천할 수 있다면 점차 먼저 돈을 벌고 나중에 돈을 쓸 수 있을 것입니다.

## Chapter 26 옵션
항상 "다음에 어떤 동작을 구현할 수 있을까?"라는 질문을 던지며 프로그래밍하자.
-> **가치에 대한 예측이 불확실할수록 바로 구현하는 것보다 옵션이 지닌 가치가 더 커진다.**

> 오늘 우리가 하는 설계는 내일의 동작 변경을 **구매**하는 **옵션**에 대해 지불하는 프리미엄니다.(콜 옵션 예시)

옵션을 만드는 것과 동작을 변경하는 것의 균형을 맞추는 데 집중하자.

## Chapter 27 옵션과 현금흐름 비교

- **현금흐름할인**은 높은 확률로 먼저 돈을 벌고, 낮은 확률로 나중에 돈을 쓰라고 말한다. 코드 정리를 먼저 하지 말아라. 돈을 더 일찍 쓰고, 돈을 나중에 버는 것이다. 어쩌면 나중에는 정리가 필요하지 않을 수도 있다.
- **옵션**은 나중에 더 많은 돈을 벌기 위해 설사 정확한 방법을 모르더라도, 지금 돈을 쓰라고 말한다. 옵션이 생길 일이 명백하다면, 코드 정리를 선행하자. 동작 변경 후에도 정리할 내용이 있다면 또 한다.
 
### 확실히 코드 정리부터 해야 할 때

```
비용(코드 정리) + 비용(코드 정리 후 동작 변경) < 비용(바로 동작 변경)
```

### 판단하기 어려운 순간

```
비용(코드 정리) + 비용(코드 정리 후 동작 변경) > 비용(바로 동작 변경)
```

- 소프트웨어 설계의 시기와 범위에 영향을 미치는 인센티브를 인식하는 데 익숙해지자
- 대인 관계 기술을 우리 자신에게 연습해서, 나중에 밀접하게 일하는 동료부터 더 넓은 범위의 동료에게까지 활용하자

## Chapter 28 되돌릴 수 있는 구조 변경
대부분의 소프트웨어 설계 결정은 쉽게 되돌릴 수 있다.

되돌릴 수 없는 설계 변경의 경우,
1. 이 결정이 확산될 가능성이 있는 결정인지에 대해 조금 더 생각해보고
2. 그런 일이 발생하면 한 번에 하나씩 정리해나가자

항상 그렇듯이 중단 가능한 작은 조각을 사용한다.

## Chapter 29 결합도
- 한 요소를 변경하면 다른 요소도 함께 변경해야 하는 경우, 두 요소는 특정 변경과 관련하여 서로 결합되어 있다.
- 어떤 변경이 일어나면 한 요소는 여러 요소와 결합이 일어난다.
- 일단 변경이 일어나면 한 요소에서 다른 요소로 변경이 파급되고 그 변경은 수많은 변경을 촉발할 수 있다.

-> 결합도는 소프트웨어 비용을 결정하는 중요한 요소다.

## Chapter 30 콘스탄틴의 등가성
소프트웨어 비용은 그것을 변경하는 데 드는 비용과 거의 같다.

가장 많이 비용이 드는 하나의 변경이 나머지 변경을 모두 합친 것보다 훨씬 많은 비용이 든다.
```
비용(소프트웨어) ~= 비용(전체 변경) ~= 비용(큰 변경들) ~= 결합도
```

따라서 소프트웨어 비용을 줄이려면 **결합도**를 줄여야 한다.

## Chapter 31 결합도와 결합도 제거
한 종류의 코드 변경에 대한 결합도를 줄일수록 다른 종류의 코드 변경에 대한 결합도가 커진다.

-> 모든 종류의 결합을 다 색출하듯 없애려고 애쓰지 말아야 한다.

<img src="https://github.com/Coveong/reading-books-for-programmers/assets/32327475/b7b787b9-a285-43ee-b744-e71f5e4ba6b4" width="40%"/>

(결합도에 따르는 비용과 결합도 제거 비용의 상충 관계)

## Chapter 32 응집도
- 결합된 요소들은 둘을 포함하는 같은 요소의 하위 요소여야 한다.
- 결합되지 않은 요소는 다른 곳으로 이동해야 한다.

> 한 번에 한 요소씩 이동하세요. 다음에 코드를 볼 사람을 위해 코드를 더 깔끔하게 정리하세요.
> 모두가 스카우트 규칙을 따른다면, 시간이 지날수록 코드가 더 살기 좋은 코드가 될 것입니다.

## Chapter 33 결론

코드 정리가 먼저인가?라는 질문에 답변하기 위해서는 다음 4가지 요소를 고려해야 한다.
- **비용**: 코드를 정리하면 비용이 줄까? 아니면 나중에 하는 편이 나을까? 줄어들 가능성이 있을까?
- **수익**: 코드를 정리하면 수익이 더 커질까? 혹은 더 빨리 발생하거나 커질 가능성이 있나?
- **결합도**: 코드를 정리하면 변경이 필요한 요소의 수가 줄어드나?
- **응집도**: 코드를 정리하면 변경을 더 작고 좁은 범위로 집중시켜 더 적은 수의 요소만 다룰 수 있을까?

하지만 가장 중요한 것은... 바로 우리다.
> 코드 정리는 소프트웨어 설계의 프링글스입니다. 마치 프링글스 뚜껑을 열면 멈출 수 없는 것처럼 한 가지 코드 정리를 하고 나면 다음 정리를 하고 싶은 충동이 생기겠지만, 억제해야 합니다.
> 또한 코드 정리는 다음 동작 변경을 가능하게 합니다. 그러나 변경을 기다리는 다른 누군가가 기다리다 폭발하지 않도록 코드 정리를 나중에 해야 할 때도 있습니다.

이 훌륭한 기술의 궁극적인 보상은 자신이 다른 사람들과 더 잘 지내는 것이다.

### 코드 정리를 먼저 하실 건가요?
> 아마도요. 바로 그것입니다. 예, 여러분을 위해서 충분히 그만한 가치가 있습니다.
