### Cooperating processes

서로 영향을 주거나 영향을 받거나 한다.

물리적 주소를 공유하거나 데이터를 공유하거나 한다.

쓰레드의 데이터 무결성 공유

- Concurrent executon -  프로세스의 경쟁은 언제 다른 활동이 더 추가될지모른다.
- Parallel exxecution - 여러 프로세스가 분리된 코어에서 실행되면 동기화 문제 발생한다.

### race Condition(경쟁상태)

여러 프로세스의 데이터를 공유시 이를 동시에 처리하려고 하면 어떤 순서에 따라  일이 일어난다라고 하는것에 따라 달라지는것

예시 ) 계좌 이체 출금과 입금이 동시에 이루어 져야함

**해결 방법**

시간에 따라한개의 프로세스만 데이터를 공유할수 있게 한다.

이를 Process Synchronization이라고 한다.

### Critical section Problem (임계영역)

다른 역역의 프로세스와 공유되고 있다면 이를 임계영역이라고 부른다.

이때  프로세스가 임계영역에서 실행되는걸 확인했을때는 다른 프로세스에서 임계영역에 진입하지 않게 하자 → race Condition이 되지 않음

Entry section - 임계 영역에 들어가는 섹션

Critical section - 엔트리이후의 임계영역

Exit section 임계영역 진입 해서 나왔다는걸 알려주는 섹션

Remainder section

## 목표(중요)

**Mutual exclusion** - 상호배제를 존중해 주어야 한다.

이때 문제가 생기는데

 데드락이 발생하고 

기아(한정대기)가  발한다.

DeadLock(데드락) 해결법

들어간 프로세스 식별 확인???

starvation(기아)의 해결법

 기다리눈 시간에도 제한을 둔다

싱글코어에서 접근하지 못하게 막는 방법

인터럽트를 발생하지 못하게 아예 사전에 막아버린다.

### 크리티컬 섹션의 소프트웨어 해결책

Dekker Algorithm

- 2개의 프로세스에 대해서

**EisenBerg & McGurie’s Algorithm**

- n개의 프로세스에 대해서 waitingTime이  n-1을 가지는 알고리즘 입니다.

### Peterson Algorithm

critical-section Solution즉 임계영역에 대한 문제를 가장 완벽하게 해결한 알고리즘

- load and store 아키텍쳐에서 발생하는 알고리즘이다.

해결방법

- 2개의 프로세스라고 했을 때 크리티컬 섹션으로 이동했다가  나머지 섹션으로 이동했다.

```jsx
while(ture) {
flag[i] = true
turn = j
while(flag[j]&&ture ==j) 
/* critical section */
flag[i] = false;
/* remainder section */
}
int turn;
boolean flag[2]

```

코드로 구현해보았을때..

해결이 되지 않는다.. 왜? ( 자세한 내용은 강의와 서적 참고)

- no guarantees that…(?)
- 기계어 레벨로 생각을 해보아야 한다.
- 피터슨 솔루션은 권한적인 부분을 정확히 해야 제대로 동작하게 만들어 졌다.
- 그래서 알고리즘의 개념, 증명적으로는 훌륭한다.(데드락, 한계 대기 등)

하지만 hardware의 지원도 필요하다.

csp(critical section problme)을 푸는데 지원을 해줄수 있는데

- 하드웨어에서 동기화 지침을 제공해주면 좋고
- 하드웨어를 방법을 제공하는데 필요한 하나의 도구로 생각해서 사용할수도 있다.

3가지 원시적인 방법이 있다.

- 메모리 배리어 , 메모리 팬스
- 하드웨어 지침서
- 원자성있는 변수
    - 말하는 원자 → 더이상 쪼갤수 없는 변수

## CSP를 해결하기 위한 software  Tool

Mutex Locks :  동기화 해결을 위한 가장 심플한 툴

Semaphore : n개에서더 좋고,편리하고 효과적인 툴

Monitor:  Mutex locks와 Semaphore를 해결하는 툴

Liveness: 프로세스의 동작을 보장 → 데드락 해결

### MutexLcok

mutex :상호 배재

크리티컬 섹션을 보호하고 race condition을 예방해 준다.

프로세스는 열쇠를통해서 진입하고 나온다. → 열쇠를 통해 크리티컬 섹션에 들어가고 나오며, 이를 통해 크리티컬 섹션을 하나의 프로세스만 동작할수 있게 해준다.( 교착 방지 )

### Busy waiting

프로세스가 critical section에 접근하기 위해서 계속 시도를 함 → 무한루프 → cpu소모로 이어짐(계속 쓸데없는 접근 , 블락이 계속되기때문)

### Spinlock

MutexLock을 사용해 busy waiting을 하는 경우

이경우가 유용할떄가 있다? → cpu코어가 여러개면 다른 세션에서 계속 루프를 돌다가 끝날경우 바로 섹션에 접근하게 됨 → context switch를 안하게 함 → 시간을 줄여준다.

## Semaphores

신호 장치

초기활르 어떻게 해주냐에 따라   (Proberen - to test)P()  와 (Verhogen - to increment) V(),혹은wait() and signal()이라고 한다.

- Mutex lock처럼 키 방식이 아닌 일정 값의 추가 감소로 구분한다.

특징

- 여러개의 인스턴스를 가진 자원에 쓰기 용이하다.
- wait를주고 어떤 Semaphore에서 사용하는지 가르쳐주면 값을 감소시키면서 Semaphore가 언제 끝나는지 기다린다
- 그리고 프로세스의 릴리즈를 확인하면  sinal로 증가신다.

## Monitors

Semaphore는 편리하고 효과적이지만 타이밍 에러가 자주 발생한다.

- semaphore를 1로 초기화 하고  wait하기전에 signal을 보내면 문제가 된다.

이를 문제되지 않게 하기 위해

- 고급진 모니터( high Level- language)를 사용한다.

Monitor type이란건 ADT(abstract data type)로써 mutual exclusion(상호배제)을 제공해주는 하나의 클래스라고 이해하면 좋다.

변수를 선언하고 정의된 함수를 호출하게 해준다.

```jsx
class Monitor() {
func p1(){}
func p2(){}
frunc pn(){}
}
```

Conditionalm Variables

모니터가 의 부족한 점을  condition이라는 친구로 보안해준다.

```jsx
condition x, y;
x.wait()
x.signal()
```

### java Monitors

자바에서는 유사한 기능으로  monitor like을 사용한다.

synchronized keyword 

자바에서 쓰는 코드 블록선억 키워드

 임계영역에 모니터락을 획득해야 진입 가능하게 한다.

모니터락을 가진 객체를 지정할수 있다.

```jsx
synchronized(object){/* critical section*/}

public synchronized void add() {}
```

## Liveness

progress(deadLock)와 bounded-waiting(한계대기)를 해결해 주는 것

deadLock & priortity inversion(우선순위 역전)

### DeadLock

waiting Queue에 있는 프로그래스만 실행시려고 했을때 영원히 기다리는 상황이 된다.

```jsx

p0
wait(s) wait(Q)
위 두개가 순서대로 있어야 cs(critical section)에 들어갈수 있고 나올때도
signal(S),Signal(Q)로 나와야한다.
그런데
p1
wait(Q)wait(S)에서 qs로 하면 p0에서 SQ순서로 호출하면 둘다 서로를 기다리게 된다...
```

### priority inversion

우선순위가 높은 프로세스가 낮은 프로세스에 밀리는 현상

해결방법

- priority-inheritance - 우선순위 상속
