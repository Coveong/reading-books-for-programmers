# 4장 주석

주석은 가능한 적은게 좋다. (주석이 필요없는 코드를 만들어야한다.)

1. 주석은 나쁜 코드를 보완하지 못한다.
    - 표현력이 풍부하고 깔끔한 코드 > 복잡하고 어수선하며 주석이 많이 달린 코드

        ```java
        // bad
        // 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
        if ((employee.flags && HOURLY_FLAG) && (employee.age > 65))

        // good
        if (employee.isEligibleForFullBenefits())
        ```

2. 좋은 주석
    - 법적인 주석

        ```java
        // Copyright (c) 2020 by Yellow Mark
        ```

    - 정보를 제공하는 주석

        ```java
        // kk:mm:ss EEE, MMM dd, yyyy 형식이다.
        Patter timeMatcher = Pattern.compile("\\d*:\\d* ...);
        ```

    - 의도를 설명하는 주석

        ```java
        // 스레드를 대량 생성을 위한 코드
        for (int i = 0; i < 25000; i++) {
         // something
        }
        ```

    - 의미를 명료하게 밝히는 주석

        ```java
        assertTrue(a.compareTo(a) == 0); // a == a
        ```

    - 결과를 경고하는 주석

        ```java
        // 여유시간이 충분핟 않다면 실행 금지
        writeLinesToFile(10000000);
        ```

    - TODO 주석

        ```java
        something; // TODO: 현재 필요하지 않다.
        ```

    - 중요성을 강조하는 주석

        ```java
        // trim은 시작 공백을 제거하기 때문에 굉장히 중요
        // 시작 공백이 있다면 다른 문자열로 인식
        String listItemContent = match.group(3).trim();
        ```

    - 공개 API에서 Javadocs

        ```java
        /**
         * @author ss
         */
        ```

3. 나쁜 주석
    - 주절거리는 주석

        ```java
        catch(Exception e) {
        	// 속성 파일이 없다면 기본값을 모두 메모리로 읽어들였다.
        }
        ```

    - 같은 이야기를 중복하는 주석

        ```java
        // 0부터 9까지 반복문을 돌린다.
        for (int i = 0; i < 10; i++) {

        }
        ```

    - 이력을 기록하는 주석

        ```java
        // 2020-01-23 클래스를 정리하고 패키지를 com.sun.aaa로 옮겼다.
        // 2020-02-10 hello() 멤버변수 sun을 삭제했다.
        // 2020-03-04 bye() 메소드를 추가했다.
        ```

    - 닫는 괄호에 추가한 주석

        ```java
        while (true) {

        } // end while
        ```

    - 저자를 표시하는 주석

        ```java
        // @author sun.y
        public void hello() {

        }
        ```

    - 주석으로 처리한 코드

        ```java
        while (true) {
        	// hello();
        	bye();
        }
        ```

    - 비공개 코드에서의 Javadocs