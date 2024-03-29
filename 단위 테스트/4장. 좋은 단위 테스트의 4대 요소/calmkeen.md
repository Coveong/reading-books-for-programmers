# 4. 좋은 단위 테스트의 4대 요소

앞서 봤던 좋은 단위 테스트 스위트의 특성

- 개발 주기에 통합돼 있다. 실제로 사용하는 테스트에만 가치가 있다.
- 코드베이스의 가장 중요한 부분만을 대상으로 한다. 모든 실행 코드에 똑같이 신경쓸 필요가 없다.
- 최소한의 유지비로 최대 가치를 끌어낸다.
- 가치 있는 테스트(낮은 가치의 테스트) 식별
- 가치 있는 테스트 작성

## 4.1 좋은 단위 테스트의 4대 요소 자세히 살펴보기

- 회귀 방지
- 리팩터링 내성
- 빠른피드백
- 유지 보수성

위 특성으로 어떤 자동화된 테스트도 분석할 수 있다.

### 4.1.1  첫 번째 요소 : 회귀 방지

회귀방지 지표에 대한 고려 사항

- 테스트 중에 실행되는 코드의 양
- 코드 복잡도
- 코드의 도메인 유의성

일반적으로 코드가 많을수록 테스트에서 회귀가 나타날 가능성이 높다.

복잡한 비지니스 로직을 나타내는 코드가 보일러프렐이트 코드보다 훨씬 더 중요하다.

최상의 보호를 위해서는 테스트가 해당 lib, frameWork, 외부 시스템을 테스트 범주에 포함시켜 의존성에 대한 검증을 해야한다.

### 4.1.2 두 번쨰 요소: 리팩터링 내성

테스트를 실패로 바꾸지 않고 기본 애플리케이션 코드를 리팩터링할 수 있는지에 대한 척도이다.

즉 , 식별할 수 있는 동작을 수정하지 않고 기존 코드를 변경하는 것을 의미

거짓양성: 실제로 기능이 의도한 대로 작동하지만 테스트는 실패를 나타내는 결과이다.

리팩터링 내성의 장점

기존 기능이 고장났을 때 테스트가 조기 경고를 제공한다.운영환경에 제공되기 전에 문제를 해결하게 해준다.

코드 변경이 회귀로 이어지지 않을 것이라고 확신하게 된다.

거짓양성이 가지는 단점

 테스트가 타당한 이유 없이 실패하면, 코드 문제에 대응하는 능력과 의지가 희석된다. 즉 시간이 흐르면 신경쓰이지 않는다.

테스트 스위트에 대한 신뢰가 서서히 떨어지며, 더 이상 믿을 만한 안전망으로 인식하지 않는다.

4.1.3 무엇이 거짓 양성의 원인인가?

테스트와 테스트 대상 시스템의 구현 세부 사항이 많이 결합할수록 허위 경보가 더 많이 생긴다.

거짓양성을 줄이는 방법 :  구현 세부 사항에 테스트를 분리하는 것 뿐이다.

테스트를 구성하기에 가장 좋은 방법은 문제 영역에 대해 이야기하는 것이다.

