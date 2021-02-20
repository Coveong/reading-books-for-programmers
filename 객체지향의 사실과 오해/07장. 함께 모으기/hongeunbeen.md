# 7. 함께 모으기

> 코드와 모델을 밀접하게 연관시키는 것은 코드에 의미를 부여하고 모델을 적절하게 한다.
- 에릭 에반스(Eric Evans)

마틴 파울러 [UML Distilled 2판]

객체 지향설계 안에 존재하는 세가지 상호 연관된 관점 - 개념 관점, 명세 관점, 구현 관점

# 개념 관점(Conceptual Perspective)

- 설계는 도메인 안에 존재하는 개념과 개념들 사이의 관계 표현
- 실제 도메인의 규칙과 제약을 최대한 유사하게 반영하는 것 핵심

# 명세 관점(Specification Perspective)

- 사용자의 영역인 도메인을 벗어나 개발자의 영역인 소프트웨어로 초점 옮김
- 객체들의 책임에 초점을 맞추어 객체의 인터페이스 바라보고 객체가 협력을 위해 '무엇'을 할 수 있는가 생각

→ 명세 관점과 구현 관점을 명확하게 분리!!

# 구현 관점(Implementation Perspective)

- 프로그래머인 우리에게 가장 익숙한 관점, 실제 작업을 수행하는 코드와 연관
- 객체들이 책임을 수행하는데 필요한 동작하는 코드 작성
- 인터페이스를 구현하는데 필요한 속성과 메서드를 클래스에 추가

# 세가지 관점

- 개념 관점 → 명세 관점 → 구현 관점 순서대로 소프트웨어 개발 의미 X
- 개념 관점, 명세 관점, 구현 관점 동일한 클래스를 세 가지 다른 방향에서 바라보는 것 의미

## 클래스

 세 가지 관점 통해 설계와 관련된 다양한 측면 드러냄

- 클래스가 은유하는 개념 = 도메인 관점 반영
- 클래스의 공용 인터페이스 = 명세 관점 반영
- 클래스의 속성과 메서드 = 구현 관점 반영

→ 세 가지 관점을 모두 수용할 수 있도록 개념, 인터페이스, 구현을 함께 드러내야 함

→ 코드 안에서 세 가지 관점을 쉽게 식별할 수 있도록 깔끔하게 분리 중요

# 커피 전문점 도메인

커피 전문점에서 커피를 주문하는 과정을 객체들의 협력 관계로 구현

## 개념 관점

커피 전문점이라는 도메인

손님 객체, 메뉴 항목 객체, 메뉴판 객체, 바리스타 객체, 커피 객체 존재

## 객체들 간의 관계

- 손님과 메뉴판 객체 사이 관계 존재, 손님과 바리스타 객체 사이 관계 존재

→ 동적인 객체를 정적인 타입으로 추상화해 복잡성 낮추기!

## 객체 타입

타입은 분류를 위해 사용되고 상태와 무관하게 동일하게 행동할 수 있는 객체들 동인한 타입으로 분류

- 커피 전문점 구성 범주 : 손님 타입, 메뉴판 타입, 메뉴 항목 타입, 바리스타 타입, 커피 타입

## 타입 간의 관계

각 타입간의 관계를 합성 관계로 단순화

- 포함(containment) 관계, 합성(composition) 관계
- 연관(association) 관계
    - 한 타입의 인스턴스가 다른 타입의 인스턴스를 포함하지는 않지만 서로 알고 있어야 할 경우

→ 도메인 모델이란 소프트웨어가 대상으로 하는 영역인 도메인을 단순화해서 표현한 모델 

→ 어떤 타입이 도메인을 구성하느냐와 타입들 사이에 어떤관계 존재 파악 중요

# 설계하고 구현하기

훌륭한 협력은 객체가 메시지를 선택하는것이 아닌 메시지가 객체를 선택

- 도메인 모델 안 책임을 수행하기에 적절한 타입 존재여부 확인
- 객체를 그 타입의 인스턴스로 생성

→ 유사성을 통해 소프트웨어 객체가 수행하는 책임과 상태 쉽게 유추 가능

현실 속 사물이 수동적인 존재라도 객체지향 세계에서는 모든 객체가 능동적이고 자율적인 존재!

## 인터페이스 정리하기

객체가 수신한 메시지가 객체의 인터페이스 결정

