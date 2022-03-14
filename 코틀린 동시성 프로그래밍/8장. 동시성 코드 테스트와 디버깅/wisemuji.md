# 동시성 코드 테스트와 디버깅

## 함수기능형 테스트

기능 테스트는 작은 단위의 코드를 테스트하는 것이 아니라 기능의 전반적인 동작을 테스트한다는 점에서 유닛 테스트와는 다르다. 

기능 테스트는 애플리케이션이 비동기적으로 작업을 수행하도록 하는 것과 함께 오는 복잡성을 묘사하는 테스트를 만들 수 있게 해주기 때문에 동시성 코드에 필요하다.

## 테스트 작성을 위한 조언

* 버그 수정에는 시나리오를 다루는 테스트가 수반되어야 한다. 
* 동시성 버그가 응용 프로그램의 다른 부분에 어떤 영향을 미칠 수 있는지 항상 고려해야 한다. 
* 목표는 모든 시나리오를 다루는 것이 아니라 가치를 추가하는 동시에 가정에 도전하는 시나리오를 찾는 것이다.
* 엣지 케이스를 찾으려면 탐지 범위 보고서의 분기 분석을 사용해보자. 이것은 동시 코드를 테스트할 때 항상 유용하지는 않지만 시도해 볼 가치가 있다.
* 인터페이스를 사용하여 의존성을 연결한다.

## 테스트 작성해보기

```kotlin
// Mock a datasource that retrieves the data in a different order
   class MockSlowDbDataSource: DataSource {
       // Mock getting the name from the database
       override fun getNameAsync(id: Int) = async {
           delay(1000)
           "Susan Calvin"
       }
       // Mock getting the age from the cache
       override fun getAgeAsync(id: Int) = async {
           delay(500)
           Calendar.getInstance().get(Calendar.YEAR) - 1982
       }
       // Mock getting the profession from an external system
       override fun getProfessionAsync(id: Int) = async {
           delay(200)
           "Robopsychologist"
       }
}
```

```kotlin
   @Test
   fun testOppositeOrder() = runBlocking {
       val manager = UserManager(MockSlowDbDataSource())
       val user = manager.getUser(10)
       assertTrue { user.name == "Susan Calvin" }
       assertTrue { user.age == Calendar.getInstance().get(Calendar.YEAR) - 1982 }
       assertTrue { user.profession == "Robopsychologist" }
}
```

위와 같이 작성하면 크래시가 나게 된다. 

```kotlin
   suspend fun getUser(id: Int): User {
       val name = datasource.getNameAsync(id)
       val age = datasource.getAgeAsync(id)
       val profession = datasource.getProfessionAsync(id)
       // Wait for each of them, don't assume they are ready
       return User(
  ) }
```

# 디버거 Watch 추가
디버그 플래그가 스레드 이름 값에 영향을 미치므로 IDE에서 워치를 추가할 수 있다. 

일단 실행이 코루틴 내부의 중단점에서 중지되고, 스레드의 이름을 감시할 수 있다면, 어떤 코루틴이 현재 실행 중인지 알 수 있을 것이다.

## 조건부 중단점
마찬가지로 Thread.currentThread().name 의 값을 지정된 코루틴에만 영향을 미치는 중단점을 구성하기 위해 사용할 수 있다. 

이것은 두 가지 시나리오에서 특히 유용할 것이다: 
* 루프에서 생성된 코루틴 그룹이 있지만 하나에만 관심이 있거나, 
* 응용 프로그램의 모든 부분에서 호출할 수 있지만 특정 코루틴에 관심이 있는 코드의 일부에서 중단점을 설정하고자 할 때.

## 복원력 및 안정성

복원력은 프로젝트를 시작할 때 고려해야 할 사항이다. 

이전 장에서 보았듯이, 당신은 코루틴에 대한 예외 핸들러를 쉽게 설정할 수 있고, Delayed나 Job이 예외로 끝났는지 검증하는 데 많은 작업이 필요하지 않지만, 만약 당신이 기능이 완료된 후에 예외 핸들링을 추가하려고 한다면, 당신은 당신의 코드를 심하게 수정해야 할 것이고, 어쩌면 전체를 다시 작성해야 할 수도 있다. 

복원력은 원래부터 설계되어야 하기 때문에 코드를 미리 계획하고 예상 동작에 맞게 구성해야 한다.

