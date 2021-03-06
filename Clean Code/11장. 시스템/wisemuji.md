# 시스템

> 복잡성은 죽음이다. 개발자에게서 생기를 앗아가며, 제품을 계획하고 제작하고 테스트하기 어렵게 만든다.

-- 레이 오지, 마이크로소프트 최고 기술 책임자

## 시스템 제작과 시스템 사용을 분리하라

* 제작과 사용은 다른 개념이다.
* 체계적이고 탄탄한 시스템을 만들고 싶다면 흔히 쓰는 좀스럽고 손쉬운 기법으로 모듈성을 깨서는 안된다.

### Main 분리
시스템 생성과 시스템 사용을 분리하는 한 가지 방법으로, 생성과 관련한 코드는 모두 main이나 main이 호출하는 모듈로 옮기고, 
나머지 시스템은 모든 객체가 생성되었고 모든 객체가 생성되었고 모든 의존성이 연결되었다고 가정한다.

즉, 애플리케이션은 main이나 객체가 생성되는 과정을 전혀 모르고 모든 객체가 적절히 생성되었다고 가정한다.

### 팩토리
때로는 객체가 생성되는 시점을 애플리케이션이 결정할 필요도 생긴다. 그럴 때는 Abstract Factory 패턴을 사용한다.
그러면 객체를 생성하는 시점은 애플리케이션이 결정하지만 객체를 생성하는 코드는 모르게 된다.

### 의존성 주입
사용과 제작을 분리하는 강력한 메커니즘 하나다. 의존성 주입은 제어 역전(IoC) 기법을 의존성 관리에 적용한 메커니즘이다.

제어 역전에서는 한 객체가 맡은 보조 책임을 새로운 객체에게 전적으로 떠넘긴다. 새로운 객체는 넘겨받은 책임만 맡으므로 단일 책임 원칙을 지키게 된다.

## 확장
처음부터 올바르게 시스템을 만들 수 있다는 믿음은 미신이다. 대신에 우리는 오늘 주어진 사용자 스토리에 맞춰 시스템을 구현해야 한다. 
내일은 새로운 스토리에 맞춰 시스템을 조정하고 확장하면 된다. 이것이 반복적이고 점진적인 애자일 방식의 핵심이다.

## 테스트 주도 시스템 아키텍처 구축
아주 단순하면서도 멋지게 분리된 아키텍처로 소프트웨어 프로젝트를 진행해 결과물을 재빨리 출시한 후, 기반 구조를 추가하며 조금씩 확장해나가도 괜찮다.
그렇다고 아무 방향 없이 프로젝트에 뛰어들어도 좋다는 뜻은 아니다. 프로젝트를 시작할때는 일반적인 구조를 생각해야 한다.

> 최선의 시스템 구조는 각기 POJO(또는 다른) 객체로 구현되는 모듈화된 관심사 영역(도메인)으로 구성된다. 
이렇게 서로 다른 영역은 해당 영역 코드에 최소한의 영향을 미치는 관점이나 유사한 도구를 사용해 동합한다. 

## 의사 결정을 최적화하라

> 관심사를 모듈로 분리한 POJO 시스템은 기민함을 제공한다. 이런 기민함 덕택에 최신 정보에 기반해 최선의 시점에 최적의 결정을 내리기가 쉬워진다. 또한 결정의 복잡성도 줄어든다.

## 명백한 가치가 있을 때 표준을 현명하게 사용하라

> 표준을 사용하면 아이디어와 컴포넌트를 재사용하기 쉽고 적절한 경험을 가진 사람을 구하기 쉬우며, 좋은 아이디어를 캡슐화하기 쉽고, 컴포넌트를 엮기 쉽다.
하지만 때로는 표준을 만드는 시간이 너무 오래 걸려 업계가 기다리지 못한다. 
어떤 표준은 원래 표준을 재정한 목적을 잊어버리기도 한다.

## 시스템은 도메인 특화 언어가 필요하다

> 도메인 특화 언어(Domail-Specific Language)를 사용하면 고차원 정책에서 저차원 세부사항에 이르기까지 모든 추상화 수준과 모든 도메인을 POJO로 표현할 수 있다.

# 결론
시스템 역시 깨끗해야 한다. 꺠끗하지 못한 아키텍처는 도메인 논리를 흐리며 기민성을 떨어뜨리고 제품 품질도 떨어진다. 버그가 숨어들기 쉬워지고 생상성이 낮아져 TDD가 제공하는 장점이 사라진다.

모든 추상화 단계에서 의도는 명확히 표현해야 한다. 그러려면 POJO를 작성하고 각 구현 관심사를 분리해야 한다.

실제로 돌아가는 가장 단순한 수단을 사용해야 한다는 사실을 명심하자.
이런 구조 역시 코드와 마찬가지로 테스트 주도 기법을 적용할 수 있다.
