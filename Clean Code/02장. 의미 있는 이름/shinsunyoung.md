# 2장 의미 있는 이름

## 이름을 잘 짓는 방법

1. **의도를 분명히 밝히기**
    - 변수, 클래스, 함수 이름을 지을 때 답할 수 있어야한다.
        - 존재 이유가 어떻게 되는가?
        - 수행하는 기능은 어떻게 되는가?
        - 사용 방법은 어떻게 되는가?
    - 주석이 필요하다면 의도를 분명히 밝히지 못한 이름이다.

    ```java
    // bad
    for (int[] x : theList) {
    	if (x[0] == 4) {
    		list1.add(x);
    	}
    }
    return list1;

    // good
    for (Cell cell : gameBoard) {
    	if (cell.isFlagged()) {
    		flaggedCells.add(cell);
    	}
    }
    retrun flaggedCells;
    ```

2. **그릇된 정보를 피하기**
    - 널리 쓰이는 의미가 있는 단어를 다른 의미로 사용하면 안된다.

        ```java
        // 이미 sec은 second로 널리 알려진 약어
        int sec = 0; // Security
        ```

    - 서로 흡사한 이름을 사용하면 안된다.

        ```java
        public class XYZControllerForExcelDownload {
        	// something
        }

        public class XYZControllerForExcelStorage {
        	// something
        }
        ```

3. **의미 있게 구분하기**
    - 이름이 달라야한다면 의도도 달라야한다.

        ```java
        for (int i = 0; i < a1.length; i++) {
        	a2[i] = a1[i];
        }
        ```

    - 불용어를 사용하는 것을 피해야한다.

        ```java
        Product product = new Product(); // 뭐가
        ProductInfo productInfo = new ProductInfo(); // 다른
        ProductData productData = new ProductData(); // 거지?!
        ```

4. **발음하기 쉬운 이름을 사용하기**

    ```java
    Date genymdhms; // generate date, year, month ...
    ```

5. **검색하기 쉬운 단어를 사용하기**
    - 긴 단어 > 짧은 단어

        ```java
        // bad
        int sum;

        // good
        int sumOfRefrigeratorFoods
        ```

    - 검색하기 쉬운 이름 > 상수

        ```java
        // bad
        if (i == 7) {
        	// something
        }

        // good
        if (i == MAX_ROW_OF_BOARD) {
        	// something
        }
        ```

    - **이름 길이는 범위 크기에 비례해야한다.**
6. **인코딩 피하기**
    - 헝가리식 표기법

        ```java
        String phoneStr;
        ```

    - 멤버변수 접두어

        ```java
        String m_phone;
        ```

    - 인터페이스 클래스와 구현 클래스
        - 인터페이스에 I 접두사 붙이는것은 좋지 않다.
        - 차라리 구현 클래스에 Impl을 붙이는게 낫다.

        ```java
        public interface IBoardRepository {
        	// something
        }
        ```

7. **기억력 자랑하지 않기**
    - 반복문 이외에는 한 글자를 변수명으로 사용하지 않는게 좋다.

        ```java
        int a = 0;
        ```

8. **클래스 이름**
    - 명사 이름이 좋다.
    - 동사나 불용어는 피하는 것이 좋다.
9. **메소드 이름**
    - 동사나 동사구가 좋다.
    - 표준에 따라 `get`, `set`, `is`를 사용한다.
    - 생성자를 중복 정의할 때에는 정적 팩토리 메소드를 사용하는 것이 좋다.

        ```java
        // bad
        Cat cat = new Cat(1);

        // good
        Cat cat = Cat.fromAge(1);
        ```

10. **기발한 이름 피하기**

    ```java
    // bad
    eatMyShort();

    //good
    abort();
    ```

11. **한 개념에 한 단어 사용하기**
    - 혼용될 수 있는 개념은 한 단어로 정의하는 것이 좋다.

        ```java
        // bad
        public class DeviceControllr {
        	// something
        }

        public class ProtocolManager {
        	// something
        }

        // good
        public class DeviceControllr { // 또는 둘 다 Manger로 맞추기
        	// something
        }

        public class ProtocolController {
        	// something
        }
        ```

12. **말장난 하기 않기**
    - 한 단어를 두 가지 목적으로 사용하지 않는 것이 좋다.

        ```java
        addAge(1); // 나이를 더하는 메소드
        addLocation(); // '위치'라는 열을 추가하는 메소드
        ```

13. **해법 영역에서 가져온 이름 사용하기**
    - 프로그래머에게 익숙한 단어들은 사용해도 된다.
        - ex. JobQueue, Sort ...
14. **문제 영역에서 가져온 이름 사용하기**
    
    - 적절한 프로그램 용어가 없다면 문제 영역에서 이름을 가져와도 된다.
15. **의미 있는 맥락 추가하기**
    
    - 마지막 수단으로는 접두어를 붙여도 된다.
16. **불필요한 맥락 없애기**

```java
// bad
class AccountAddress {

}

// good
class Address {

}
```

