# 15장 Junit 들여다보기

# TOC

- [Contents](#contents)
  * [Junit 프레임워크](#junit-프레임워크)
  * [예제 코드 리팩토링하기](#예제-코드-리팩토링하기)
  * [결론](#결론)
- [Reference](#reference)

<br>

# Contents

## Junit 프레임워크

Java의 테스트 프레임워크

<br>

## 예제 코드 리팩토링하기

- 접두어 제거

    ```java
    // before 
    private int fContextLength;

    // after
    private int contextLength;
    ```

- 조건문 캡슐화

    ```java
    // before
    if (expected == null || actual == null || areStringsEqual()) {
    	
    }

    // after
    if (shouldNotCompact()) {

    }

    private boolean shouldNotCompact() {
    	return expected == null || actual == null || areStringsEqual();
    }
    ```

- 변수, 함수 이름 명확하게 변경
- 부정문을 긍정문으로 변경

    ```java
    // before
    if (shouldNotCompact()) {

    }

    private boolean shouldNotCompact() {
    	return expected == null || actual == null || areStringsEqual();
    }

    // after
    if (canBeCompacted()) {

    }

    private boolean canBeCompacted() {
    	return expected != null && actual != null && areStringsEqual();
    }
    ```

- 함수 분리
- 함수 사용 방식 일관적이게 맞추기

    ```java
    // before
    if (shouldNotCompact()) {
    	findCommonPrefix();
    	compactExpected = compactString(expected);
    }

    // after
    if (shouldNotCompact()) {
    	prefixIndex = findCommonPrefix();
    	compactExpected = compactString(expected);
    }
    ```

<br>

## 결론

세상에 개선이 불필요한 모듈은 없다.

코드를 처음보다 조금 더 깨끗하게 만드는 책임은 우리 모두에게 있다.

<br>

# Reference

[Clean Code](https://book.naver.com/bookdb/book_detail.nhn?bid=7390287)