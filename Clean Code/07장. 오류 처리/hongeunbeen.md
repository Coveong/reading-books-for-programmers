# 7장 오류 처리

오류 처리는 프로그램에 반드시 필요한 요소 중 하나

→ 프로그램은 뭔가 잘못될 가능성이 늘 존재

오류 처리로 인해 프로그램 논리 이해하기 어려워짐

→ 여기저기 흩어진 오류 처리 코드로 실제 코드가 하는 일 파악하기 불가능

## 오류 코드보다 예외를 사용하라

```jsx
// Bad
public class DeviceController {
  ...
  public void sendShutDown() {
    DeviceHandle handle = getHandle(DEV1);
    // Check the state of the device
    if (handle != DeviceHandle.INVALID) {
      // Save the device status to the record field
      retrieveDeviceRecord(handle);
      // If not suspended, shut down
      if (record.getStatus() != DEVICE_SUSPENDED) {
        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);
      } else {
        logger.log("Device suspended. Unable to shut down");
      }
    } else {
      logger.log("Invalid handle for: " + DEV1.toString());
    }
  }
  ...
}
```

→ 호출자 코드가 복잡해짐

```jsx
// Good
public class DeviceController {
  ...
  public void sendShutDown() {
    try {
      tryToShutDown();
    } catch (DeviceShutDownError e) {
      logger.log(e);
    }
  }
    
  private void tryToShutDown() throws DeviceShutDownError {
    DeviceHandle handle = getHandle(DEV1);
    DeviceRecord record = retrieveDeviceRecord(handle);
    pauseDevice(handle); 
    clearDeviceWorkQueue(handle); 
    closeDevice(handle);
  }
  
  private DeviceHandle getHandle(DeviceID id) {
    ...
    throw new DeviceShutDownError("Invalid handle for: " + id.toString());
    ...
  }
  ...
}
```

→ 오류 발생시 예외를 던지는 편 추천

→ 호출자 코드 깔끔해짐!

- 디바이스를 종료하는 알고리즘과 오류를 처리하는 알고리즘 분리
(논리처리 코드와 오류처리 코드 나뉨)

## Try-Catch-Finally 문부터 작성하라

예외에서 프로그램 안에다 범위를 정의함

- try-catch-finally
    - try블록에 들어가는 코드를 실행하면 어느 시점에서든 실행 중단 후 catch 블록으로 넘어감
    - try 블록은 트랜잭션과 비슷 
    → try 블록에서 무슨 일이 생기든지  catch 블록은 프로그램 상태 일관성있게 유지

→ 예외가 발생할 코드를 짤 때 try-catch-finally문으로 시작!

- 무슨 일이 생기든지 호출자가 기대하는 상태 정의 쉬워짐

try-catch 구조로 범위를 정의하면 TDD 사용해 필요한 나머지 논리 추가

→ 강제로 예외 일으키는 테스트 케이스 작성 후 테스트 통과하는 코드 작성 방법 권장 

## 미확인 예외를 사용하라

확인된 예외 몇가지 장점 제공

→ 지금은 안정적인 소프트웨어 제작 요소로 확인된 예외 필요X 

→ 아주 중요한 라이브버리 작성시 모든 예외 잡아야 함

- 애플리케이션은 의존성 비용 > 이익 → 생각하기

→ 확인된 오류가 치르는 비용에 상응하는 이익 제공하는지 따져보기

확인된 예외는 OCP 위반

→ 하위 단계에서 코드 변경 시 상위 단계 메서드 선언부 수정해야 함

→ 모듈과 관련된 코드 변경X ⇒ 모듈 다시 빌드 후 배포 필요(선언부 수정)

확인된 오류 함수에 던지면

- 함수 모두가 catch 브록에서 새로운 예외 처리
- 선언부에 throw절 추가

→ 확인된 예외가 캡슐화 깨버리는 현상 발생

## 예외에 의미를 제공하라

예외를 던질 때 전후 상황을 충분이 덧붙이기

→ 오류가 발생한 원인과 위치 찾기 쉬워짐

자바는 모든 예외에 호출 스택 제공

→ 호출 스택만으로 부족하다면 오류 메시지에 정보 담아 예외와 함께 던지기

- 로깅 기능 사용한다면 오류 기록 가능한 정보 넘겨줌

## 호출자를 고려해 예외 클래스를 정의하라

오류 분류 방법 굉장히 많음

- 오류가 발생한 위치로 분류
- 오류가 발생한 컴포넌트로 분류
- 유형으로 분류  - 디바이스 실패, 네트워크 실패, 프로그래밍 오류 등

