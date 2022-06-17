# 2장. 학습

## 1. 가능하면 적게 캡슐화

- 4개 또는 그 이하의 객체를 캡슐화할 것을 권장
    - 더 많은 객체 캡슐화 필요 = 리팩토링 필요 → 클래스를 더 작은 클래스들로 분해
- 상태 없는 객체는 존재해서는 안되고, 상태는 객체의 식별자여야함
    
    ```java
    class Cash {
    	private Integer digits; //상태
    	private Integer cents; //상태
    	private String currency; //상태
    }
    ```
    
    ⇒ 내부에 캡슐화된 객체 전체 = 객체의 상태 또는 식별자라 칭함
    
- 다른 객체를 캡슐화하지 않는 객체란 존재하지 않으며 존재할 수도 없음
    - Java를 비롯한 대부분의 OOP 언어에서는 객체는 데이터를 저장할 수 있는 껍질과 유사함..
    
    ```java
    Cash x = new Cash(29, 95, "USD");
    Cash y = new Cash(29, 95, "USD");
    assert x.equals(y);
    assert x == y;
    ```
    
    - 두 객체의 상태는 서로 동일하지만 식별자는 다름…
        
        → Java언어의 설계적 결함 존재
        
- Java의 결함을 해결하기 위해선 == 연산자를 사용하지 말고 항상 equals 메서드 오버라이드

## 2. 최소한 뭔가는 캡슐화

- 아무것도 캡슐화하지 않으면 클래스의 모든 객체들은 동일함
    
    ```java
    class Year {
    	int read() { ... }
    }
    ```
    
    → 프로퍼티가 없는 클래스는 OOP에서 정적 메서드와 유사함
    
    - 아무런 상태와 식별자를 가지지 않고 오직 행동만 포함함
- 순수한 OOP
    - 정적 메서드가 존재하지 않음
    - 인스턴스 생성과 실행을 엄격하게 분리함
    
    → 즉, 프로퍼티가 없는 클래스를 만들 수 없음
    
- 오직 생성자에서만 new 연산자를 허용해야 함
    - 어떤 클래스의 인스턴스를 생성한 후 인스턴스를 통해 시스템 클럭을 얻는 방식으로 설계
    
    ```java
    class Year {
    	private Millis millis;
    	Year(Millis mesc){
    		this.millis = mesc;
    	}
    	int read() { ... }
    }
    ```
    
- 객체는 자기 자신을 식별할 수 있도록 다른 객체들을 캡슐화해야함
    - 객체가 어떤 것도 캡슐화하지 않는다 = 객체가 세계 전체다 와 같은 의미

## 3. 항상 인터페이스를 사용

- 객체는 살아있는 유기체로 다른 유기체들과 의사소통하며 다른 객체의 작업을 지원하고, 도움을 받음
    - 매우 사회적이면서 유대감 높음 = 결합 발생
- 객체 사이의 강한 결합도는 유지보수성에 문제 미침
    - 상호작용하는 다른 객체를 수정하지 않고 해당 객체를 수정할 수 있도록 객체 분리 필요

### [인터페이스]

- 인터페이스 : 객체가 다른 객체와 의사소통하기 위해 따라야하는 계약
    - 인터페이스를 구현하는 클래스는 인터페이스 구현 방법에 관심 없음(동작 방식 알 수 X)
    - 클래스간의 느슨한 결합 의미함
- 클래스 안의 모든 퍼블릭 메서드가 인터페이스를 구현하도록 만들어야 함
    
    ```java
    class Cash {
    	public int cents() { ... } //인터페이스 구현하도록 만들어야 함
    }
    ```
    
    - 이 설계는 다른 클래스와의 강하게 결합을 조장함

### [느슨한 결합도]

- 클래스가 존재하는 이유 = 다른 클래스의 서비스를 필요로 하기 때문
    - 서비스 제공자들은 서로 갱쟁하기 때문에 동일한 인터페이스를 구현하는 여러 클래스들 존재함 == 각각의 경쟁자는 서로 다른 경쟁자를 쉽게 대체할 수 있어야 함
- 클래스와 인터페이스 간의 결합은 항상존재하며 제거 불가
    - 결합은 나쁘지 않음 ⇒ 결합을 통해 전체 시스템 안정적인 상태로 유지 가능
    - 즉, 요소들 사이의 계약으로 전체적인 환경을 구조화된 상태로 유지할 수 있도록 함

## 4. 메서드 이름 신중하게 선택

