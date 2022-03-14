# 코틀린의 동시성 내부

## 연속체 전달 스타일 (CPS)

계산을 일시 중단하는 실제 구현은 CPS를 사용하여 수행된다. 이 패러다임은 호출되는 함수에 Continuation(콜백)을 보내는 것을 전제로 하여 완성되면 함수가 Continuation을 호출한다. 

일시 중단된 계산이 다른 계산을 호출할 때마다 완료 또는 오류 발생 시 호출되어야 하는 Continuation을 전달합니다.

모든 무거운 리프팅은 컴파일러에 의해 수행되는데, 컴파일러는 모든 보류 계산을 변환하여 그들이 말한 연속을 보내고 받을 수 있도록 한다. 
또, 일시 중단된 계산은 상태를 저장하고 복원할 수 있는 상태 기계로 변환되어 한 번에 코드의 일부를 실행할 수 있으므로, 재개될 때마다 상태를 복원하고 중단되었던 위치에서 실행을 계속한다.

CPS를 상태 머신과 결합함으로써 컴파일러는 다른 계산이 완료되기를 기다리는 동안 일시 중단할 수 있는 계산을 생성한다. 

## Continuation

```kotlin
   public interface Continuation<in T> {
       public val context: CoroutineContext
       public fun resume(value: T)
       public fun resumeWithException(exception: Throwable)
   }
```
구성:

* 본 Continuation과 함께 사용될 CoroutineContext.
* T 값을 매개 변수로 사용하는 resume() 함수. (이 값은 일시 중단의 원인이 된 연산의 결과이므로, 이 함수가 Int를 반환하는 함수를 호출하기 위해 일시 중단되었다면 이 값은 정수가 된다.)
* 예외를 전파할 수 있는 resumeWithException() 함수.

따라서 Continuation은 호출해야 하는 컨텍스트에 대한 정보도 포함하는 확장된 콜백이라고 할 수 있다. 

이 장의 뒷부분에서 알게 되겠지만, 각 Continuation을 특정 스레드 또는 스레드 풀(예외 핸들러와 같은 다른 구성)에서 실행할 수 있게 해주기 때문에 설계의 중요한 부분이다.

## suspend 수식어

suspend 수식어는 컴파일러에게 주어진 범위(함수 또는 람다)의 코드가 계속을 사용하여 동작함을 나타낸다.

그래서 중지된 연산이 컴파일될 때마다, 그것의 바이트코드는 큰 Continuation이 될 것이다. 예를 들어, 다음과 같은 일시 중단 함수를 생각해 보자.
일시 중단 getUserSummary(id: Int): 사용자요약 {log.log("$id 요약 가져오기")

```kotlin
suspend fun getUserSummary(id: Int): UserSummary { 
   logger.log("fetching summary of $id")
   val profile = fetchProfile(id) // suspending fun 
   val age = calculateAge(profile.dateOfBirth)
   val terms = validateTerms(profile.country, age) // suspending fun
   return UserSummary(profile, age, terms)
}
```

우리는 컴파일러에게 getUserSummary()의 실행은 Continuation을 통해 이루어질 것이라는 것을 말하고 있다. 
그래서 컴파일러는 Continuation을 사용하여 getUserSummary()의 실행을 제어한다.

IntelliJ IDEA와 Android Studio를 통해 기능의 일시 중단 지점을 확인할 수 있다.

또 우리의 함수 실행이 3단계로 이루어진다는 것을 의미한다. 
1. 먼저 함수가 시작되고 로그가 인쇄된 다음 fetchProfile()의 호출로 인해 실행이 일시 중단된다.
2. fetchProfile()이 종료되면 함수는 사용자의 사용 기간을 계산한 다음 유효성 확인을 위해 실행을 다시 일시 중단한다.
3. 마지막 단계는 용어가 검증되고 함수가 마지막으로 다시 시작되며 이전 단계의 모든 데이터를 사용하여 사용자의 summary를 만든다.

## 상태 머신

컴파일러가 코드를 분석하면 그것은 일시 중단 함수를 상태 머신으로 변환한다. 이 아이디어는 일시 중단 함수가 현재 상태에 기반하여 재개될 때마다 코드의 다른 부분을 실행함으로써 연속하게 동작할 수 있다는 것이다.



