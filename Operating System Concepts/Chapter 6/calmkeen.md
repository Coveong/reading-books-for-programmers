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
