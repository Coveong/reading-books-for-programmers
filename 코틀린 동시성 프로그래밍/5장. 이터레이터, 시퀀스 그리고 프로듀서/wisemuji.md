# 이터레이터, 시퀀스 그리고 프로듀서

## 일시 중단 가능한 시퀀스 & 이터레이터

`yield` 키워드를 사용하면 값이 다시 요청될 때까지 시퀀스 또는 이터레이터가 일시 중단된다.

```kotlin
   val iterator = buildIterator {
       yield("First")
       yield("Second")
       yield("Third")
   }
```

요소가 처음 요청될 때 첫 번째 줄은 "First" 값을 산출하고 그 후에 실행을 일시 중단한다.
다음 요소가 요청되면 두 번째 줄이 실행되어 "Second"를 생성하고 다시 일시 중단된다. 

따라서 이 이터레이터가 포함하는 세 가지 요소를 얻기 위해서는 다음 함수를 세 번 호출하기만 하면 된다.   

```kotlin
   fun main(args: Array<String>) {
       val iterator = buildIterator {
           yield("First")
           yield("Second")
           yield("Third")
       }
       println(iterator.next())
       println(iterator.next())
       println(iterator.next())
   }
```

## 이터레이터의 특징

* 인덱스별로 요소를 검색할 수 없으므로 순서대로만 요소에 액세스할 수 있다.
* 요소는 단일 방향으로만 검색할 수 있으며, 이전 요소를 검색할 수 없다.
* 리셋할 수 없으므로 한 번만 반복할 수 있습니다.

## 이터레이터의 모든 요소 가져오기

`forEach()`, `forEachRemaining()` 키워드를 통해 이터레이터의 모든 요소를 살펴볼 수 있다.

```kotlin
iterator.forEach {
     println(it)
}
```

## 다음 값이 있는지 검증하기

이터레이터의 다음 요소가 있는지 검증하기 위해서는 `hasNext()` 키워드를 사용하면 된다.

```kotlin
   fun main(args: Array<String>) {
       val iterator = buildIterator {
           for (i in 0..4) {
               yield(i * 4)
           }
       }
       for (i in 0..5) {
           if (iterator.hasNext()) {
                println("element $i is ${iterator.next()}")
           } else {
               println("No more elements")
           }
       } 
   }
```

## 시퀀스의 특징

* 인덱스별로 값을 검색할 수 있다.
* 상태가 없고, 상호 작용 후 자동으로 리셋된다. 
* 한 번의 호출로 값 그룹을 가져올 수 있다.

## 시퀀스의 모든 요소 살펴보기

`forEach()`, `forEachIndexed()` 키워드를 통해 이터레이터의 모든 요소를 살펴볼 수 있다.

```kotlin
   sequence.forEach {
       print("$it ")
   } 
   sequence.forEachIndexed { index, value ->
      println("element at $index is $value")
   }
```

# 요약

* 시퀀스의 몇 가지 특징: 상태가 없기 때문에 각 호출 후에 스스로 리셋된다.
* 이터레이터의 몇 가지 특징: 상태를 가지고 있다; 한 방향으로만 읽을 수 있기 때문에 이전 요소를 검색할 수 없다; 그리고 인덱스별로 요소를 검색할 수 없다.
* 시퀀스 및 이터레이터 모두 하나 이상의 값을 산출한 후 일시 중단할 수 있지만 실행의 일부로 일시 중단할 수 없으므로 숫자 시퀀스와 같은 비동기 작업이 필요하지 않은 데이터 소스에 적합하다.
* 시퀀스 및 반복자는 실행 중에 일시 중단할 수 없으므로 일시 중단되지 않는 계산에서 호출할 수 있다.
