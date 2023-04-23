# Synchronization Examples

동시성 제어 문제 (  대표적 동기화적인 문제들

- Bounded-Buffer Problem
- producer-Consumer Problem
- Readers -Writers Problem
- Dining-Philosopbers

### Bounded-Buffer Problem

고객과 관리자의 문제같은 느낌인데

관리자는 고객이 상품을 살수 있게 버퍼. 즉 상품을 채워 넣어야하고,

고객은 필요한걸 빨리 사서 상품칸을 비워야한다고 생각하면 된다.

- 즉   Producer가 Full Buffer를 만들고  Cunsumer가  empty buffer를 만들어낸다.

### Readers -Writers Problem

Bounded Buffer처럼 동작하지만 다른점은

어떤 데이터들은 읽기만 하고 어떤데이터는 읽고 쓰고를 둘다함

한 쓰레드에 Read와Write가 동시에 벌어질때
ex) 불합격으로 된 점수가 합격으로 write 되고 있는중에  read되면 합격이지만 불합격으로 표시되는 점

해결방법

- First readers - writers problem
- 다른 리딩이 끝날때까지 기다리지 말고 읽는다.
- Second Readers-writers problem
- Wirter 를 무조건 먼저 하게 한다.

### 이후 코드는 강의 및 문서 참조
