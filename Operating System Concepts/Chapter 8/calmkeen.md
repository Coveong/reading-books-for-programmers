# 8. Deadlocks

deadlock 이란?

- 모든 프로세스가 대기 상태에서 기다리고 있을때
- 세트의 다른 프로세스에 의해서만 발생할 수 있는 이벤트
- 쓰레드가 대기 상태에서 다른상태로 변경할수 없을때
- 리소스가 다른 스레드에 있어서 점령되어 있을때

시스템을 고려해보자

- 리소스타입을 고려해보자.
- 같은  인스턴스 →  cpu cycles files, i/o device 같은 경우 리소스타입이 중요 갯수는 중요하지 않음
- 위는  rqeust - use - Release의 단계를 거치는데   use가 크리티컬 섹션이다.

데드락의 필수 4가지 조건

1. Mutual Exclusion( 상호배제)
- 마지막 리소스가 상태를 잡고 공유하지 않는 경우
2. Hold and Wait( 점유 대기)
- 쓰레드가 어떤 자원을 점유한상태에서 대기를 해야만 일어난다.
3. No preemption(선점 불가)
- 자원이 선점이 불가능 할때
4. Circular Wait(원형대기)
- 데드락걸린 경우에는 무조건적으로 원형대기가 일어나고 있다.

### Resource-Allocation Graph(자원 할당 그래프)

- 데드락을 이해하기 위한 그래프

V == Vertieces

E == Edges

-노드의 두가지 종류

T = active Thread

R = Resource types

- 중간로직이 이해가 안가서 나중에 추가 정리하겠습니다

중요한 점은..

사이클이 없으면 데드락은 발생하지 않는다.

사이클이 있으면..? RAG 그래프의  사이클 디텍션을 잘 해야한다..

데드락에 대한 데처.

1. 무시하기
2. 발생하지 않게 방지하거나 회피
- 방지는 거의 불가능하다.
- 회피 →   Bankers algorithm
3. 데드락 발생 감지, 복구.

### Deadlock prevention

4가지 필수 요소 하나라도 되지 않게 막아보자!

**Mutual Exclusion**

모든 리소스를 공유 할수 있게 하자! → 불가능.. 어떻게 모든것을 공유할수 있어… mutex lock,eg 이런것은 공유되지 않음

**Hold and Wait**

공유하기 전 모든것을 버린후에 다시 점유를 한다. → 실용성 측면에서 불가능

**No preemption**

강제로 점유를 하자.사용중이라면 뺏어버리자. → 그럼 동작되고 있던 프로세스가 망가질수 있다.

**Circular Wait**

리소스마다 번호를 부여한다. → 요청을 할때 사용중인 번호 다음번후로만 요청을 하게 한다. 
단점 : starvation(기아상태)발생이 높아진다.

### 데드락 예방의 디메리트.

데드락의 4요소를 막아보려 한결과

사이드 이펙트가 너무 많이 터짐

막기위한 제한 요소가 많음

디바이스 유틸이 떨어짐

시스템 처리량 문제

## Deadlock Avoidance( 데드락 기피)

- 요청이 왔을때 요청을 받기전에 미래의 데드락을 미리 확인해본 후에 문제될경우 대기시킨다.

조건 : 이방식은 리로스 요청의 방식을 알아야만 가능하다.

데드락 어보이던스를 하려면,최대 자원의 수를 파악해야하는데,

그 상태는 가능한 리소스의 개수와 할당한 리소스의 갯수를  알고

최대 쓰레드의 수요를 알고 있어야 데드락 어보이던스를 쓸수 있다.

### Safe State:

safe state(안전상태)

- 시스템을 할당할수 이쓴 자원이 쓰레드의 실행순서가 있고 그걸 찾아 할당할수 있다면 데드락을 피할수 있다.
- 시스템이 안전상태에 있따는 것은  **safe sequence**에 따라서(쓰레드 실행순서) 자원들의 작업을 할당해 줄수 있다.

### Basic facts(기본사실)

- safe State는 데드락이 발생할수 없는 상태
- 데드락은 안전하지 않은상태에만 있다.
    - 모든 불안전 상태가 데드락을 발생시키지는 않지만, 데드락이 발생하는 환경은 불안전 상태이다.

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/04bd2f06-500e-4343-b6eb-45e198db6eca)

- 그럼  safe state에서 2가지 알고리즘을 구현한다.
    - single instance
    - mutiple instance
- 아니면 데드락 스페이스에 접근하지 않는다, 즉 안전 상태에만 게속 유지한다.
- 시스템 초기 상태는 안전상태이기 떄문에 그 이후 요청에 대해서 판단해본다.
- 데드락이 발생 하지 않으면 자원을 부여해주고, 그렇지 않으면 거절한다.

Banker algorithm

- RAG는 자원을 할당하는 시스템인데 이걸로 사이클을 찾을경우, 멀티플 인스턴스일 경우에는 할당할 수 없다.
- 이를 위해 벙커스 알고리즘을 채택했다.
- 뱅커인 이유?
    - 은행은 절대로 여러사람에게 한번에 작업하지 않기 때문에.

### Data Structures

- n = thread count
- m = resource types

available = available reousrce types(가능한 자원 타입)

max : maximum demand(최대 가능 양)

allocation currently allocated(최근 할당)

need: remaining reource(앞으로 요청할 리소스)

Safety structure

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/79bb2ced-c6ec-480b-91de-b93eb6e8ccf8)
알고리즘

이게..뭐..무슨소리야..?

교수님도.. 보고 이해하기 힘들꺼니.. 알고리즘 풀때 띄어놓고.. 보라고하신다… 알아만 두도록하자.. 절대 이해를 완벽히 못해 못쓴게 아니다.

**Safety algorithm**

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/ca002d07-b422-499b-b7c5-8f1d65b23550)


**Resource-Request algorithm**

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/6a77b4a9-1edc-4252-bb09-0c02868797a5)

결과 - 문제를 띄워놓고 위 순서를 띄워도 풀수 없다. 나는 망했다.이해를 하고 화면을 끄면 기억이 지워진다.

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/b7e61a72-a057-467e-82e5-dbb85d0473c5)
위문제…간단하게 이해한대로라면..

allocation에서 

A는 전체 t0~ t4 까지 7개 / B는 2개 / C는 5개

resource type이 10 / 5/ 7 이니까 .

available 3 /3 / 2

Need가 없네요.  need는

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/9d157e24-5edf-4c0e-a0df-3c07f552c910)

위 값인데 Max - allocation을 한 값이다..

… 그다음에… 진도가 너무 빠르다..?? 벡터가… 뭐… 예..  어.. 알고리즘 더 하고 오겠습니다.. 이글을 보시는 분은 강의를 보시길 바랍니다.20분정도..부터..40분..정도..까지..?40분 이후로 요약해주십니다.

## Deadlock Detection

예방(prevent)이나 (avoid)회피하지않으면 데드락이 발견할수 있는데 이는 리소스가 많이 탄다.

그럴빠에 감지를 하자.감지되면  recover 즉 다시 회복시킨다.

Several Instance 에서는 어떻게 할까?

멀티플 인스턴스의 경우 → 뱅커스 알고리즘을 적용하면 된다.

디텍션 알고리즘에서 데드락이 발견되면.?

- 필요의 경우에 따라 일정 주기로 감지를 계속 해서 모니터링을 자주 해준다. (빠른 발견)
- 우리가 아는 점검시간등을 이용한 재실행
- 급한경우 리커버리

deadlock recovery

1. 데드락이 걸린 쓰레드 한 가지씩  kill시켜버리기.
2. 자원을 선점 해버린다.
