# 스레드 한정, 액터 그리고 뮤텍스

## 원자성의 의미

소프트웨어 실행의 맥락에서, 연산은 단일이고 분리할 수 없을 때 원자적이다. 공유 상태에 대해 이야기할 때, 우리는 종종 많은 스레드에서 단일 변수를 읽거나 쓰는 것에 대해 이야기한다.

* 변수 상태를 수정하는 것은 일반적으로 원자적이지 않기 때문에 문제가 제기되는데, 이는 변수가 업데이트된 값을 읽고, 수정하고, 저장하는 것과 같은 여러 단계로 구성되기 때문이다. 
* 동시 응용 프로그램을 실행하는 동안 공유 상태를 수정하는 코드 블록이 다른 스레드와 중복되는 변경을 통해 이를 수행할 수 있다. 
* 코드 블록을 원자적으로 만들기 위해서는 블록 내에서 발생하는 모든 메모리 액세스가 동시에 실행될 수 없음을 보장해야 한다. 

## 스레드 한정의 의미

스레드 한정은 이름에서 알 수 있듯이 공유 상태에 접근하는 모든 코루틴을 제한하여 단일 스레드에서 실행하는 것을 의미한다. 이것은 상태가 더 이상 스레드 간에 공유되지 않음을 의미한다.

모든 코루틴이 동일한 스레드에서 상태를 수정하는 것이 응용 프로그램 성능에 부정적인 영향을 미치지 않는다는 것을 알고 있는 경우 유용하다.

### 코루틴을 단일 스레드에 고정

```kotlin
var counter = 0
val context = newSingleThreadContext("counter")

fun asyncIncrement(by: Int) = async(context) { 
  for (i in 0 until by) {
    counter++ 
  }
}
```

asyncIncrement()가 호출되는 횟수에 관계없이 단일에서 실행된다.
스레드, 즉 카운터에 대한 모든 변경 사항이 순차적이라는 의미다.

## 액터의 의미

액터는 두 가지 강력한 도구의 조합이다. 상태의 액세스를 단일 스레드로 제한하고 다른 스레드가 채널을 사용하여 상태에 대한 수정을 요청할 수 있다. 

이를 통해 값을 수정할 수 있는 안전한 방법뿐만 아니라 그것을 할 수 있는 강력한 커뮤니케이션 메커니즘을 활용할 수 있다.

### 액터의 생성

여러 스레드로부터 카운터를 안전하게 수정해야 한다고 해보자. 우선 새로운 클래스를 만들어서 이 클래스에서는 카운터의 현재 값을 검색하는 함수를 사용하여 전용 카운터와 전용 단일 스레드 디스패처를 가질 수 있도록 구현한다.

```kotlin
private var counter = 0
private val context = newSingleThreadContext("counterActor")
fun getCounter() = counter
```

이제 카운터의 값을 캡슐화했으므로 수신된 각 메시지의 값을 증가시킬 액터를 추가하기만 하면 된다.

```kotlin
val actorCounter = actor<Void?>(context) { 
  for (msg in channel) {
    counter++ 
  }
}
```

실제로 전송되는 메시지를 사용하지 않기 때문에, 우리는 단순히 액터 유형을 Void?로 설정하여 발신자가 null을 보낼 수 있도록 할 수 있다. 이제 다음 액터를 사용하도록 주요 기능을 업데이트한다.

```kotlin
fun main(args: Array<String>) = runBlocking {
  val workerA = asyncIncrement(2000)
  val workerB = asyncIncrement(100)
  
  workerA.await()
  workerB.await()
  
  print("counter [${getCounter()}]") 
}

fun asyncIncrement(by: Int) = async(CommonPool) { 
  for (i in 0 until by) {
    actorCounter.send(null)
  } 
}
```

### 기능성을 향상시킨 액터

```kotlin
enum class Action {
    INCREASE,
    DECREASE
}

var actorCounter = actor<Action>(context) {
    for (msg in channel) {
        when (msg) {
            Action.INCREASE -> counter++ 
            Action.DECREASE -> counter--
        }
    }
}

fun asyncDecrement(by: Int) = async {
    for (i in 0 until by) {
        actorCounter.send(Action.DECREASE)
    }
}

fun asyncIncrement(by: Int) = async {
    for (i in 0 until by) {
        actorCounter.send(Action.INCREASE)
    }
}
```

### 액터에 버퍼 활용하기

```kotlin
fun main(args: Array<String>) {
  val bufferedPrinter = actor<String>(capacity = 10) {
    for (msg in channel) {
        println(msg)
    } 
  }
  bufferedPrinter.send("hello")
  bufferedPrinter.send("world")
  bufferedPrinter.close()
}

```

## 뮤텍스 이해하기