→ 오류 정의 시 가장 중요한 것은 오류를 잡아내는 방법!!

```jsx
// Bad
  // catch문의 내용이 거의 같다.
  
  ACMEPort port = new ACMEPort(12);
  try {
    port.open();
  } catch (DeviceResponseException e) {
    reportPortError(e);
    logger.log("Device response exception", e);
  } catch (ATM1212UnlockedException e) {
    reportPortError(e);
    logger.log("Unlock exception", e);
  } catch (GMXError e) {
    reportPortError(e);
    logger.log("Device response exception");
  } finally {
    ...
  }
```

→ 대다수 상황에서 오류를 처리하는 방식은 오류를 일으킨 원인과 무관하게 비교적 일정함

- 오류를 기록
- 프로그램을 계속 수행해도 좋은지 여부 확인

→ 하지만 !! 예외에 대응하는 방식이 예외 유형과 무관

```java
LocalPort port = new LocalPort(12);

try {
  port.open();
} catch (PortDeviceFailure e) {
  reportError(e);
  logger.log(e.getMessage(), e);
} finally {
  ...
}

public class LocalPort {
  private ACMEPort innerPort;
  public LocalPort(int portNumber) {
    innerPort = new ACMEPort(portNumber);
  }
  
  public void open() {
    try {
      innerPort.open();
    } catch (DeviceResponseException e) {
      throw new PortDeviceFailure(e);
    } catch (ATM1212UnlockedException e) {
      throw new PortDeviceFailure(e);
    } catch (GMXError e) {
      throw new PortDeviceFailure(e);
    }
  }
  ...
}
```

→ 예외를 잡아 변환하는 감싸기(wrapper) 클래스를 잘 활용하면 매우 유용

- 외부 API를 사용할 때는 감싸기 기법이 최선 → 프로그램 사이에서 의존성이 줄어든다.
- 특정 업체가 API를 설계한 방식에 발목 잡히지 않음

## 정상 흐름을 정의하라

→ 비즈니스 논리와 오류 처리가 잘 분리된 코드

외부 API를 감싸 독자적인 예외를 던지고, 코드 위에 처리기를 정의해 중단된 계산을 처리한다.

→ 멋진 처리 방식이지만, 적합 X 가능성 존재

- 특수 사례 패턴 - 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식
→ 특수 사례 패턴을 화룡하면 클라이언트 코드가 예외적인 상황을 처리할 필요 없음

```java
//version1
try {
  MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
  m_total += expenses.getTotal();
} catch(MealExpensesNotFound e) {
  m_total += getMealPerDiem();
}
//version2
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
  m_total += expenses.getTotal();
  ...

//version3 특수 사례 패턴 -> 객체를 조작
public class PerDiemMealExpenses implements MealExpenses {
  public int getTotal() {
    // return the per diem default
  }
}
```

## null을 반환하지 마라

null을 반환하는 코드

- 일거리를 늘릴 뿐만 아니라 호출자에게 문제를 떠넘김
- NullPointerException 발생 가능성 존재

→ 메서드에서 null 반환 대신 예외를 던지거나 특수 사례 객체 반환

→ 외부 API에서 null 반환 대신 감싸기 메서드 구현해 예외 던지거나 특수 사례 객체 반환

→ 많은 경우 특수 사례 객체 손쉬운 해결방법

```java
//Bad
List<Employee> employees = getEmployees();
if (employees != null) {
  for(Employee e : employees) {
    totalPay += e.getPay();
  }
}

// Good
List<Employee> employees = getEmployees();
for(Employee e : employees) {
  totalPay += e.getPay();
}
```

## null을 전달하지 마라

- 메서드에서 null을 반환하는 방식
- 메서드로 null을 전달하는 방식

→ 최악임!!!

null을 전달하는 방식

- NullPointerException 발생 가능성 존재

→ 예외 처리기 생성

→ assert 문 사용

→ 호출자가 실수로 넘기는 null 처리 방법 X로 애초에 null을 넘기지 못하도록 금지하는 정책 합리적

## 결론

- 깨끗한 코드는 읽기 좋아야 함
- 깨끗한 코드는 안정성도 높아야 함

→ 그러나 이 둘을 상충하는 목표X 

오류 처리를 프로그램 논리와 분리해 독자적으로 고려해 튼튼하고 깨끗한 코드 작성하자

→ 독립적인 추론 가능

→ 코드 유지보수성 높아짐