# 일시 중단 함수와 코루틴 컨텍스트

* 일시 중단 함수란?
* 일시 중단 함수 사용 방법
* 일시 중단 함수를 사용하지 않고 비동기 기능을 사용해야 하는 경우
* 코루틴 컨텍스트란?
* 다양한 컨텍스트 유형
* 코루틴의 동작을 정의하기 위한 컨텍스트 결합 및 분리

## 일시 중단 함수

```kotlin
suspend fun greetDelayed(delayMillis: Int) { 
  delay(delayMillis)
  println("Hello, World!")
}
```
일시 중단 함수를 생성하기 위해서는 `suspend` 키워드를 이용하면 된다.

일시 중단 함수에서는 다른 일시 중단 함수를 직접적으로(예를 들어 delay처럼) 바로 호출하는게 가능하다. 

만약 다른 일시 중단 함수를 일시 중단 함수가 아닌 곳에서 호출하려면 다음과 같이 코루틴 빌더를 래핑하여 호출해야 한다.
```kotlin
fun main(args: Array<String>) { 
  runBlocking {
    greetDelayed(1000)
  }
}
```

## 비동기 함수 vs 일시 중단 합수 이용

### 비동기 함수를 사용했을 때

```kotlin
class ProfileServiceClient : ProfileServiceRepository { 
  override fun asyncFetchByName(name: String) = async {
    Profile(1, name, 28)
  }
  override fun asyncFetchById(id: Long) = async { 
    Profile(id, "Susan", 28)
  } 
}
```

* 함수의 이름을 구체적으로 적어야 함. 즉, 함수가 비동기식임을 명시하여 클라이언트가 필요한 경우 완료될 때까지 기다려야 함을 인식하도록 해야 한다.
* 요청이 완료될 때까지 호출자가 항상 일시 중지해야 할 가능성이 높으므로 await() 호출은 일반적으로 함수에 대한 호출 직후에 발생한다.
* Deferred 타입에 종속될 가능성이 큼.


### 일시 중단 함수를 사용했을 떄

```kotlin
class ProfileServiceClient : ProfileServiceRepository { 
  override suspend fun fetchByName(name: String) : Profile {
    return Profile(1, name, 28)
  }
  override suspend fun fetchById(id: Long) : Profile { 
    return Profile(id, "Susan", 28)
  } 
}
```
* 유연함: 세부 구현 사항이 노출되지 않아 현재 스레드를 차단하지 않고 예상된 프로파일을 반환하는 한 유연하게 확장할 수 있다.
* 단순함: 이름을 변경할 필요가 없고 await()를 호출할 필요가 없다.

### 비교 결론
* 일반적으로 구현이 잡에 묶이지 않도록 비동기 기능에 대해 일시 중단 기능을 사용하는 것이 좋다.
* 인터페이스/abstract를 정의할 때는 항상 일시 중단 기능을 사용하자. 비동기 함수를 사용하면 구현이 강제로 Job을 반환하게 된다.

## 코루틴 컨텍스트

### CommonPool

CommonPool은 CPU 바인딩 작업을 위해 프레임워크에 의해 자동으로 생성되는 스레드 풀이다. 최대 크기는 현재 기기의 코어 크기에서 하나를 뺀 값이다. 현재 기본 디스패처로 사용되고 있다.

### Unconfined

이 디스패처는 첫 번째 일시 중단 지점에 도달할 때까지 현재 스레드에서 코루틴을 실행한다. 일시 중단 후, 코루틴은 사용되는 스레드에서 재개된다.

### Single thread context

이 디스패처는 항상 코루틴이 특정 스레드에서 실행되는 것을 보장한다.

### Thread pool

이 디스패처는 스레드 풀을 가지고 있으며 해당 풀에서 사용할 수 있는 모든 스레드에서 코루틴을 시작하고 재개한다. 
