# 시스템
복잡성은 죽음이다. 개발자에게서 생기를 앗아가며, 제품을 계획하고 제작하고 테스트하기 어렵게 만든다.

## 시스템 제작과 시스템 사용을 분리하라
제작은 사용과 아주 다름. 
관심사 분리는 우리 분야에서 가장 오래되고 가장 중요한 설계 기법중 하나.

체계적이고 탄탄한 시스템을 만들고 싶다면 흔히 쓰는 좀스럽고 손쉬운 기법으로 모듈성을 깨서는 절대 안된다.
객체를 생성하거나 의존성을 연결할 때도 마찬가지.

### Main 분리
시스템 생성과 시스템 사용을 분리하는 방법.
생성과 관련한 코드는 모두 main이나 main이 호출하는 모듈로 옮기고 나머지 시스템은 모든 객체가 생성되었고 모든 의존성이
연결되었다고 가정함.

### 팩토리
객체가 생성되는 시점을 애플리케이션이 결정할 필요도 생김.
Abstract Factory 패턴을 사용.

### 의존성 주입
사용과 제작을 분리하는 강력한 메커니즘 중 하나.

### 확장
처음부터 올바르게 시스템을 만들 수 있다는 믿음은 미신. 우리는 오늘 주어진 사용자 스토리에 맞춰 시스템을 구현해야하며,
내일은 새로운 스토리에 맞춰 시스템을 조정하고 확장한다. 
-> 반복적이고 점진적인 애자일 방식의 핵심 !

## 자바 프록시
단순한 상황에 적합하다. 개별 객체나 클래스에서 메서드 호출을 감싸는 경우!

## 의사 결정을 최적화 하라
모듈을 나누고 관심사를 분리하면 지엽적인 관리와 결정이 가능해진다. 
성급한 결정은 불충분한 지식으로 내린 결정이다. 

## 명백한 가치가 있을 때 표준을 현명하게 사용하라
표준을 사용하면 아이디어와 컴포넌트를 재사용하기 쉽고, 적절한 경험을 가진 사람을 구하기 쉬우며, 좋은 아이디어를 캡슐화 하기 쉽고
컴포넌트를 얻기 쉽다. 하지만 때로는 표준을 만드는 시간이 너무 오래 걸려 업계가 기다리지 못한다. 
어떤표준은 원래 표준을 재정한 목적을 잊어버리기도 한다.

## 시스템은 도메인 특화 언어가 필요하다
도메인 특화 언어(Domail-Specific Language)를 사용하면 고차원 정책에서 저차원 세부사항에 이르기까지 
모든 추상화 수준과 모든 도메인을 POJO로 표현할 수 있음.

## 결론
시스템 역시 꺠끗해야한다. 깨끗하지 못한 아키텍처는 도메인 논리를 흐리며 기민성을 떨어뜨린다.
도메인 논리가 흐려지면 제품의 품질이 떨어진다. 버그가 숨어 들기 쉬워지고, 스토리를 구현하기 어려워지는 탓이다.

모든 추상화 단계에서 의도는 명확히 표현해야 한다. 

시스템을 설계하든 개별모듈을 설계하든, 실제로 돌아가는 가장 단순한 수단을 사용해야 한다는 사실을 명심. 
