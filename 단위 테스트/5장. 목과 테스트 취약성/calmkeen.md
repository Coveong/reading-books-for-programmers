# 5. 목과 테스트 취약성

## 5.1 목과 스텁 구분

2장에서 목은 테스트 대상 시스템과 그 협력자 사이의 상호 작용을 검사할 수 있는 테스트 대역이라고 했다.

스텁 (stub)이라는 건 테스트 대역의 또다른 유형이다.

### 테스트 대역 유형

테스트 대역이란?

모든 유형의 비운여용 가짜 의존성을 설명하는 포괄적인 용어이다.

![6](https://user-images.githubusercontent.com/78361650/189585205-9d719628-6b13-4a8d-8023-726cc2ea1a03.png)


두 유형의차이점

- 목은 외부로 나가는 상호 작용을 검사하고 모방하는데 도움이 된다.
- 스텁은 내부로 들어오는 상호작용을 모방하는데 도움이 된다.

### 스텁으로 상호 작용을 검증하지 말라

> 참고 | 스텁과 상호작용을 검증하는 것은 취약한 테스트를 야기하는 일반적인 안티 패턴이다.
> 

최종 결과가 아닌 사항을 검증하는 이러한 관행을 과잉 명세라고 부른다.

과잉 명세는 상호작용을 검사할때 발생하는데 스텁과 상호작용을 확인하는 것은  결함이다.

목은 더 복잡하다. 목을 쓰면 무조건 테스트 취약성을 초래하는 것은 아니지만 대다수가 그렇다.

### 목과 스텁 함께 쓰기

```swift
[Fact]
public ovdi Purchase_fails_when_not_enough_inventory()
{
	var storeMock = new Mock<IStore>();
//준비된 응답을 설정
	storeMock
		.Setup(x => x.HasEnoughInventory(
			Product.Shampoo, 5))
		.Returns(false);
	var sut = new Customer();

bool success = sut.Purachase(
//SUT에서 수행한 호출을 검사
		storeMock.Object, Product.Shampoo, 5);
	Assert.False(sucess);
	storeMOck.Verify(
		x => x.RemoveInventroy(Product.Shampoo, 5), Times,Never);

```

위 코드는 두가지 목적을 storeMock을 사용한다.준비된 응답을 반환하고 SUT에서 수행한 메서드 호출을 검증한다.

### 목과 스텁은 명령과 조회에 어떻게 관련돼 있는가?

목과 스텁의 개념은 명령 조회 분리 원칙과 관련이  있다.

 CQS(command Query Separation)원칙에 따르면 모든 메서드는 명령이거나 조회여야 하며, 혼용해서는 안된다.

![7](https://user-images.githubusercontent.com/78361650/189585226-50b70512-e7b2-41ce-b424-ed32323d054b.png)


명령조회 분리 원칙에서 명령은 목에 해당하는 반면, 조회는 스텁과 일치한다.

항상 CQS 원칙을 따를수 있는것은 아니다. 하지만 가능할 때마다 원칙을 따르는 것이 좋다.

## 5.2 식별할 수 있는 동작과 구현 세부 사항

테스트는 ‘어떻게'가 아니라 ‘무엇’에 중점을 둬야 한다. 

### 식별할 수 있는 동작은 공개 API와 다르다.

모든 제품 코드는 2차원으로 분류할 수 있다.

- 공개 api 또는 비공개 api
- 식별할 수 있는 동작 또는 구현 세부 사항

식별할 수 있는 동작과 내부 구현 세부 사항에는 차이가 있다.

코드가 시스템의 식별할 수 있는 동작이려면 다음 중 하나를 해야 한다.

- 클라이언트가 목표를 달성하는 데 도움이 되는 연산을 노출하라.
- 클라이언트가 목표를 달성하는 데 도움이 되는 상태를 노출하라.

![8](https://user-images.githubusercontent.com/78361650/189585239-4f5435f5-3fc5-41ee-8598-1f43a28b3d89.png)

잘 설계된 api

![9](https://user-images.githubusercontent.com/78361650/189585254-f9ff86ef-a588-4e46-9429-0a35122da227.png)

공개 api가 식별할 수 있는 동작의 범위를 넘어서면 시스템은 구현 세부 사항을 유출한다.

### 잘 설계된 api와 캡슐화

캡슐화는 불변성 위반이라고도 하는 모순을 방지하는 조치이다.

장기적으로 코드베이스 유지 보수에서 캡슐화가 중요하다.

묻지말고 말하라 라는 유사 원칙이 있다.

데이터를 연산 기능과 결합하는 것을 의미한다. 코드 캡슐화가 목표이지만 구현 세부 사항을 숨기고 데이터와 기능을 결합하는 것이 해당 목표를 달성하기 위한 수단이다.

- 구현 세부 사항을 숨기면 클라이언트의 시야에서 클래스 내부를 가릴 수 있기 떄문에 내부를 손상시킬 위험이 적다.
- 데이터와 연산을 결합하면 해당 연산이 클래스의 불변성을 위반하지 않도록 할 수 있다.

> API를 잘 설계하면 단위 테스트도 자동으로 좋아진다.
> 

![10](https://user-images.githubusercontent.com/78361650/189585268-4d370b1d-c63f-47d4-8762-647bc7b20a9f.png)

## 5.3 목과 테스트 취약성 간의 관계

### 육각형 아키텍쳐의 정의

![11-1](https://user-images.githubusercontent.com/78361650/189585333-edd7db81-53ce-4530-9d76-216ab5fc93ad.png)


애플리케이션 서비스에 대한 조정의 예

- 데이터베이스를 조회하고 해당 데이터로 도메인 클래스 인스턴스 구체화
- 해당 인스턴스 연산 호출
- 결과를 데이터베이스에 다시 저장

![11](https://user-images.githubusercontent.com/78361650/189585352-fdd3d95d-53f0-4a99-86af-06b02bd59fbb.png)

육각형 아키텍쳐는 상호작용하는 애플리케이션의 집합이다.

세가지 중요 지침

- 도메인 계층과 애플리케이션 서비스 계층 간의 관심사 분리
- 애플리케이션 내부 통신
- 애플리케이션 간의 통신

애플리케이션의 각 계층은 식별할 수 있는 동작을 나타내며 해당 구현 세부 사항을 포함하고 있다.

각 계층의 API를 잘 설계하면 테스트도 프랙탈 구조를 갖기 시작한다.

### 시스템 내부 통신과 시스템 간 통신

![12](https://user-images.githubusercontent.com/78361650/189585375-3c110247-a3f9-4590-8d4f-7daff09870d9.png)

> 시스템 내부 통신은 구현 세부 사항이고, 시스템 간 통신은 그렇지 않다.
> 

목을 사용하면 시스템과 외부 애플리케이션간의 통신 패턴을 확인할 때 좋다.반대로 시스템 내 클래스 간의 통신을 검증하는데 사용하면 테스트가 구현 세부 사항과 결합되며 리팩터링 내성 지표가 미흡해진다.

## 5.4 단위 테스트의 고전파와 런던파 재고

### 모든 프로세스 외부 의존성을 목으로 해야 하는 것은 아니다.

- 공유 의존성 : 테스트 간에 공유하는 의존성
- 프로세스 외부 의존성: 프로그램의 실행 프로세스 외에 다른 프로세스를 점유하는 의존성
- 비공개 의존성: 공유하지 않는 모든 의존성

종종 목이 동작을 검증한다고 한다. 하지만 대부분의 경우 그렇지 않다. 목은 애플리케이션의 경계를 넘나드는 상호 작용을 검증할 떄와 이러한 상호자굥의 부작용이 외부환경에서 보일 때만 동작과 관련이 있다.
