# 27장. 크고 작은 모든 서비스들

서비스 지향 아키텍처, 마이크로서비스 아키텍처는 최근에 큰 인기를 끄는데, 그 이유는

- 서비스를 사용하면 상호결합이 철저하게 분리되는 것처럼 보인다.
    - 나중에 보겠지만, 일부만 맞는 말이다.
- 서비스를 사용하면 개발과 배포 독립성을 지원하는 것처럼 보인다.
    - 이것도 일부만 맞는 말이다.

# 서비스 아키텍처?

모놀리틱/마이크로 서비스 아키텍처는 아키텍처 경계를 넘나드는 함수 호출들이다. 시스템의 나머지 많은 함수들은 행위를 서로 분리할 뿐이며, 아키텍처적으로는 전혀 중요하지 않다.

서비스도 프로세스나 플랫폼 경계를 가로지르는 함수 호출에 지나지 않는다.

# 서비스의 이점?

## 결합 분리의 오류

시스템을 서비스로 분리하면 결합이 분리되겠지? 라는 생각을 많이 가진다.

→ 개별 변수로는 결합이 분리되지만, 프로세서 내의 또는 네트워크 상의 공유 자원 때문에 결합될 가능성이 여전히 존재한다. 오히려 오가는 데이터들때문에 더 강력하게 결합될 수도 있다. 

## 개발 및 배포 독립성의 오류

서비스는 확장 가능한 시스템을 구축하는 유일한 선택지가 아니다.

서비스라고 해서 항상 독립적으로 개발하고, 배포하며, 운영할 수 있는 것은 아니다. 데이터나 행위에서 어느 정도 결합되어 있다면 결합된 정도에 맞게 개발, 배포, 운영을 조정해야만 한다.

# 야옹이 문제

서비스들이 결합되어있으면 절대 독립적으로 개발, 배포, 유지가 불가능하다.

이게 바로 횡단 관심시가 지닌 문제다. 모든 소프트웨어 시스템은 서비스 지향이든 아니든 이 문제에 직면하기 마련이다. 

# 객체가 구출하다

다형성

템플릿 메서드나 전략 패턴등을 통해 오버라이드한다.

# 컴포넌트 기반 서비스

서비스에도 물론 적용 가능하다. 

서비스를 하나 이상의 jar 파일에 포함되는 추상 클래스들의 집합이라고 생각하라. 

1. 새로운 기능 추가 혹은 확장은 새로운 jar 파일을 만든다. 
2. 이 때 새로운 jar 파일을 구성하는 클래스들은 기존 jar 파일에 정의된 추상 클래스들을 확장해서 만들어진다.
3. 새로운 기능 배포는 서비스를 재배포하는 문제가 아니라, 서비스를 로드하는 경로에 단순히 새로운 jar 파일을 추가하기만 하면 된다.

→ 새로운 기능을 추가하는 행위가 OCP를 준수한다.

# 횡단 관심사

횡단 관심사를 처리하려면, 서비스 내부는 의존성 규칙도 준수하는 컴포넌트 아키텍처로 설계해야 한다. 이 서비스들은 시스템의 아키텍처 경계를 정의하지 않는다. 아키텍처 경계를 정의하는 것은 서비스 내에 위치한 컴포넌트다.

# 결론

서비스는 시스템의 확장성과 개발 가능성 측면에서 유용하지만, 그 자체로는 아키텍처적으로 그리 중요한 요소는 아니다.