- 빌더의 이름은 명사로, 조정자의 이름은 동사로
- 올바르게 지은 메서드 이름은 사용자들이 객체를 설꼐한 목적, 객체가 수행해야 하는 임무, 객체의 존재 목적과 살아가는 의미를 더 잘 이해할 수 있도록 함
    - 객체 = 자신의 의무를 수행하는 방법을 알고 있고 존중 받기를 원하는 살아있는 유기체

### [빌더(builder)]

- 빌더 : 뭔가를 만들고 새로운 객체를 반환하는 메서드
- 반환타입은 절대 void가 될 수 없으며, 이름은 항상 명사여야 함
    
    ```java
    int pow(int base, int power);
    String parsedCell(int x, int y); 
    ```
    
    - 형용사 덧붙여 메서드 의미 풍부하게 설명 가능

### [조정자(manipulator)]

- 조정자 : 객체로 추상화한 실세계 엔티티를 수정하는 메서드
- 반환타입은 항상 void이고, 이름은 항상 동사
    
    ```java
    void save(String content);
    void quicklyPrint(int id);
    ```
    
    - 부사 덧붙여 메서드 의미 풍부하게 설명 가능

### [빌더와 조정자]

- 빌더와 조정자 사이에는 메서드 존재 X
    - 뭔가를 조작한 후 반환, 뭔가를 만드는 동시에 조작하는 메서드가 존재해서는 안됨
- 메서드 이름을 동사로 지을 떄 객체에게 무엇을 할지를 알려주어야 함
    - 만들라고 요청하는 것은 예의에 어긋남
    - 만드는 방법은 객체 스스로 결정하도록!
    
    ```java
    //InputStream load(URL url);  잘못된 이름
    InputStream stream(URL url); 
    //String read(File file);  잘못된 이름
    String content(File file);
    //int add(int x, int y);  잘못된 이름
    int sum(int x, int y);
    ```
    
    - 객체에게 어떤 일을 해야 하는 지를 직접적으로 이야기하지 않고, 특정한 계약을 준수하는 결과를 요청함
- 핵심적인 원칙만 준수한다면 접두어 사용 가능
    
    ```java
    class Book{
    	Book withAuthor(String author);
    }
    ```
    
- 빌더와 조정자가 함께 있는 메서드는 너무 많은 일을 하는 중임
    - OOP의 전체 목적 : 개념을 고립시켜 복잡성을 낮추는 것
    - 고립시킨 개념이 작을 수록 이해하고 유지보수하기 쉬워짐

### [Boolean 값을 결과로 반환]

- 해당 메서드는 규칙에 있어서 예외적인 경우
    - 빌더에 속하지만, 가독성 측면에서 이름을 형용사로 지어야 함
    - 접두사 is는 중복으로 메서드 이름에는 포함 X, 읽을 때는 자연스럽게 같이 읽기
    
    ```java
    boolean empty();
    boolean readable();
    ```
    
- 예외적인 경우로 처리하는 이유는 대부분의 언어들이 논리 구성자를 특별한 방식으로 다루기 때문
    
    ```java
    if(name.emptiness() == true) { ... } //이런식으로 다루지 않고
    if(name.empty()) { ... } //==true 부분을 생략함
    ```
    
    - 일반적으로 생략하는 부분때문에 메서드의 이름을 형용사로 지어야 자연스럽게 읽힘

### [요약]

- 메서드의 목적이 무엇인지 확인
    - 빌더나 조정자, 둘 중 하나여야 함(동시는 불가)
- 빌더라면 이름 명사, 조정자라면 이름 동사
- Boolean 값 반환하는 빌더는 예외 → 형용사로 이름

## 5. 퍼블릭 상수를 사용하지 마세요

### [상수]

- public static final 프로퍼티로 객체 사이에 데이터를 공유하기 위해 사용하는 메커니즘
- 상수 사용 이유 : 데이터를 공유하기 위해서

→ 하지만, 객체들은 어떤 것도 공유해서는 안됨

- 상수를 이용한 공유 메커니즘은 캡슐화와 객체지향적인 사고 전체를 부정하는 일

### [상수 사용 방법]

1.  지역적으로 상수 정의(private static final) 해  사용한다.
    
    ```java
    class Rows {
    	private static final String EOL = "\r\n";
    }
    
    class Records {
    	private static final String EOL = "\r\n";
    }
    ```
    
    - 문제점 : 코드가 중복됨
