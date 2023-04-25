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

### The Dining- Philosophers Problem


![Untitled](https://user-images.githubusercontent.com/78361650/234279649-056f3a89-43ba-420c-911e-6876da9a1224.png)

여러 사람(의자,철학자)이 여러 자원( 젓가락 )을 사용할 때..

서로 같은 젓가락을 잡을수 있고 동시에 같은 젓가락을 잡을수도, 여러 경우가 생기게 되고 그런 상호 배제되지 않은 상태가 되어 critical section에 들어가지 못하게 된다.

**해결법** 

semaphore solution

1. 젓거락을 잡을떄 무조건 wait()를 호출한다음 release할때 signal()을 사용하는 방법 → 상호배제 해결

```jsx
semaphore chopstick[5];

while(true){
wait(chapstick[i]);
wait(chopstick[(i+1) % 5));
/* eat for a wihle*/ -> 임계영역

signal(chopstick[i])'
signal(chopstick[(i+1) % 5]);
```

이떄의 문제 → dealock,starvation(교착상태, 기아상태)이 발생할수 있다. → 5명이 한번에 사용하려고한다면?

→ 철학자의 숫자를 4명까지만 제한해볼까?

→ 양쪽 젓가락이 가능한지 확인을 먼저 하고 사용해볼까?

→ 홀수 번호는 왼쪽을 먼저 짚고 오른쪽을 사용, 짝수는 오른쪽 먼저 다음 왼쪽으로 규칙을 만들어볼까?

하지만 이렇게 하면 deadlock은 해결되지만  starvation은 해결이 안된다.

그리고 방지하는 비용이 방치 비용보다 비싸다…

**Monitor Solution**

양쪽이 다가능한지 확인후에 가능하면 사용해보자.

→ condition variable을 통해 wait, siganl등을 제어 해보자.

DiningPhilosoper Monitor(젓가락을 배분해주는 기능) → 젓가락 pickup, putdown등으로 먹는걸 감지함

서로서로 교환하면서 먹어도 리소스 크기에 따라  큰 리소스사용 근처의  철학자는 굶어 죽을수 밖에 없다..

Thread-safe 해결방법

1. Transactional Memory
2. OpenMp
3. Functional Programming Language