![1](https://user-images.githubusercontent.com/78361650/189584383-013299c2-fe7d-499c-82ae-d137c6152075.png)



4.1.4 구현 세부 사항 대신 최종 결과를 목표로 하기

## 4.2 첫 번째 특성과 두번째  특성간의 본질적인관계

![2](https://user-images.githubusercontent.com/78361650/189584506-bdd398cc-25d3-4e2a-ad40-fe0a95ff1f2f.png)


회귀 방지와 리팩터링 내성은 테스트 스위트 정확도를 극대화하는 것을 목표로 한다.

정확한 지표의 두가지 요소

- 테스트가 버그 있음을 얼마나 잘 나타내는가
- 테스트가 버그 없음을 얼마나 잘 나타내는가

테스트 정확도 = 신호(발견된 버그 수)/ 소음(허위 경보 발생 수)

### 4.2.2 거짓 양성과 거짓 음성의 중요성: 역학 관계

![3](https://user-images.githubusercontent.com/78361650/189584524-372f84ec-c12b-473a-8024-1877b583d637.png)


> 참조 : [https://velog.io/@mabr2845/단위-테스트-4장-좋은-단위-테스트의-4대-요소](https://velog.io/@mabr2845/%EB%8B%A8%EC%9C%84-%ED%85%8C%EC%8A%A4%ED%8A%B8-4%EC%9E%A5-%EC%A2%8B%EC%9D%80-%EB%8B%A8%EC%9C%84-%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%9D%98-4%EB%8C%80-%EC%9A%94%EC%86%8C)
> 

거짓양성(허위 경보)은 초기에 그다지 부정적인 영향을 미치지 않는다. 그러나 프로젝트에 거짓 음성(알려지지 않은 버그)이 주용한 만큼 거짓 양성도 점점 더 중요해진다.

프로젝트 초기에는 개발자의 기억에 남아 아직 생생하기에 문제가 없지만 시간이 흐를수록 복잡하고 체계적이지 않게 된 코드는 리팩터링을 해야 한다.

## 4.3 세 번째 요소와 네 번째 요소: 빠른 피드백과 유지 보수성

테스트 속도가 빠를 수록 테스트 스위트에서 더 많은 테스트를 수행할 수 있고, 더 자주 실행할 수 있다.

느린 테스트는 피드백을 느리게 하고 잠재적으로 버그를 뒤늦게 눈에 띄게 해서 버그 수정 비용이 증가한다.

유지 보수성에 대한 지표는 두가지로 구분된다.

- 테스트가 얼마나 이해하기 어려운가:  테스트 코드 크기와 관련 있다. 테스트를 작성할 떄 절차를 생략하지 마라. 일급 시민으로 취급하라.
- 테스트가 얼마나 실행하기 어려운가: 테스트가 프로세스 외부 종속성으로 작동하면, 데이터 베이스 서버를 재부팅하고 네트워크문제 해결 등 의존성을 상시 운영하는 데 시간을 들여야 한다.

## 4.4 이상적인 테스트를 찾아서

테스트 코드르 포함한 모든 코드는 책임이다. 최소한으로 필요한 가치로 임계치를 상당히 높게 설정하고 이 임계치를 충족하는 테스트만 테스트 스위트에 남기자.

이상적인 테스트를 만드는 것은 거의 불가능하다.

회귀 방지, 리팩터링 내성, 빠른 피드백은 상호 배타적이기 떄문이다. 최대한 둘이라도 최대로 유지하자.

1. 엔드 투 엔드 테스트 : 많은 코드를 테스트하므로 회귀 방지를 해내며, 거짓양성에 면역 돼 리팩터링 내성도 우수하다. 하지만  느리다. 피드백을 빨리 받을수 가 없다.
2. 간단한 테스트 :  매우 간결한 코드를 사용하기 떄문에 빠르고  거짓양성 또한 생길 가능성이 낮아 리팩터링 내성도 우수하다. 하지만 간단한 테스트는 회귀 방지가 없다.
3. 깨지기 쉬운 테스트 :  빠르고 회귀방지가 가능하지만 리팩터링 내성이 전혀 없다.

## 4.5 대중적인 테스트 자동화 개념 살펴보기

### 4.5.1 테스트 피라미드

![4](https://user-images.githubusercontent.com/78361650/189584635-1a192786-edb1-4058-8013-6d7336bbcaa4.png)


피라미드 테스트는 빠른 피드백과  회귀 방지 사이에서 선택한다. 엔드 투 엔드 테스트는 회귀 방지에 유리하고, 단위 테스트는 빠른 피드백을 강조하며, 통태는 그중간이다.

### 4.5.2블랙박스 테스트와 화이트박스 테스트 간의 선택

블랙박스 테스트 :  시스템의 내부 구조를 몰라도 시스템의 기능을 검사할수 있는 방법

화이트 박스 테스트 : 애플리케이션 내부 작업을 검증하는 테스트 방식

![5](https://user-images.githubusercontent.com/78361650/189584656-d32d8e64-5f6a-41b7-96a1-c8752ae124a9.png)
