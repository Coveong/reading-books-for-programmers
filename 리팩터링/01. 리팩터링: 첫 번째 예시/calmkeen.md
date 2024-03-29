# 0.시작하기 앞서

저는 코드를 잘 정리하지 않는 독자입니다. 하지만 이책은 코드읽기를 강력하게 권합니다. 코드로 봐야만 알수 있는 부분도 많고 실제로 적용할때 큰 도움이 됩니다. 개념적인 부분만 다루고 있기 때문에 실제코드에 녹여보려면 책을 구매하셔서 보시길! 정말 강추드립니다.

### 1.1 자 시작해 보자

연극 외주를 받아서 공연하는 극단에 대한 코드(책 참고)

### 1.2 예시 프로그램을 본 소감

그럭저럭 쓸만함.

설계가 나쁜 시스템은수정하기 어렵다. 원하는 동작을 위해 수정해야 할 부분을 찾고, 기존 코드와 잘 맞물려 작동할게 할 방법을 강구하기가 어렵기 때문이다.

```jsx
**프로그램이 새로운 기능을 추가하기 편한 구조가 아니라면,
 먼저 기능을 추가하기 쉬운 형태로 리팩터링하고 나서 원하는 기능을 추가한다.**
```

### 1.3 리팩터링의 첫 단계

리팩터링의 첫단계는 리팩터링할 코드 영역을 꼼꼼하게 검사해줄 테스트 코드들 부터 마련해야 한다.

```jsx
**리팩터링하기 전에 제대로 된 테스트 부터 마련한다.
테스트는 반드시 자가진단하도록 만든다.**
```

### 1.4  statement함수 쪼개기

테스트는 내가 저지른 실수로부터 보호해주는 버그 검출기 역할을 해준다.

```jsx
**리팩터링은 프로그램 수정을 작은 단계로 나눠 진행한다.
그래서 중간에 실수하더라도 버그를 쉽게 찾을 수 있다.**
```

자바스크립트와 같은 동적 타입 언어를 사용할는 타입이 드러나게 작성하면 도움이 된다.

```jsx
**컴퓨터가 이해하는 코드는 바보도 작성할 수 있다.
사람이 이해하도록 작성하는 프로그래머가 진정한 실력자다.**
```

좋은 코드라면 하는 일이 명확히 드러나야 하며, 변수 이름은 커다란 역할을 한다.

지역변수를 제거하면 얻는 가장 큰 장점은 추출 작업이 쉬워진다는 것이다,

리팩터링이 성능에 상당한 영향을 주기도 한다. 그런 경우라도 리팩터을 한다. 잘 다듬어진 코드라야 성능 개선 잡업도 수월하기 때문이다.

그리고 책에서 중요시하는 변수 제거의 단계가 있다.

1. 반복문 쪼개기 - 변수 값 누적 분리
2. 문장 슬라이드하기 - 변수 초기화 문장을 변수 값 누적 코드 앞으로 옮긴다.
3. 함수 추출하기 - 적립 포인트 계산 부분을 별도 함수로 추출
4. 변수 인라인하기- 나누어진 불필요 변수 제거

### 1.5 중간점검:난무하는 중첩 함수

결과적으로 최상위 함수를 줄였으면 출력 문장 및 로직을 보조 함수로 빼냈다. → 로직이해가 쉬워졌다.

### 1.6 계산 단계와 포맷팅 단계 분리하기

쪼개 놓은 함수에 대해 대응하는 코드를 짤때(책에서는 html) 쓸수 있는 방법 중 하나는 **단계 쪼개기**(6.11절에서 설명) 이다.

메인 로직을 두단계로 나누어 첫 로직에서 데이터 처리 다음단계 에서 데이터 및 결과 값 처리 표시 한다.

### 1.7 중간점검

결과적으로 코드의 양은 늘었지만, 추가된 코드 덕분에 로직 구성 요소가 뚜렷이 부각되고, 계산하는 부분과 출려 형식을 분리시키는 코드가 작성되었다. 결과적으로 과정을 파악하기 쉬워진다.

```jsx
**캠핑자들에게는 "도착했을 때보다 깔끔하게 정돈하고 떠난다"는 규칙이 있다.
프로그래밍도 마찬가지다.
코드베이스를 작업 시작 전보다 건강하게 만들어놓고 떠나야 한다.**
```

### 1.8 다형성을 활용해 계산 코드 재구성하기

조건부 로직을 명확한 구조로 보완하는 방법은 다양하지만, 객체지향의 특징인 다형성을  활용하는 것이 좋다.

- 상속 계층을 구성하여 두가지(서로다른) 서브 클래스가 각자의 구체적인 계산 로직 정의하기
- 조건부 로직을 다형성으로 바꾸기(10.4절에서 설명)를 활용한다.

## 마치며

- 함수 추출하기 / 변수 인라인하기 / 함수 옮기기 / 조건부 로직을 다형성을 바꾸기등의 리팩터링 기법이 적혀있다.
- 리팩터링은 대부분 코드가 하는 일을 파악하는 데서 시작한다.

```jsx
**좋은 코드를 확실한 방법은 얼마나 수정하기 쉬운가 이다.**
```

리팩터링을 효과적으로 하는 핵심은 , 단계를 나누고 , 이러한 작은 단계들이 모여서 변화를 이룬다.
