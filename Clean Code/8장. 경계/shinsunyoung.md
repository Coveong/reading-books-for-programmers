# 8장 경계

# TOC

- [Contents](#contents)
  * [외부 코드 사용하기](#외부-코드-사용하기)
  * [경계 살피고 익히기](#경계-살피고-익히기)
  * [학습 테스트는 공짜 이상이다](#학습-테스트는-공짜-이상이다)
  * [아직 존재하지 않는 코드를 사용하기](#아직-존재하지-않는-코드를-사용하기)
  * [깨끗한 경계](#깨끗한-경계)
- [Reference](#reference)



# Contents

## 외부 코드 사용하기

- Map 또는 유사한 경계 인터페이스를 공개 API의 인수로 넘기거나 반환 값으로 사용하지 않도록 주의해야 한다.
- 피치 못할 경우에는 **캡슐화**를 사용하는 것이 좋다.

<br>

## 경계 살피고 익히기

- 간단한 테스트 케이스를 작성해서 외부 코드를 익혀보는 것도 좋은 방법이다. (**학습 테스트**)
- 예시 - log4j 익히기
    1. 패키지를 내려받아 소개 페이지를 연다.
    
2. 문서를 자세히 읽기 전에 첫 번째 **테스트 케이스**를 만든다.
    
        ```java
        // 화면에 "hello"를 출력하는 테스트 케이스이다.
         @Test
         public void testLogCreate() {
             Logger logger = Logger.getLogger("MyLogger");
             logger.info("hello");
         }
    ```
    
    3. 테스트 케이스를 돌려본다. → Appender라는 뭔가가 필요하다는 오류 발생

    4. 문서를 읽어 ConsoleAppender라는 클래스를 생성해야 하는 것을 알고, 생성한 후에 다시 돌린다. → Appender에 출력 스트림이 없다는 사실을 발견
    
        ```java
        @Test
         public void testLogAddAppender() {
             Logger logger = Logger.getLogger("MyLogger");
             ConsoleAppender appender = new ConsoleAppender();
             logger.addAppender(appender);
             logger.info("hello");
     }
        ```

    5. 구글링을 하고 문서를 읽으며 아래와 같이 시도한다. → 성공 & log4j의 동작을 많이 이해
    
        ```java
        @Test
         public void testLogAddAppender() {
             Logger logger = Logger.getLogger("MyLogger");
             logger.removeAllAppenders();
             logger.addAppender(new ConsoleAppender(
                 new PatternLayout("%p %t %m%n"),
                 ConsoleAppender.SYSTEM_OUT));
             logger.info("hello");
     }
        ```

    6. 이해한 지식을 바탕으로 단위 테스트 케이스 몇 개를 작성한다.
    
        ```java
    public class LogTest {
             private Logger logger;
    
             @Before
             public void initialize() {
                 logger = Logger.getLogger("logger");
                 logger.removeAllAppenders();
             Logger.getRootLogger().removeAllAppenders();
             }
    
             @Test
             public void basicLogger() {
                 BasicConfigurator.configure();
             logger.info("basicLogger");
             }
    
             @Test
             public void addAppenderWithStream() {
                 logger.addAppender(new ConsoleAppender(
                     new PatternLayout("%p %t %m%n"),
                     ConsoleAppender.SYSTEM_OUT));
             logger.info("addAppenderWithStream");
             }
    
             @Test
             public void addAppenderWithoutStream() {
                 logger.addAppender(new ConsoleAppender(
                     new PatternLayout("%p %t %m%n")));
                 logger.info("addAppenderWithoutStream");
             }
     }
        ```
    
    7. 모든 지식을 Logger 클래스로 캡슐화한다.

<br>

## 학습 테스트는 공짜 이상이다

- 학습 테스트의 이점
    - 드는 비용보다 필요한 지식을 더 얻을 수 있다.
    - 패키지가 예상대로 도는지 검증할 수 있다.
    - 새로운 버전이 나와도 안전하게 대응이 가능하다.

<br>

## 아직 존재하지 않는 코드를 사용하기

- 모르는 걸 먼저 보지 말고, 거리가 먼 것부터 개발하기

<br>

## 깨끗한 경계

- 변경이 이루어질 때 경계를 통제하면 향후 유지보수 비용이 줄어들 수 있다.
- 경계에 위치하는 코드는 깔끔히 분리해야 한다.
- 기대치를 정의하는 테스트 케이스 작성하면 좋다.
- 외부 패키지를 호출하는 코드를 가능한 줄이는 것이 좋다.
    - 통제가 불가능한 외부 패키지에 의존하는 것보다는 통제가 가능한 **우리 코드에 의존**하는 편이 좋다.



<br>

<br>

# Reference

[Clean Code](https://book.naver.com/bookdb/book_detail.nhn?bid=7390287)

[클린코드 8장 - 경계](http://amazingguni.github.io/blog/2016/05/Clean-code-8-%EA%B2%BD%EA%B3%84)