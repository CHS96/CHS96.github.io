---
title:  "전략 패턴(Strategy Pattern)"
excerpt: "전략 패턴(Strategy Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-01-11T18:35:00
---

전략 패턴(Strategy Pattern)은 **행위(Behavioral) 패턴 중 하나로써 실행 중에 알고리즘을 선택할 수 있게 하는 디자인 패턴**이다. 특정한 계열의 알고리즘들을 정의하고 각 알고리즘을 캡슐화하며 이 알고리즘들을 해당 계열 안에서 상호 교체가 가능하게 만드는 방법이다.  

간단하게 얘기하면 클라이언트가 수행하는 어떤 행위(알고리즘)를 클래스 별로 캡슐화하여 필요할 때마다 쉽게 행위를 교체할 수 있도록 해주는 방법이다. 별도의 클래스를 통해 클라이언트와 독립적으로 관리하기 때문에 새로운 기능을 추가하거나 수정한다 하여도 클라이언트는 아무런 영향을 받지 않는다.  

전략 패턴의 구조는 다음과 같다.  

![전략1](https://user-images.githubusercontent.com/53072057/104142207-de84ab80-53fd-11eb-8e90-a56d815267dd.JPG)  

* Context(문맥) : Strategy의 인터페이스를 이용하는 역할  
* Strategy(전략) : 클래스 별로 관리한 전략들을 결정하는 역할  
* ConcreteStrategy(구체적인 전략) : 클라이언트가 사용하는 전략을 실제로 구현하는 역할  

디자인 패턴은 글보다는 예시를 통해 이해하는 것이 편하니 간단하게 코드를 통해 전략 패턴을 이해해 보자.  

Client라는 객체가 마트에 가서 상품을 구매하는 상황이라고 생각해 보자. 상품을 구매하는 메서드를 구현한다면 일반적으로 다음과 같이 조건문을 통해 구현할 것이다.  

```java
class Client {
	public void buy_Product(String product) {
		if (product.equals("달걀")) {
			System.out.println("비용 : 300원");
		} else if (product.equals("우유")) {
			System.out.println("비용 : 500원");
		}
	}
}
```

그렇다면 만약 라면을 추가 및 구매하려면 어떻게 해야 할까? 마찬가지로 조건문을 통해 buy_Product 메서드에 추가해야 할 것이다.  

```java
class Client {
	public void buy_Product(String product) {
		if (product.equals("달걀")) {
			System.out.println("비용 : 300원");
		} else if (product.equals("우유")) {
			System.out.println("비용 : 500원");
		} else if (product.equals("라면")) {
			System.out.println("비용 : 700원");
		}
	}
}
```

매번 상품이 추가되거나 기존 상품의 비용이 변경될 때마다 메서드를 수정한다면 상당히 귀찮으며 코드도 길어지면서 난잡해질 것이다. 또한 클라이언트는 어떠한 이벤트가 발생하여도 모르도록 설계해야 하는데 이 방법은 클라이언트의 클래스 내에서 작업이 발생하기 때문에 좋은 방법이 아니다. 그렇다면 한 번 전략 패턴을 적용시켜 코드를 개선해보자.  

우리는 상품을 구매하는 것을 전략으로 생각할 수 있다. 따라서 상품 구매라는 메서드를 가지는 인터페이스를 구현한 후 각 상품들을 클래스로 선언하여 인터페이스를 상속하여 상품 구매 메서드를 재정의하도록 한다. 또한, Client 클래스는 단지 상품을 구매하는 메서드만 구현해놓는다.  

```java
interface Product {
	public void buy_Product();
}

class Egg implements Product {
	@Override
	public void buy_Product() {
		System.out.println("비용 : 300원");
	}
}

class Milk implements Product {
	@Override
	public void buy_Product() {
		System.out.println("비용 : 500원");
	}
}

class Ramen implements Product {
	@Override
	public void buy_Product() {
		System.out.println("비용 : 700원");
	}
}

class Client {
	private Product product;
	
	public Client(Product product) {
		this.product = product;
	}
	
	public void setProduct(Product product) {
		this.product = product;
	}
	
	public void buy_Product() {
		product.buy_Product();
	}
}
```

이런 식으로 행위들을 별도의 클래스를 통해 관리한다면 이후 상품을 추가하거나 비용을 변경한다 하여도 Client 클래스에는 아무런 영향을 끼치지 않는다. 또한 Client 클래스 내용이 상당히 깔끔해진 것을 확인할 수 있다. 