우리는 코드 블록을 동기화하여 동시에 실행되지 않도록 하여 원자성 위반의 위험을 제거하는 방법을 찾고 있다. 뮤텍스는 한 번에 하나의 코루틴만이 코드 블록을 실행할 수 있음을 보장하는 동기화 메커니즘을 가리킨다.

코틀린 뮤텍스의 가장 중요한 특징은 Non-blocking이라는 것이다: 실행을 기다리는 코루틴은 잠금을 획득하고 코드 블록을 실행할 수 있을 때까지 일시 중단된다. 그럼에도 불구하고, 일시 중단 기능이 없어도 뮤텍스를 잠글 수 있다.

## Volatile 변수

변수의 변경사항이 다른 스레드에 즉시 표시되도록 하려면 다음 예제의 @Volatile 주석을 사용한다.

```kotlin
@Volatile
var shutdownRequested
```
이렇게 하면 값이 변경되는 즉시 다른 스레드에 대한 변경사항을 확인할 수 있다. 즉, 스레드 X가 shutdownRequested 값을 수정하는 경우 스레드 Y가 변경 사항을 즉시 확인할 수 있다.

### Volatile 변수를 사용할 때 주의해야 할 점

Volatile 변수가 무엇을 보장하는지에는 일반적인 오해가 있다. 언제 유용한지 이해하기 위해서는 이전의 예에서 나온 스레드 세이프 카운터를 다시 고려해야 한다. 

두 스레드가 동일한 값을 읽는 데는 두 가지 이유가 있다.
* 다른 스레드를 읽거나 수정하는 동안 스레드의 읽기가 발생하는 경우: 두 스레드는 동일한 데이터로 시작하고 동일한 증가분을 만든다. 둘 다 카운터를 X에서 Y로 변경하므로 하나의 증분이 손실된다.
* 한 스레드의 읽기가 다른 스레드의 수정 후에 발생하지만 스레드의 로컬 캐시가 업데이트되지 않았을 때: 스레드는 로컬 캐시가 제때 업데이트되지 않았기 때문에 다른 스레드가 Y로 설정한 후에도 카운터의 값을 X로 읽을 수 있다. 두 번째 스레드는 카운터의 값을 증가시키지만 오래된 값으로 시작했기 때문에 이미 수행된 변경만 재정의한다.

Volatile은 두 번째 경우 상태를 읽을 때 항상 최신 값을 유지하도록 함으로써 보호 기능을 제공한다. 그러나 두 개의 스레드가 동일한 증가분을 수행할 수 있을 정도로 현재 값을 가까이 읽을 수 있기 때문에 첫 번째 경우로부터 보호를 보장하지 않는다.

# 요약

* 공유 상태가 있으면 동시 코드에서 문제가 될 수 있다. 스레드의 캐시와 메모리 접근의 원자성은 다른 스레드에서 오는 수정사항들을 손실시킬 수 있다. 또한 상태가 일관되지 않게 될 수 있다.
  * 이러한 문제를 피하기 위한 두 가지 방법이 있다: 
    * 오직 하나의 스레드가 상태와 상호 작용하는 것을 보장하는 것 - 그러므로 읽기만을 위해 공유되도록 하고, 코드 원자 블록을 만들기 위해 잠금을 사용하여 코드 블록을 실행하려고 시도하는 모든 스레드의 동기화를 강제한다.
    * 코루틴 컨텍스트를 오직 하나의 스레드의 디스패처와 함께 사용하여 코루틴의 실행을 강제할 수 있다. (스레드 한정)
* 액터는 코루틴과 송신 채널을 결합하는 것이다. 메시지를 기반으로 하는 것보다 강력한 동기화 메커니즘을 구축하기 위해 액터를 단일 스레드로 제한할 수 있다. 원하는 스레드에서 메시지를 전송하여 변경을 요청할 수 있지만, 변경사항은 특정 스레드에서 실행된다.
* 액터는 코루틴의 스레드 제한과 쌍을 이룰 때 특히 좋지만 액터를 사용할 CoroutineContext를 지정할 수 있으므로 액터 스레드 풀에서 실행하도록 할 수 있다.
* 뮤텍스를 사용하면 코루틴이 동기화 작업을 수행할 수 있도록 대기하는 동안 코루틴을 일시 중단할 수 있다.
* JVM은 스레드의 캐시에 넣을 수 없는 Volatile 변수를 제공한다. 스레드 간에 공유되는 변수가 두 가지 특성을 가진 경우 기본 동시성 문제를 해결할 수 있다. 수정될 때 새 값은 이전 값에 의존하지 않으며 휘발성 변수의 상태는 다른 속성에 의존하거나 영향을 미치지 않는다.

