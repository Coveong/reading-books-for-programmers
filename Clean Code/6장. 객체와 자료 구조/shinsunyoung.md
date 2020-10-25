# 6장 객체와 자료구조

- 객체 지향에서 어려운 변경 코드는 절차 지향 코드에서 쉽고, 절차 지향에서 어려운 변경 코드는 객체 지향에서 쉽다.

    ```java
    // 이것보다
    public class Square implements Shape {
    	public double area() {
    		//
    	}
    }

    public class Circle implements Shape {
    	public double area() {
    		//
    	}
    }

    // 이게 더 나을수도 있다
    public class Geometry {
    	public double area(Object shape) {
    		if (shape instanceof Square) {
    			//
    		} else if (shape instanceof Circle) {
    			//
    		}
    	}

    }
    ```

- 디미터 법칙을 지키는 게 좋다.
    - 모듈은 자신이 조작하는 객체의 속사정을 몰라야한다.
    - 기차 충돌은 피하는게 좋다.

        ```java
        String outDir = ctxt.getOption().getScratch().getPath();
        ```

- 새로운 자료 타입을 추가하는 유연성이 필요하면 객체가 더 적합하다.
- 새로운 동작을 추가하는 유연성이 필요하면 자료 구조, 절차적인 코드가 더 적합하다.