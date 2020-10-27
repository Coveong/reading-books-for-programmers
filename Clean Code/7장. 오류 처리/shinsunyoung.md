# 7장 오류 처리

# TOC

- [Contents](#contents)
  * [오류 코드보다 예외를 사용해라](#오류-코드보다-예외를-사용해라)
  * [Try-Catch-Finally 문부터 작성하라](#try-catch-finally-문부터-작성하라)
  * [미확인 예외를 사용하라](#미확인-예외를-사용하라)
  * [예외에 의미를 제공하라](#예외에-의미를-제공하라)
  * [호출자를 고려해 예외 클래스를 정의하라](#호출자를-고려해-예외-클래스를-정의하라)
  * [정상 흐름을 정의하라](#정상-흐름을-정의하라)
  * [null을 반환하지 마라](#null을-반환하지-마라)
  * [null을 전달하지 마라](#null을-전달하지-마라)
- [Reference](#reference)

<br>

# Contents

## 오류 코드보다 예외를 사용해라

```java
// bad
if (result == SUCCESS) {
	if (subResult == SUCCESS) {
		// something
	} else {
		return false;	
	}
} else {
	return false;
}

// good
throw new DeviceShutDownError("Invalid handle for: " + id);
```

<br>

## Try-Catch-Finally 문부터 작성하라

- 호출자가 기대하는 상태를 정의하기 쉬워진다.
- TDD 방식으로 구현하는 방법
    1. 단위 테스트 만들기

        ```java
        @Test(expected = StorageException.class)
         public void retrieveSectionShouldThrowOnInvalidFileName() {
             sectionStore.retrieveSection("invalid file");
         }
        ```

    2. 단위 테스트에 맞춰 코드를 구현하기 (throw)

        ```java
        public List<RecordedGrip> retrieveSection(String sectionName) {
             // 실제로 구현할 때까지 비어 있는 리스트를 반환한다.
             return new ArrayList<RecordedGrip>();
         }
        ```

        → 예외가 발생하지 않기 때문에 TDD가 실패한다.

    3. 파일 접근을 시도하도록 구현하기 (try-catch-finally)

        ```java
        public List<RecordedGrip> retrieveSection(String sectionName) {
             try {
                 FileInputStream stream = new FileInputStream(sectionName);
             } catch (Exception e) {
                 throw new StorageException("retrieval error", e);
             }
             return new ArrayList<RecordedGrip>();
         }
        ```

        → 테스트 성공!

    4. 리팩토링

<br>

## 미확인 예외를 사용하라

- 확인된 예외란?
    - **컴파일 단계**에서 확인되며 반드시 처리해야 하는 예외를 뜻한다.
        - IOException
        - SQLException
        - Exeption을 상속받는 하위클래스 중 Runtime Exception을 제외한 모든 예외
- 미확인 예외란?
    - **실행 단계**에서 확인되며 명시적인 처리를 강제하지 않는 예외를 뜻한다.
        - NullPointerException
        - IllegalArgumentException
        - IndexOutOfBoundException
        - RuntimeException 하위 예외
- 예외를 던지는 메소드가 하위에 있다면 상위 메소드에서 모두 예외를 선언부에 추가해주거나 catch 블록에서 처리해야한다.
    - OCP를 위반하게 된다.

    ```java
    public void getById() throws IOException {
    	get();
    }

    private void get() throws IOException {
    	// something
    	sizeMustBeGreaterThanZero();
    }

    private void sizeMustBeGreaterThanZero() {
    	if (size == 0) {
    		throw new IOException();
    	}
    }
    ```

<br>

## 예외에 의미를 제공하라

- 오류가 발생한 원인과 위치를 찾기 쉽도록 아래 정보를 덧붙인다.
    - 실패한 연산 이름
    - 실패 유형

<br>

## 호출자를 고려해 예외 클래스를 정의하라

- 외부 라이브러리를 사용할 때에는 **wrapper class**를 고려해보는 것이 좋다.
    - 외부 라이브러리 - 프로그램 간의 의존성이 줄어든다.
    - 라이브러리가 바뀌어도 유지보수 비용이 적다.
    - 테스트가 용이해진다.
    - 특정 업체가 API를 설계한 방식에 발목 잡히지 않는다.

```java
// bad
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

// good
LocalPort port = new LocalPort(12); // ACMEPort 클래스가 던지는 예외를 잡아 변환하는 wrapper clas
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

<br>

## 정상 흐름을 정의하라

- 예외적인 상황이 있을 때 그 상황을 캡슐화해서 처리하면 클라이언트가 예외 상황을 처리할 필요가 없어진다.

<br>

## null을 반환하지 마라

- null을 의무적으로 체크해야한다.

    ```java
    public void registerItem(Item item) {
    	if (item != null) {
    		ItemRegistry registry = peristentStore.getItemRegistry();
    		if (registry != null) {
    			Item existing = registry.getItem(item.getID());
    			if (existing.getBillingPeriod().hasRetailOwner()) {
    				existing.register(item);
    			}
    		}
    	}
    }
    ```

- NullPointerException의 발생 가능성이 있다.

→ 차라리 예외를 던지거나 특수 사례 객체를 반환하는 것이 좋다.

<br>

## null을 전달하지 마라

- null을 넘기지 못하도록 금지하는 것이 가장 합리적이다.



<br><br>



# Reference

[Clean Code](https://book.naver.com/bookdb/book_detail.nhn?bid=7390287)

[클린코드 7장 - 오류 처리](http://amazingguni.github.io/blog/2016/05/Clean-Code-7-%EC%98%A4%EB%A5%98-%EC%B2%98%EB%A6%AC)