2. 전역 범위에서 접근가능하도록 class를 따로 생성한다.
    
    ```java
    public class Constants {
    	public static final String EOL = "\r\n";
    	//어떤 정보도 제공하지 않은 채 전역 가시성 안에 방치되어 있음
    }
    
    class Rows {
    	System.out.println(Constants.EOL);
    	//목적을 명확하게 만들어줄 코드를 추가해 원시적인 정적 상수를 감싸야 함
    }
    
    class Records {
    	System.out.println(Constants.EOL);
    }
    ```
    
    - 지역적으로 상수 정의할 필요 없이 퍼블릭 상수 재사용 가능
    - 문제점 : 결합도가 높아지고, 응집도가 낮아짐
        - 높은 결합도: 다른 객체를 사용하는 상황에서 서로 어떻게 사용하는지 모르겠다면 높은 결합도 의미함
        - 낮은 응집도 : 객체가 자신의 문제를 해결하는데 덜 집중할 때 낮은 응집도 의미함
3. 기능을 공유할 수 있는 class를 생성한다.
    - 객체 사이에 데이터가 아닌 기능을 공유해야 함
    
    ```java
    class EOLString {
    	private final String origin;
    	EOLString(String src){
    		this.origin = src;
    	}
    	@Override
    	String toString() { return String.format("\r\n"); }
    }
    
    class Rows {
    	Rows row = new Rows();
    	System.out.println(new EOLString(row.toString());
    }
    
    class Records {
    	Records rec = new Records();
    	System.out.println(new EOLString(rec.toString());
    }
    ```
    
    - 각 클래스는 EOLString의 정확한 방법은 알지 못하지만, 그 작업을 책임진다는 사실을 암
        - EOLString은 계약에 따라 행동하며 내부에 계약의 의미를 캡슐화함
        - 계약에 의한 결합은 언제라도 분리가 가능함 → 유지보수성 bb

### [퍼블릭 상수마다 클래스]

- 퍼블릭 상수마다 계약의 의미를 캡슐화하는 새로운 클래스를 만들어야 함
    - 클래스 사이에 중복 코드가 없다면 클래스는 작아지고 코드는 깔끔해짐
    - 애플리케이션을 구성하는 클래스의 수가 많을수록 설계가 더 좋아지고 유지보수하기 쉬워짐
    
    ```java
    String body = new HttpRequest().method(HttpMethods.POST).fetch(); // X
    //리터럴 대신 단순 메서드 사용하기
    String body = new PostRequest(new HttpRequest()).fetch(); // O
    ```
    

### [요약]

- OOP에서 퍼블릭 상수를 절대로 사용해서는 안됨
- 열거형 역시 사용해서는 안됨 (열거형과 퍼블릭 상수 사이에는 아무런 차이도 없음)

## 6. 불변 객체로 만드세요

- 모든 클래스를 상태 변경이 불가능한 불변 클래스로 구현
    - 유지보수성 크게 향상 가능
    - 깔끔하고, 더 작고, 더 쉽게 이해할 수 있는 코드 만들기 가능

### [불변 객체]

- 불변 객체 : 인스턴스를 생성한 후에 상태를 변경할 수 없는 객체
    - 필요한 모든 것을 내부에 캡슐화하고 변경할 수 없도록 통제함
    
    ```java
    class Cash {
    	private final int dollars;
    	Cash(int val) { 
    		this.dollars = val; 
    	}
    }
    ```
    
    → final 키워드 : 생성자 외부에서 프로퍼티의 값을 수정할 경우 컴파일 타임 에러 발생해야 한다는 사실 컴파일러에게 알려줌
    
- 불변 객체를 수정하고 싶다면 🤔
    - 프로퍼티를 수정하는 대신 새로운 객체를 생성해야 함
    
    ```java
    class Cash {
    	private final int dollars;
    	public Cash mul(int factor){
    		return new Cash(this.dollars * factor);
    	}
    }
    ```
    
    - 불변 객체는 어떤 방식으로든 자기 자신을 수정할 수 없으며, 항상 원하는 상태를 가지는 새로운 객체를 생성해 반환해야 함

### [가변 객체]

- 가변 객체는 전반적인 객체 패러다임의 오용
    
    ```java
    Cash five = new Cash(5);
    five.mul(10);
    System.out.println(five); // 10이 출력
    ```
    
    - 가변성은 코드를 이해하고 유지보수하기 어렵게 만듬
- 가변 객체는 엄격하게 금지해야 함

### [불변 객체의 지연 로딩]

- 지연 로딩 기법 : 객체가 캡슐화하고 있는 프로퍼티를 업데이트할 필요가 있을 떄 프로퍼티를 게이르게 로딩
    - 기술적으로 불변 객체를 사용해 지연 로딩 구현하는 것은 불가능
- @OnlyOnce 애노테이션을 이용하면 가능
    - 메서드가 오직 한 번만 호출되어야 하는 사실을 컴파일러에게 공유함

→ 다양한 캐싱 기법을 통해 객체를 불변으로 유지하면서도 지연 로딩을 구현할 수 있음

