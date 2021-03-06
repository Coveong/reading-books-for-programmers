# 04장. 구조적 프로그래밍

에츠허르 비버 데이크스트라는 사람이 1930년도에 태어났고, 1952년에 최초의 프로그래머로 취업했다.

# 증명

프로그래밍 ... 너무 어렵다!

goto보다는 if/then/else & do/while가 더 좋은 사용 방식인 것 같은데 ...

→ 모든 프로그램은 **순차, 분기, 반복**으로 표현할 수 있지 않을까?

1. 순차
    - 순차 구문의 입력을 순차 구문의 출력까지 수학적으로 추적
    - 일반적인 수학적 증명 방식과 같음
2. 분기 
    - 열거법 재적용
    - 분기를 통해 각 경로를 열거
    - 두 경로가 수학적으로 적절한 결과를 만들어냄
3. 반복
    - 귀납법 사용
    - 열거법
        - 1의 경우가 올바르다.
        - N의 경우가 올바른 경우에는 N + 1의 경우도 올바르다.
        - 반복의 시작 조건과 종료 조건

# 해로운 성명서

goto는 좋지 않다는 성명서를 냄 → 반응이 극과극으로 갈림 → 10여년을 싸움

결국 구조적 프로그래밍이 승리했다.

# 기능적 분해

구조적 프로그래밍을 통해 모듈을 증명 가능한 더 작은 단위로 재귀적으로 분해가 가능해짐

→ 모듈을 기능적으로 분해할 수 있음

# 수학과 과학

- 수학
    - 증명 가능한 서술이 '참'이라는 것을 입증하는 원리
- 과학
    - 증명 가능한 서술이 '거짓'임을 입증하는 원리

# 테스트

테스트는 버그가 있음을 보여줄 뿐, 버그가 없음을 보여줄 수는 없다.

소프트웨어는 과학과 같다. 

→ 최선을 다하더라도 올바르지 않음을 증명하는 데 실패함으로써 올바름을 보여주기 때문

# 결론

소프트웨어 아키텍트는 모듈, 컴포넌트, 서비스가 쉽게 반증 가능하도록(테스트하기 쉽도록) 만들기 위해 분주히 노력해야 한다. 이를 위해 구조적 프로그래밍과 유사한 제한적인 규칙들을 받아들여 활용해야 한다.