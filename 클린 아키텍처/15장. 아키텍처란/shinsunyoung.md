# 15장. 아키텍처란?

- 소프트웨어 시스템의 아키텍처
    - 시스템을 구축했던 사람들이 만들어낸 시스템의 형태

> 이러한 일을 용이하게 만들기 위해서는 가능한 한 많은 선택지를, 가능한 한 오래 남겨두는 전략을 따라야 한다.

- 시스템 아키텍처는 시스템의 동작 여부와는 거의 관련이 없다.
    - 주된 목적은 시스템의 생명주기를 지원하는 것
    - 좋은 아키텍처는 시스템을 쉽게 이해하고, 쉽게 개발하며, 쉽게 유지보수하고, 쉽게 배포하게 해준다.
    - 궁금적인 목표는 시스템의 수명과 관련된 비용은 최소화하고, 프로그래머의 생산성은 최대화하는데 있다.

# 개발

- 시스템 아키텍처는 개발팀이 싯스템을 쉽게 개발할 수 있도록 뒷받침해야만 한다.

# 배포

- 배포 비용이 높을수록 시스템의 유용성이 떨어진다.

**→ 소프트웨어 아키텍처는 시스템을 단 한 번에 쉽게 배포할 수 있도록 만드는 데 목표를 두어야 한다.** 

# 운영

- 좋은 소프트웨어 아키텍처는 시스템을 운영하는 데 필요한 요구를 알려준다.
- 시스템 아키텍처는 유스케이스, 기능, 시스템의 필수 행위를 일급 엔티티로 격상시키고, 이들 요소가 개발자에게 주요 목표로 인식되도록 해야 한다.

→ 이를 통해 시스템을 이해하기 쉬워지며, 따라서 개발과 유지보수에 큰 도움이 된다.

# 유지보수

- 유지보수할 때 탐사가 가장 큰 비용을 차지한다.
- 탐사
    - 기존 소프트웨어에 새로운 기능을 추가하거나 결함을 수정할 때, 소프트웨어를 파헤쳐서 어디를 고치는게 최선인지, 그리고 어떤 전략을 쓰는게 최적일지를 결정할 때 드는 비용
- 아키텍처를 신중하게 만들면 탐사의 비용을 줄일 수 있다.
    - 컴포넌트 분리 및 안정된 인터페이스를 두어 서로 격리하기

→ 의도치 않은 장애가 발생할 위험을 크게 줄일 수 있다.

# 선택사항 열어 두기

- 소프트웨어는 행위적 가치, 구조적 가치를 가진다.
    - 여기서 더 중요한건 구조적 가치 → 소프트웨어를 부드럽게 만들기 때문!
- 소프트웨어를 부드럽게 만들려면 선택사항을 가능한 한 많이, 오랫동안 열어두어야 한다.
    - 중요치 않은 세부사항
        - 데이터베이스 시스템
        - 웹 서버
        - REST ... 등등
    - 더 많은 정보를 얻을 수 있고, 기초로 제대로 된 결정을 내릴 수 있다.

> 좋은 아키텍트는 결정되지 않은 사항의 수를 최대화한다.

# 장치 독립성

- 장치로부터 독립되어야 한다.
- 어떤 장치를 사용할지 전혀 모른채, 그리고 고려하지 않고도 프로그램을 작성할 수 있어야 한다.

# 결론

- 좋은 아키텍트는 세부사항을 정책으로부터 신중하게 가려내고, 정책이 세부사항과 결합되지 않도록 엄격하게 분리한다.

    → 이를 통해 정책은 세부사항에 관한 어떠한 지식도 갖지 못하게 되며, 어떤 경우에도 세부사항에 의존하지 않게 된다.

- 좋은 아키텍트는 세부사항에 대한 결정을 가능한 한 오랫동안 미룰 수 있는 방향으로 정책을 설계한다.
