# 1장. 출생

- 객체는 자신의 가시성 범위(scope of visivility)안에서 살아간다.
    - 아래 코드가 있다면 extra 객체의 가시성 범위는 if 블록 내부이다.
        
        ```java
        if (price < 100) {
           Cash extra = new Cash(5);
           price.add(extra);
        }
        ```
        
- 객체가 거주할 현재와 미래의 환경을 정의해야 한다.
    - 위 예제에서 숫자 5는 객체 내부에, price는 객체 외부에 존재한다.
    - 자신이 extra라면 price는 둘러싸고 있는 주변 세계이다.

# 1.1 -er로 끝나는 이름을 사용하지 마세요

### 클래스와 객체의 차이

- 가장 큰 차이점은 팩토리(factory)
- 클래스는 객체를 인스턴스화한다고 표현한다.
    - new 연산자를 사용해여 객체(object)라고 불리는 클래스의 인스턴스를 생성할 수 있다.

→ 필요할 때 객체를 꺼낼 수 있고, 필요하지 않는 객체를 반환할 수 있는 객체의 웨어하우스(warehouse)로 클래스를 바라보는 것이 좋다.

클래스는 객체의 템플릿이 아닌, **객체의 팩토리**이다. (수동적이 아닌, 능동적인 관리자)

### 클래스의 이름을 짓는 적절한 방법

- 잘못된 방법
    - 클래스의 객체들이 무엇을 하고 있는지를 살펴본 후, 기능에 기반해서 이름을 짓는 방법
        
        ```java
        class CashFormatter {
        	private int dollars;
          
        	public String format() {
        		return String.format("$ %d", this.dollars);
        	}
        
          // 생성자 생략
        }
        ```
        
        - 위 예제코드의 객체들이 하는 일 → 저장된 금액을 문자로 포맷팅하는 일
        
        → 잘못된 방법. **클래스의 이름은 ‘무엇을 하는지’가 아닌, ‘무엇인지'에 기반해야 함.**
        
- 올바른 방법
    - 무엇인지에 기반해서 이름을 짓는 방법
        
        ```java
        class Cash {
        	private int dollars;
          
        	public String usd() {
        		return String.format("$ %d", this.dollars);
        	}
        
          // 생성자 생략
        }
        ```
        

> 객체는 속성이 아닌, 할 수 있는 일에 따라 설명해야한다.
> 

### -er을 쓰지말자

‘-er’ 접미사를 가진 객체는 모두 잘못 지어진 이름이다. 단, 오랜시간이 흐르며 의미가 정착된 단어는 제외 (user, computer 같은). ‘-er’로 끝난다면, 실제로 객체가 아니라 어떤 데이터를 다루는 절차들의 집합일 뿐이다.

- 객체는 객체의 외부 세계와 내부 세계를 이어주는 연결장치가 아니다.
- 객체는 내부에 캡슐화된 데이터를 다루기 위해 요청할 수 있는 절차의 집합이 아니다.
- 객체는 **캡슐화된 데이터를 대표자**이다.
    - 그렇기 때문에 스스로 결정을 내리고 행동할 수 있다.

### 올바른 클래스 이름을 짓는 방법

- 무엇을 캡슐화할 것인지를 관찰하고, 요소들에 붙일 적합한 이름을 찾는다.
    - ex. 임의의 숫자 리스트가 존재할 때, 리스트의 요소 중에서 소수를 찾는 알고리즘은…
        - 클래스 이름으로는 PrimeNumbers라는 이름이 적합하다.
        - Primer, PrimeFinder, PrimeChooser … 은 적합하지 않다.

# 1.2 생성자 하나를 주 생성자로 만드세요

### 생성자는 많이, 메서드는 적게

> 클래스는 많은 수의 생성자와 적은 수의 메서드를 포함하는 것이 좋다.
(2-3개의 메서드와 5-10개의 생성자를 포함하는 것이 적당하다.)
> 

- 생성자가 많아질 수록 클래스를 더 유연하게 사용할 수 있다.
- 메서드가 많아질 수록 클래스를 사용하기 어려워진다.
    - 초점이 흐려지고, SRP를 위반하게 된다.

### 생성자의 역할

- 제공된 인자를 사용해 캡슐화하고 있는 프로퍼티를 초기화하는 일
    - 이런 초기화 로직은 단 하나의 생성자에만 위치시기고 주 생성자라고 명명한다.
    - 부 생성자는 주 생성자를 **호출**한다.
    
    ```java
    class Cash {
       private int dollars;
     
       // 부 생성자들
       Cash(float dlr) {
         this((int) dlr);
       }
    
       Cash(String dlr) {
         this(Cash.parse(dlr));
       }
    
       // 주 생성자
       Cash(int dlr) {
         this.dollars = dlr;
       }
    }
    ```
    

# 1.3 생성자에 코드를 넣지 마세요

주 생성자는 객체 초기화 프로세스를 시작하는 유일한 장소이기 때문에 제공되는 인자들은 완전해야 합니다.

인자에 손대지 말아야한다.

```java
class Cash {
   private int dollars;

   Cash(String dlr) [
     this.dollars = Integer.parseInt(dlr);
   }
}
```

→ 객체 초기화 코드에는 코드가 없어야하고, 인자를 건드려서는 안됩니다. 

```java
class Cash {
   private Number dollars;

   // 부 생성자
   Cash(String dlr) [
     this(new StringAsInteger(dlr));
   }

   // 주 생성자
   Cash(Number dlr) [
     this.dollars = dlr;
   }
}

class StringAsInteger implements Number {
   private String source;

   StringAsInteger(Stirng src) {
     this.source = src;
   }

	 int intValue() {
      return Integer.parseInt(this.source);
   }
}
```

→ 실제로 사용하는 시점까지 객체의 변환 작업을 연기한다.

### 두 예제 코드의 차이점

- 첫 번째 예제는 숫자 5를 캡슐화한다.
    - 필요한 메서드를 제공하지 않는다.
- 두 번째 예제의 객체 five는 Number**처럼 보이는** StringAsInteger 인스턴스를 캡슐화한다.
    - 나중에 요청이 있을때 파싱하기 때문에 파싱 시점을 클래스의 사용자들이 자유롭게 결정할 수 있다.
    - 만약 파싱이 여러번 수행되지 않도록 하고 싶다면 데코레이터 패턴을 사용할 수도 있다.

> 진정한 객체지향에서의 인스턴스화란 더 작은 객체들을 조합해서 더 큰 객체를 만드는 것이다.
조합해야 하는 단 하나의 이유는 새로운 계약을 준수하는 새로운 엔티티가 필요하기 때문이다.
>