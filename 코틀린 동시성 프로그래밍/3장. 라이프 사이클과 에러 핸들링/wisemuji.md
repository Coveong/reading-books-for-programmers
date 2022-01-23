# 라이프 사이클과 에러 핸들링

## Jobs and their use cases

## Job

한 번 실행하면 그걸로 끝. 예외가 있을 수 있다.

```kotlin
fun main(args: Array<String>) = runBlocking { val job = launch {
           // background task 실행
       }
}
```

### Exception handling

기본적으로 작업 내부에서 발생하는 예외가 생성된 위치로 전파된다. 작업이 완료되기를 기다리지 않아도 이런 현상이 발생한다.

```kotlin
fun main(args: Array<String>) = runBlocking {
       launch {
           TODO("Not Implemented!")
       }
       delay(500) 
}
```

### Life cycle

* New: 존재하지만 아직 실행되지 않는 작업.
* Active: 실행 중인 작업. 일시 중단된 작업도 활성 상태로 간주된다.
* Completed: 작업이 더 이상 실행되지 않을 때
* Cancelling: 활성 상태인 Job에서 cancel()이 호출되면, 취소가 완료되기까지 시간이 걸릴 수 있다.
* Cancelled: 취소로 인해 실행이 완료된 작업. 취소된 작업은 완료됨으로 간주할 수도 있다.

## Defferred

결과를 가진 비동기 작업으로 만들기 위해 Job을 확장한다.

```kotlin
fun main(args: Array<String>) = runBlocking {
       val headlinesTask = async {
           getHeadlines()
       }
       headlinesTask.await()
}
```

### Exception handling

순수 작업과 달리 지연은 처리되지 않은 예외를 자동으로 전파하지 않는다. 실행이 성공적이었는지 확인하는 것은 사용자의 몫이다. 

```kotlin
fun main(args: Array<String>) = runBlocking {
    val deferred = async {
        TODO("Not implemented yet!")
    }
    // 실패할때까지 기다리가
    delay(2000)
    
    // 혹은 바로 결과 확인하기
    deferred.await()
}
```
