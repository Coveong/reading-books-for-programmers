# 32장. 프레임워크는 세부사항이다

# 혼인 관계의 비대칭성

우리는 프레임워크에게 헌신을 해야한다. 즉, 프레임워크와 결합된다. 위험 요소를 부담해야하는 것도 우리이다.

# 위험 요인

- 프레임워크의 아키텍처에서 의존성 규칙을 위반하는 경향이 있다.
- 제품이 성숙해짐에 따라 프레임워크가 제공하는 기능과 틀을 벗어나게 될 것이다.
- 프레임워크는 우리에게 도움이 되지 않는 방향으로 진화할 수도 있다. 심지어 사용 중이던 기능이 사라지거나 반영하기 힘든 형태로 변경될 수도 있다.
- 더 나은 프레임워크가 등장해서 갈아타고 싶을 수도 있다.

# 해결책

> 프레임워크와 결혼하지 마라!

사용할 수는 있지만, 적당히 거리를 두고 아키텍처의 바깥쪽 원에 속하는 세부 사항으로 취급하라.

# 결론

프레임워크와 첫만남부터 바로 결혼하려 들지 말아라. 가급적이면 프레임워크를 가능한 한 오랫동안 아키텍처 경계 너머에 두자