### [불변 객체를 사용해야 하는 이유]

- 식별자 가변성 제거
- 실패 원자성 제거
- 시간적 결합
- 부수효과 제거
- NULL 참조 없애기
- 스레드 안전성
- 더 작고 더 단순한 객체
1. **식별자 가변성**
    - 식별자 가변성 문제 : 동일해 보이는 두 객체를 비교한 후 한 객체의 변경할 때 발생
        
        ```java
        Map<Cash, String> map = new HashMap<>();
        Cash five = new Cash("$5");
        Cash ten = new Cash("$10");
        
        map.put(five, "five");
        map.put(ten, "ten");
        
        five.mul(2);
        System.out.println(map); //결과 예측 불가
        ```
        
        → 동일하지 않은 두 객체를 HashMap에 독립적인 엔트리로 구성되어 있지만, 객체가 변경되면서 map에 변경을 알려주지 않기때문에 결과 예측 불가
        
    - 불변 객체는 식별자 가변성 문제 해결 가능
        - 객체를 map에 추가한 후 상태 변경이 불가능하기에 문제 발생 가능성 ZERO
2. **실패 원자성**
    - 실패 원자성 : 완전하고 견고한 상태의 객체를 가지거나 아니면 실패하거나 둘 중 하나만 가능한 특성
        
        ```java
        //가변 변수 - 실패 원자성 X
        class Cash {
        	private int a;
        	private int b;
        	public void mul(int factor) {
        		this.a*= factor; //처리됨
        		if(/*잘못됨,,,*/) { throw new RuntimeExcetpion("");}
        		this.b += factor; //처리되지 않음
        	}	
        }
        
        //불변 변수 - 실패 원자성 O
        class Cash {
        	private final int a;
        	private final int b;
        	public Cash mul(int factor) {
        		if(/*잘못됨,,,*/) { throw new RuntimeExcetpion("");}
        		return new Cash( this.a * factor, this.b * factor);//처리 되거나 처리되지 않음
        	}	
        }
        ```
        
    - 가변 객체를 사용하더라도 특별한 주의를 하면 실패 원자성 목표 달성 가능하지만 불변 객체는 별도의 처리 없이 목표 달성 가능
    - 불변 객체는 원자적이기 때문에 원자성 걱정 X
3. **시간적 결합**
    - 완전한 자바 빈 표준을 사용하면 시간적 결합이 생김
        
        ```java
        Cash price = new Cash();
        price.setCollars(29);
        System.out.println(price);
        price.setCents(95);
        ```
        
        - 코드를 재정렬하기 위해선 시간적인 결합을 이해해야함
        - 컴파일러는 도움이 되지 않기 때문에 프로그래머의 몫임..
    - 불변 객체를 사용하면 시간적인 결합 제거 가능
        
        ```java
        Cash price = new Cash(29, 95);
        System.out.println(price);
        ```
        
        - 하나의 문장으로 객체를 인스턴스화 하기 때문에 인스턴스화 + 초기화 분리 불가
        - 순서를 변경하게 되면 코드가 컴파일되지 않음
4. **부수효과 제거**
    - 불변 객체는 누구도 객체를 수정할 수 없어 객체의 상태가 변하지 않았다고 확신 가능
5. **NULL 참조 없애기**
    - 설정되지 않은(unset) 프로퍼티
        
        → 프로퍼티의 초기화 여부에 따라 객체가 누구인지를 판단하기 위해 사용
        
        ```java
        class User {
        	private String name = null; //unset property
        	public void setName(String txt) {
        		this.name = txt;
        	}
        }
        ```
        
        - NULL이 아닌지 체크해야 하고, 문제가 없는지 확인해야 함
        - 객체가 유효한 상태이고 언제 객체가 아닌 다른 형태로 바뀌는지 이해 어려움
    - 불변 객체라면 NULL을 포함시키는 것 불가능
6. **스레드 안전성**
    - 스레드 안정성 : 객체가 여러 스레드에 동시에 사용될 수 있으며 그 결과가 항상 예측가능하도록 유지할 수 있는 객체의 품질을 의미
    - 불변 객체는 실행 시점에 상태를 수정할 수 없게 금지함으로써 많은 스레드가 객체에 접근해도 문제가 없음
    - 명시적인 동기화 이용 시 가변 객체도 스레드 안전성 가능 🤔
        
        ```java
        class Cash{
        	private int a, b;
        	public void mul(int factor){
        		synchronized (this){
        			this.a *= factor;
        			this.b *= factor;
        		}
        	}
        }
        ```
        
        - 문제점 1 : 스레드 안전성 추가 어려움
        - 문제점 2 : 성능상의 비용 초래
        - 문제점 3 : 데드락 발생 가능성
