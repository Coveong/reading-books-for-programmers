# 3장 함수

1. **작게 만들기**
    - 함수 안의 줄 수는 적을수록 좋다.
    - 들여쓰기 레벨도 최대 2단까지만 있는 것이 가장 바람직하다.
2. **한 가지만 하기**
    - 한 가지 일을 하는 함수들 + 그 함수들을 호출하는 함수 하나로 이루어져 있어야 한다.
    - 한 함수는 한 가지 일을 잘 하는 게 중요하다.
        - 다른 표현이 아니라 의미 있는 이름으로 함수를 추출할 수 있는 여부로 한 가지의 일만 하고 있는지 판단이 가능하다.
3. **함수 당 추상화 수준은 동일하게 하기**
    - 코드는 위에서 아래로 읽혀야 한다.
4. **서술적인 이름 사용하기**
    - 길고 서술적인 이름이 짧고 어려운 이름보다 낫다.
    - 길고 서술적인 이름이 길고 서술적인 주석보다 낫다.

    ```java
    testableHtml() -> setUpTeardownIncluderRender()
    ```

5. **함수 인수 최대한 적게 쓰기**
    - 가장 좋은 경우는 인수가 없는 것, 다음에는 단항 함수(인수가 1개)이다.
    - 단항 함수를 쓰는 경우
        - 인수에 질문을 던지는 경우

            ```java
            boolean fileExists("fileName")
            ```

        - 이벤트 (입력 인수만 있고 출력 인수가 없다)

            ```java
            passwordAttemptFaildNtimes(int attempts)
            ```

    - 입력값이 있는 변환 함수는 반환 값이 있는 것이 좋다.

        ```java
        // bad
        void transform(StringBuffer in)

        // good
        StringBuffer transform(StringBuffer in)
        ```

    - 플래그 인수 사용 금지

        ```java
        transform(boolean isTrue)
        ```

    - 이항 함수(인수 2개)는 단항 함수보다 이해하기 어렵다. 단, 예외는 있다.

        ```java
        new Point(0, 0); // 예외의 경우. 하지만 결국 두 인수는 하나의 값을 표현하는 요소들 
        ```

    - 삼항 함수(인수 3개)는 이항 함수보다 이해하기 어렵다.
    - 인수가 많이 필요하다면 독자적인 클래스 생성을 고려해보는 것도 좋은 방법이다.
    - 좋은 함수 이름을 짓는 것도 중요하다.

        ```java
        writeField(name); // 함수 -> 동사, 인수 -> 명사
        assertExpectedEqualsActual(expected, actual)
        ```

6. **부수 효과 일으키지 말기**

    ```java
    public boolean checkPassword(String userName, String password) {
    	// 비밀번호를 확인하는 로직
    	Session.initialize(); // 인증과 동시에 세션을 초기화 할 위험이 있다.
    }
    ```

    - 출력 인수보다는 객체 상태를 변경하는 방식이 좋다.

        ```java
        // bad
        appendFooter(s);

        // good
        report.appendFooter(s);
        ```

7. **명령과 조회 분리하기**
    - 함수는 무언가를 수행하거나 답하거나 둘 중 하나만 해야 한다.
8. **오류 코드보다는 예외 사용하기**

    ```java
    if (deletePage(page) == E_OK) // 보다는 예외 처리
    ```

9. **try-catch 블록은 별도 함수로 뽑아내기**

    ```java
    public void delete(Page page) {
    	try {
    		deletePageAndAllReferences(page);	
    	} catch (Exception e) {
    		logError(e);
    	}
    }

    private void deletePAgeAndAllReferences(Page page) throws Exception {
    		// something
    }

    private void logError(Exceptionm e) {
    	logger.log(e.getMessage());
    }
    ```

    - 오류를 처리하는 함수는 오류만 처리해야 한다.

10. **반복하지 말기**
    - 중복은 최대한 없애는 것이 좋다.
11. **구조적 프로그래밍하기**
    - 함수는 return 문이 하나여야 한다.
    - 루프 안에서 break나 continue, goto는 사용하면 안 된다.