→ 메시지가 객체를 선택하고 선택된 객체는 메시지를 자신의 인터페이스로 받아들임

수신 가능한 메시지만 추려내면 객체의 인터페이스

→ 그 객체의 인터페이스 안에 메시지에 해당하는 오퍼레이션 존재 의미

### 객체의 타입 구현

- 일반적인 방법 : 클래스
- 공용 인터페이스는 인터페이스에 포함된 오퍼페이션 역시 외부에서 젖ㅂ근 가능하도록 공용으로 선언

    ```java
    class Customer{
    	public void order(String menuName){}
    }

    class MenuItem{
    }

    class Menu{
    	public MenuItem choose(String name) {}
    }

    class Barista {
    	public Coffee makeCoffee(MenuItem menuItem) {}
    }

    class Coffee {
    	public Coffee(MenuItem menuItem) {}
    }
    ```

## 구현하기

객체는 자신과 협력하려고 하는 객체에 대한 참조를 알고 있어야 함

참조를 얻는 방법은 다양한 방법 존재

```java
class Customer {
	public void order(String menuName, Menu menu, Barista barista) {//Menu 참조, Barista 참조
		MenuItem = menu.choose(menuName);
		Coffee coffee = barista.makeCoffee(menuItem);
		...
	}
}

class Menu {
	private List<MenuItem> itmes;//MenuItem 참조(MenuItem을 찾아야 하는 책임과 관리)
	
	public Menu(List<MenuItem> items) {
		this.items= items;
	}

	public MenuItem choose(String name) {
		for(MenuItem each : items) {
			if( each.getNAme().equals(name)){
				return each;
			}
		}
		return null;
	}
}
```

→ `Menu`의 속성으로 `MenuItem`의 목록을 포함시킨 결정은 클래스 구현 도중 내려짐!

→ 객체의 속성은 내부 구현으로 캡슐화(인터페이스에는 객체 내부 속성 정보X)

객체가 어떤 책임을 수행해야 하는지를 결정한 후 책임을 수행하는 데 필요한 객체의 속성 결정!!

```java
//Barista는 MenuItem을 이용해 커피 제조
class Barista {
	public Coffee makeCoffee(MenuItem menuItem) {
		Coffee coffee = new Coffee(menuItem);
		return coffee;
	}
}

//커피이름, 가격 - 속성
class Coffee {
	private String name;
	private int price;

	public Coffee(MenuItem menuItem) {
		this.name = menuItem.getName();
		this.price = menuItem.cost();
	}
}

//MenuItem은 getName()과 cost() 메시지에 응답할 수 있도록 메서드 구현
public class MenuItem {
	private String name;
	private int price;

	public MenuItem(String name, int price) {
		this.name = name;
		this.price = price;
	}

	public int cost() {
		return price;
	}

	public String getName() {
		return name;
	}
}
```

인터페이스는 객체가 다른 객체와 직접적으로 상호작용하는 통로!

설계를 간단히 끝내고 구현에 돌입하자!!!

# 코드와 세 가지 관점

→ 코드는 세 가지 관점(개념 관점, 명세 관점, 구현 관점)을 모두 제공해야 한다!!

- 개념 관점
    - 코드(클래스) - 도메인 구성 중요한 개념과 관계
    - 소프트웨어 클래스가 도메인 개념의 특성을 최대한 수용하며 변경 관리 쉬고 유지보수성 향상 가능
- 명세 관점
    - 클래스의 인터페이스 - 공용 인터페이스
    - 변화에 안정적인 인터페이스 위해 구현과 관련된 세부사항 숨기기
- 구현 관점
    - 클래스의 내부 구현 - 메서드와 속성(공용 인터페이스 일부X)
    - 메서드의 구현과 속성의 변경은 외부의 객체에게 영향X

→ 하나의 클래스 안 세 가지 관점 모두 포함 + 각 관점에 대응 요소 명확 깔끔 가능!!

도메인 개념 참조 이유?

- 도메인 에 대한 지식을 기반으로 코드의 구조와 의미 쉽게 유추 가능(유지보수성)

인터페이스와 구현 분리

- 명세관점은 클래스의 안정적인 측면 → 실제로 훌륭한 설계 결정 측면
- 구현 관점은 클래스의 불안정한 측면

→ 세 가지 관점 모두에서 클래스를 바라볼 수 있으려면 훌륭한 설계가 뒷받침!!
