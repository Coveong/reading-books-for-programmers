- https://www.inflearn.com/course/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98#curriculum
- 주니온 박사님 강의 중 Chapter 4에 대한 정리

# Chapter 4. Thread & Concurrency (Part 1)
지금까지 배운 내용
- 프로세스는 단일 제어 스레드로 실행 중인 프로그램
- 하지만 여러 제어 스레드를 들고 있을 수 있다.

## 스레드
- a lightweight process.
- CPU를 점유하는 기본 단위
- 스레드 ID, 프로그램 카운터, 레지스터 세트 및 스택으로 구성됨.

### 싱글스레드와 멀티스레드 프로세스
<img width="857" alt="image" src="https://user-images.githubusercontent.com/32327475/216807550-8b12f065-9124-4276-80a0-76bb3ebbcd7a.png">

### 멀티스레딩의 이점
- 응답성(Responsiveness): 지속적인 실행이 가능하다.
  - 프로세스의 일부가 차단된 경우, 특히 UI가 있는 경우 중요하다.
- 리소스 공유: 스레드는 프로세스 리소스를 공유한다.
  - 공유 메모리 또는 메시지 저장보다 쉽다.
- 경제성(Economy): 프로세스 생성보다 저렴하다.
  - 스레드 스위칭은 컨텍스트 스위칭보다 오버헤드가 낮다.
- 확장성: 프로세스가 멀티프로세서 아키텍처를 활용할 수 있다.

## Java에서의 스레드
Java에서 스레드를 명시적으로 만드는 세가지 방법: 
- Thread 클래스 상속
- Runnable 인터페이스 구현
- 람다 표현식 사용(Java Version 1.8 이후)

### Thread 클래스 상속
```java
class MyThread1 extends Thread { ... }

public class ThreadExample1 {
  public static final void main(String[] args) { 
    MyThread1 thread = new MyThread1(); thread.start();
    System.out.println("Hello, My Child!");
  } 
}
```

### Runnable 인터페이스 구현
```java
class MyThread2 implements Runnable { ... }

public class ThreadExample2 {
  public static final void main(String[] args) { 
    Thread thread = new Thread(new MyThread2()); thread.start();
    System.out.println("Hello, My Runnable Child!");
  } 
}

```

### 람다 표현식 사용(Java Version 1.8 이후)
```java
public class ThreadExample3 {
  public static final void main(String[] args) {
    Runnable task = () -> { ... };
    Thread thread = new Thread(task); thread.start();
    System.out.println("Hello, My Lambda Child!");
  } 
}
```

## 멀티코어 시스템의 멀티스레딩
- 여러 코어를 보다 효율적으로 사용하여 동시성을 향상시킬 수 있다.
- 단일 코어: 스레드는 시간이 지남에 따라 인터리빙됨.
- 다중 코어: 일부 스레드가 병렬로 실행될 수 있다.

### 멀티코어 시스템에서 고려해야 할 점
- 작업 식별: 별도의 작업으로 나눌 수 있는 영역 찾기
- 균형: 동일한 가치의 동일한 작업을 수행하도록 보장
- 데이터 분할: 별도의 코어에서 실행하기 위해 데이터 분할해야 한다.
- 데이터 종속성: 데이터 종속성을 수용할 수 있도록 작업 실행이 동기화되도록 보장
- 테스트 및 디버깅: 단일 스레드보다 어렵다.

### Amdahl’s Law

코어는 무조건 많을수록 좋은가?
- 𝑠𝑝𝑒𝑒𝑑𝑢𝑝 <=1/[𝑆+{(1−𝑆)/𝑁}]
- 𝑺: 어떤 시스템에서 serially 하게 실행될 수 있는 비율
- 𝑵: 실행 중인 코어

예를 들어
- S=0.25, N=2, speedup=1.6
- S=0.25, N=4, speedup=2.28

# Chapter 4. Thread & Concurrency (Part 2)

## 스레드 타입
- 유저 스레드: 커널 위에서 지원되고 커널 지원 없이도 관리된다.
- 커널 스레드: 운영체제에서 직접 지원하고 관리한다.
- 유저와 커널 스레드는 다양한 관계 모델이 있을 수 있다.
  - Many to One model
  - One to One model
  - Many to Many model

## PThread
실습 진행
