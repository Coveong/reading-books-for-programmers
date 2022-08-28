# 2.단위 테스트란 무엇인가?

단위테스트 접근 두가지 방식

고전파 : 모든 사람이 단위 테스트와 테스트 주도 개발에 원론적으로 접근하는 방식

런던파 : 런던 커뮤니티를 중심으로 시작된 테스트 방식

## 1. ‘단위 테스트’의 정의

단위테스트의 속성

- 작은 코드 조각을 검증
- 빠르게 수행
- 격리된 방식으로 처리하는 저동화된 테스트

여기서 3번째 속성으로 두 파를 구분짓게 된다.

### 1.1 격리 문제에 대한 런던파의 접근

런던파

- 테스트 대상 시스템을 협력자에서 격리하는 것을 일컫는다.
- 동작을 외부 영향과 분리해서 테스트 대상 클래스에만 집중할수 있게 해준다.
- 상태만이 아니라 상호작용 및 호출을 바르게 했는지 확인한다.
- 테스트가 실패하면 코드베이스의 어느 부분이 고장났는지 확실히 알수 있다.
- 객체 그래프를 분할할 수 있다.

고전파

- 단위를 구성하는 것에 대한 견해를 달리해 반드시 클래스 단위에 국한 될 필요 없다
- 공유 의존성이 업는 한 여러 클래스를 묶어서 단위 테스트 할 수도 있다.

코드로 보는 차이

[ 고전파 ]

```swift
public void Purchase_fails_when_not_enough_inventory()
{
	//준비
    var store = new Store();
    store.AddInventory(Product.shampoo, 10);
    var customer = new Customer();
    
    //실행
   	bool success = customer.Purchase(store, Product.Shampoo, 15);
    
    //검증 (실패 했기에 수량 변화가 없음)
    Assert.False(success);
    Assert.Equals(10, store.GetInventory(Product.Shampoo));
 }
```

고전파의 방식이기에 협력자를 대체하지 않고 운영용 인스턴스를 사용

[ 런던파 ]

```swift
public void Purchase_fails_when_not_enough_inventory()
{
	//준비
    var store = new Mock<IStore>();
    storeMock
    	.Setup(x => x.HasEnoughInventory(Product.Shampoo, 5))
        .Return(false)
    var customer = new Customer();
    
    //실행
   	bool success = customer.Purchase(storeMock.Object, Product.Shampoo, 5);
    
    //검증 (실패 했기에 수량 변화가 없음)
    Assert.False(success);
    storeMock.Verify(
    	x => x.RemoveInventory(Product.Shampoo, 5),
        Times.Never);
 }
```

입자성, 상호 연결된 클래스의 큰 그래프에 대한 테스트 용의성 그리고 테스트 실패 후 버그가 있는 기능을 쉽게 찾을 수 있는 편의성 등을 제공

## 2. 단위테스트의 런던파와 고전파

런던파 : 테스트 대상 시스템에서 협력자를 격리, 불변의존성을 테스트 대역으로 대체

고전파 : 단위가 아닌 단위 테스트끼리 격리,테스트 대상은 코드가 아닌 동작 단위

의견차이가 나는 3가지

- 격리 요구 사항
- 테스트 대상 코드 조각의 구성 요소
- 의존성 처리

|  | 격리 주체 |  단위의 크기 |  테스트 대역 사용 대상 |
| --- | --- | --- | --- |
|  런던파 |  단위 |  단일 클래스 |  불변 의존성 외 모든 의존성 |
|  고전파 |  단위 테스트 |  단일 클래스 or 클래스 세트 |  공유 의존성 |

## 3. 고전파와 런던파의 비교

고전파 

- 공유 의존성을 테스트 대역으로 교체
- 상향식 TDD

런던파

- 변경 가능한 비공개 의존성
- 하향식 TDD
- 입자성이 좋다.
- 서로 연결된 클래스의 그래프가 커져도 테스트 하기 쉽다.
- 테스트가 실패하면 어떤 기능이 실패했는지 알수 있다.
- 실제 협력자 객체를 사용하는 모든 테스트를 통합 테스트로 간주한다.

런던파의 단점

- 과잉 명세
- 테스트 대상 클래스에 대한 초점
