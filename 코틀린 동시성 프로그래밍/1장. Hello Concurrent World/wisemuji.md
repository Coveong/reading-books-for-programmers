# Hello Concurrent World
## 프로세스, 스레드, 코루틴

* 프로세스: 실행 중인 애플리케이션의 인스턴스
* 스레드: 프로세서가 실행할 일련의 명령을 포함함
* 코루틴: 경량 스레드

## 코루틴

코루틴은 프로세서가 실행할 명령 집합의 실행을 정의함.

코루틴은 스레드 내부에서 실행된다. 하나의 스레드는 내부에 많은 코루틴을 가질 수 있다.

```kotlin
suspend fun createCoroutines(amount: Int) {
    val jobs = ArrayList<Job>()
    for (i in 1..amount) {
        jobs += launch {
            println("Started $i in ${Thread.currentThread().name}")
            delay(1000)
            println("Finished $i in ${Thread.currentThread().name}")
        }
    }
    jobs.forEach {
         it.join()
    }
}
```
코루틴은 스레드 내에서 실행되더라도 스레드에 바인딩되지 않는다.

스레드에서 코루틴의 일부를 실행하고 실행을 일시 중단하고 나중에 다른 스레드에서 계속할 수 있다.

## 요약

* 애플리케이션은 하나 이상의 프로세스로 구성되고 각 프로세스에는 하나 이상의 스레드가 있다.
* 스레드를 차단한다는 것은 해당 스레드에서 코드 실행을 중지하는 것을 의미하며, 그런 이유로 사용자와 상호 작용하는 스레드는 차단되지 않을 것으로 예상된다. 
* 코루틴은 기본적으로 스레드에 상주하지만 하나에 묶여 있지 않은 경량 스레드다.

![image](https://user-images.githubusercontent.com/32327475/149874457-1941c29a-1201-4c71-9dd6-761f1e907842.png)

## 동시성은 병렬성이 아니다

* 동시성은 두 개 이상의 알고리즘의 타임라인이 겹치는 경우 발생한다. 
* 병렬 처리는 두 개의 알고리즘이 정확히 같은 시점에 실행될 때 발생한다. 

## CPU 바운드 및 I/O 바운드

* CPU 바운드: 실행의 모든 부분이 CPU에 의존됨
* I/O 바운드: 입출력 장치에 의존하는 알고리즘이므로 실행 시간은 이러한 장치의 속도에 따라 달라짐

## CPU 바운드 알고리즘의 동시성 vs 병렬 처리

CPU 바운드 알고리즘은 병렬 처리의 이점을 얻을 수 있지만 단일 코어의 경우 아닐 수 있음.

CPU 바운드 알고리즘을 위해 합리적인 양의 스레드를 생성하는 것을 고려하는 것이 좋으며 현재 장치의 코어 수를 기반으로 이러한 결정을 내리는 것이 중요함.

## I/O 바운드 알고리즘의 동시성 vs 병렬 처리

I/O 작업은 항상 동시에 실행되는 것이 좋다.

## 동시성이 종종 두려운 이유

* 원자성 위반
* 교착 상태
* Livelock

## 코틀린의 동시성

* Non-blocking
* 명시적임
* 가독성이 좋음
* 유지보수 잘 됨
* 유연함

## 결론

* 응용 프로그램에는 하나 이상의 프로세스가 있다. 그들 각각에는 적어도 하나의 스레드가 있고 코루틴은 스레드 내부에서 실행된다.
* 코루틴은 재개될 때마다 다른 스레드에서 실행할 수 있지만 특정 스레드로 제한될 수도 있다.
* 응용 프로그램이 둘 이상의 중첩 스레드에서 실행될 때 응용 프로그램은 동시적이다.
* 올바른 동시성 코드를 작성하려면 Kotlin에서 코루틴의 통신 및 동기화를 의미하는 서로 다른 스레드를 통신하고 동기화하는 방법을 배워야 한다.
* 병렬 처리는 동시 응용 프로그램을 실행하는 동안 적어도 두 개의 스레드가 동시에 효과적으로 실행될 때 발생한다.
* 동시 코드를 작성하는 데에는 많은 문제가 있으며 대부분은 올바른 통신 및 스레드 동기화와 관련이 있다. Race condition, 원자성 위반, 교착 상태 및 라이브록이 가장 일반적인 문제의 예다.
* Kotlin은 동시성에 대해 현대적이고 신선한 접근 방식을 취했다. Kotlin을 사용하면 비차단적이고 읽기 쉽고 활용도가 높으며 유연한 동시 코드를 작성하는 것이 가능하고 권장됩니다.