7. **더 작고 더 단순한 객체**
    - 객체가 단순해질수록 응집도는 높아지고, 유지보수하기 쉬워짐
    - Java에서 허용 가능한 클래스의 최대 크기는 주석고 ㅏ공백을 포함해 250줄 정도임
        - 넘는 클래스는 리팩토링 하기
    - 불변 객체 크기 < 가변 객체 크기
        - 생성자 안에서만 상태 초기화할 수 있음

## 7. 문서를 작성하는 대신 테스트를 만드세요

- 더 읽기 쉬운 코드를 만들기 위해서는, 코드를 읽을 사람이 훨씬 더 멍청하다고 가정해야 함

### [나쁜 설계는 문서를 작성하도록 강요함]

- 코드만으로 클래스가 어떤 일을 하는지, 메서드의 목적이 무엇인지, 사용방법을 알 수 없기에 문서가 필요
- 훌룡하고 유지보수 가능한 클래스는 문서화가 필요하지 않음

⇒ 코드를 문서화하는 대신 코드를 깔끔하게 만들기

### [코드를 깔끔하게 만들기]

- 단위 테스트 역시 클래스의 일부로 단위 테스트도 함께 만들어야 함
    - 단위 테스트 : 클래스의 사용 방법을 보여줌
    - 문서 : 이해하고 해석하기 어려운 이야기 들려줌

→ 메인 코드만큼 단위테스트에도 관심을 기울이기

## 8. 모의 객체 대신 페이크 객체 사용하세요

- 모킹 : 모의 객체를 생성해 테스트 진행하는 것
- 페이크 클래스
    
    ```java
    interface Exchange {
    	float rate(String target);
    	float rate(String origin, String target);
    	final class Fake implements Exchange {
    		@Override
    		float rate(String target) { 
    			return this.rate("USD", target);
    		}
    		@Override
    		float rate(String origin, String target) { 
    			return 1.2345;
    		}
    	}
    }
    ```
    

### [모킹 대신 페이크 객체]

- 페이크 클래스 : 테스트를 더 짧게 만들 수 있고 유지보수성이 향상됨
- 모킹 : 테스트 장황해지고, 이해하거나 리팩토링하기 어려움
    
    ⇒ 가정을 사실로 전환시키기 때문에 단위 테스트 유지보수 어렵게 만듬
    

### [모킹 문제점]

- 클래스는 공개된 행동을 변경하지 않을 경우 단위 테스트는 실패해서는 안됨
    - 우리는 객체의 구현 방법을 알 권리가 없음
    - 객체와 의존 대상 사이의 상호작용 방식을 확인하거나 테스트해서는 안됨
        
        → 캡슐화하는 정보에 대해 알 권리 없음
        

### [페이크 객체]

- 만드는 인터페이스부터 페이크 클래스들을 함께 제공해야 함
    - 클래스의 모든 메서드가 인터페이스의 오퍼레이션을 구현하게 만들고, 인터페이스와 함께 페이크 클래스 제공해야 함
- 장점 : 인터페이스 설계에 관해 더 깊이 고민하도록 함
    - 항상 인터페이스에 대해 페이크 클래스를 만들어야 함

## 9. 인터페이스를 짧게 유지하고 스마트를 사용하세요

- 인터페이스 : 구현 클래스가 준수해야 하는 계약
    - 구현자에게 너무 많은 것을 요구하면 단일 책임 원칙을 위반하는 클래스를 만들도록 부추김
    
    ```java
    interface Exchange {
    	float rate(String target);
    	float rate(String source, String target);
    }
    ```
    
- 스마트 클래스 : 인터페이스가 어떻게 구현되는지 모르지만 인터페이스에서 특별한 기능 적용
    - 인터페이스를 구현하는 서로 다른 클래스 안에 동일한 기능 반복해 구현 X 가능
    
    ```java
    interface Exchange {
    	float rate(String source, String target);
    	final class Smart {
    		private final Exchange origin;
    		public float toUsd(String source) {
    			return this.origin.rate(source, "USD");
    		}
    		public float surToUsd() {
    			return this.toUsd("ERU");
    		}
    }
    
    //사용방법
    float rate = new Exchange.Smart(new NYSE()).eurToUsd();
    ```
    
    → 인터페이스는 하나의 메서드만 선언하고 제공자는 해당 메서드를 구현
    

### [요약]

- 기본적으로 인터페이스를 잛게 만들고 스마트 클래스를 인터페이스와 함께 배포함으로써 공통 기능 추출 + 코드 중복 피함
