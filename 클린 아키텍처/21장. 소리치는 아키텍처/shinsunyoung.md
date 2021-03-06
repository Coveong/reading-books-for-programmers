# 21장. 소리치는 아키텍처

한 애플리케이션을 봤을 때

😮 이건 헬스 케어 시스템이야 ~ 

😮 이건 레일스(또는 스프링 또는 ASP)야 ~

어떻게 소리칠 것인가?!

# 아키텍처의 테마

아키텍처는 프레임워크에 대한 것이 아니다. 아키텍처를 프레임워크로부터 제공받아서는 절대 안 된다. **프레임워크는 사용하는 도구일 뿐**, **아키텍처가 준수해야 할 대상이 아니다.**

만약 프레임워크 중심으로 만들어버리면, 유스케이스가 중심이 되는 아키텍처는 절대 나올 수 없다.

# 아키텍처의 목적

좋은 아키텍처는 유스케이스를 중심에 둔다. 즉, 프레임워크나 도구, 환경에 전혀 구애받지 않고 유스케이스를 지원하는 구조를 아무 문제 없이 기술 할 수 있다.

**프레임워크는 열어 둬야 할 선택사항이다.**

# 하지만 웹은?

실제 애플리케이션을 웹으로 전달할지 여부는 미루어야 할 결정사항 중 하나다.

# 프레임워크는 도구일 뿐, 삶의 방식은 아니다

눈이 아플 정도로 면밀하게 프레임워크를 살펴보고, 비판적으로 봐보자.

# 테스트하기 쉬운 아키텍처

아키텍처가 유스케이스를 최우선으로 한다면 필요한 유스케이스 전부에 대해 단위 테스트를 할 수 있어야 한다. 즉, 아래와 같은 상황이 발생하면 안 된다.

- 테스트를 돌리는데 웹 서버가 반드시 필요한 상황
- 데이터베이스가 반드시 연결되어야하는 상황
- 복잡한 엔티티가 존재하는 상황
- 프레임워크에 의존하는 상황

# 결론

한 애플리케이션을 봤을 때

😮 이건 헬스 케어 시스템이야 ~ 

이렇게 나와야한다.

😮 모델처럼 보이는건 다 확인했는데, 뷰랑 컨트롤러는 어디에 있죠?

😀 아, 그건 세부사항이기 때문에 나중에 결정해도 됩니다.

이런 말을 할 수 있어야 한다.