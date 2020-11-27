# 16장 SerialDate 리팩터링

# Contents

SerialDate는 날짜를 표현하는 자바 클래스이다.

<br>

## 첫째, 돌려보자

Clover를 이용해 코드 분석을 하니 훨씬 높은 커버리지가 필요했다.

<br>

## 둘째, 고쳐보자

- 변경 이력 주석 삭제

    ```java
    // before
    /**
    * 2020-11-26 metd() 삭제
    * 2020-11-27 metd1() 리턴값 변경
    */
    public class Somthing() {

    // after
    public class Somthing() {
    ```

- Java docs 주석 태그 <pre>로 통일

    ```java

    // before
    /** 
    * <p> 블라블라 </p>
    * <pre>
    * 설명설명
    * </pre>
    */
    public class Somthing() {

    // after
    /** 
    * <pre> 
    * 블라블라
    * 설명설명
    * </pre>
    */
    public class Somthing() {
    ```

- 상수 모음 중 Enum으로 바꾸기

    ```java
    // before
    public static final String JANUARY = 1;
    public static final String FEBRUARY= 2;
    public static final String MARCH= 3;
    ...

    // after
    public static enum Month {
    	JANUARY(1),
    	FEBRUARY(2),
    	MARCH(3),
    	...
    }
    ```

- 불필요한 주석 제거
- 변수명 변경

    ```java
    // before
    /**  1900년 1월 1일에서 시작하는 직렬 번호 */
    private static final int SERIAL_LOWER_BOUND = 2;

    /**  999년 12월 31일에서 시작하는 직렬 번호 */
    private static final int SERIAL_LOWER_BOUND = 2958465;

    // after
    private static final int EARLIEST_DATE_ORDINAL = 2; // 1/1/1900
    private static final int LATEST_DATE_ORDINAL = 2958465; // 12/31/9999

    ```

- 사용하는 곳이 한 클래스밖에 없는 변수는 해당 클래스로 이동
- 메소드 이름 변경
- 사용하지 않는 변수 제거
- 구현하는 곳과 가까운 곳으로 변수 이동
- 알고리즘이 복잡한 코드는 임시변수를 사용하여 읽기 쉽게 변경

    ```java
    // before
    public static SerialDate addMonth(final int month, final SerialDate base) {
    	final int yy = (12 * base.getYYYY() + base.getMonth() + months - 1) / 12;
    	final int mm = (12 * base.getYYYY() + base.getMonth() + months - 1) % 12 + 1;
    	final int dd = Math.min(base.getDateOfMonth(), SerialDate.lastDayOfMonth(mm, yy));
    	
    	return SerialDate.createInstance(dd, mm, yy);

    }

    // after
    public static SerialDate addMonth(final int month) {
    	int thisMonthAsOrdinal = 12 * getYear() + getMonth().index - 1;
    	int resultMonthAsOrdinal = thisMonthAsOrdinal / 12;
    	int resultYear = resultMonthAsOrdinal / 12;
    	Month resultMonth = Month.make(resultMonthAsOrdinal % 12 + 1);

    	int lastDayResultMonth = lastDayOfMonth(resultMonth, resultYear);
    	int resultDay = Math.min(getDayOfMonth(), lastDayOfResultMonth);
    	return DayDateFactory.makeDate(resultDay, resultMonth, resultYear);
    }
    ```

<br>

# Reference

[Clean Code](https://book.naver.com/bookdb/book_detail.nhn?bid=7390287)
