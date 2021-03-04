---
title:  "데코레이터 패턴(Decorator Pattern)"
excerpt: "데코레이터 패턴(Decorator Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-03-04T18:35:00
---

데코레이터 패턴(Decorator Pattenr)은 **구조(Structural) 패턴 중 하나로써 주어진 상황 및 용도에 따라 어떤 객체에 책임(기능)을 동적으로 추가하는 패턴**이다.  

간단하게 얘기하면 데코레이터의 의미인 장식을 생각하면 쉽다. 기본 기능을 가지는 그릇을 하나 만들어주고 추가할 수 있는 기능(장식품)들을 추가하기 용이하도록 설계하는 디자인 패턴이다.  

즉, 기본 기능을 클래스로 구현해 준 후 상황에 따라 추가될 수 있는 기능들은 기능별로 별도의 클래스로 설계하여 필요할 때마다 **기본 기능 클래스 + 추가 기능 클래스의 조합**으로 객체를 생성하도록 하는 것이다.  

데코레이터 패턴의 구조는 다음과 같다.  

![데코레이터1](https://user-images.githubusercontent.com/53072057/109912680-e111cc00-7cef-11eb-8bca-00084efcf74d.JPG)  

* Component : 기본 기능과 추가될 수 있는 기능을 가지는 인터페이스, 두 기능을 가지도록 하는 이유는 Client가 하나의 인터페이스로 사용할 수 있도록 하기 위해서, 즉, Client는 Component 인터페이스를 통해서만 실제 객체를 사용  

* ConcreteComponent : 기본 기능을 구현하는 클래스  

* Decorator : 추가할 수 있는 기능을 가지는 인터페이스, 추가할 수 있는 기능 역시 다양하기 때문에 인터페이스를 통해 사용하기 쉽도록 함, Component 인터페이스를 상속받아 ConcreteComponent 객체에 접근할 수 있도록 해줌  

* ConcreteDecorator : 추가할 수 있는 기능들을 각각 구현하는 클래스  

*****

그렇다면 간단하게 라면 가게 예시를 통해 데코레이터 패턴을 이해해보자.  

라면 가게는 기본 라면뿐만 아니라 계란, 치즈 등 다양한 음식들을 토핑 할 수 있다고 하자. 만약 토핑을 통해 만들 수 있는 라면의 종류를 모두 클래스로 구현해 준다면 굉장히 많은 클래스들을 구현해줘야 할 것이다. (토핑 할 수 있는 음식의 수가 10개라면 가능한 조합은 2^10만큼 존재한다)  

간단하게 계란, 치즈, 두 가지 음식을 토핑 할 수 있다고 하자. 그렇다면 만들 수 있는 라면의 종류는 기본 라면, 기본 + 계란 라면, 기본 + 치즈 라면, 기본 + 계란 + 치즈 라면, 총 4가지가 존재한다. 이는 다음과 같이 구현할 수 있다.  

```java
class BaseRamen {
	/*
	 * 1. 기본 라면 추가
	 */
}

class BaseEggRamen {
	/*
	 * 1. 기본 라면 추가
	 * 2. 계란 추가
	 */
}

class BaseCheeseRamen {
	/*
	 * 1. 기본 라면 추가
	 * 2. 치즈 추가
	 */
}

class BaseEggCheeseRamen {
	/*
	 * 1. 기본 라면 추가
	 * 2. 계란 추가
	 * 3. 치즈 추가
	 */
}
```

하지만 이 코드는 상당히 비효율적이다. 왜냐하면 매번 새로운 라면을 만들때마다 그 라면에 들어가는 토핑들을 추가하는 기능을 구현해줘야 하기 때문이다. 이는 라면의 종류가 많으면 굉장히 코드가 길어지고 복잡해질 것이다.  

또한 Client 입장에서는 라면을 주문할 때마다 매번 해당하는 클래스의 객체를 생성해야 하는 번거로움이 있다.  

이를 개선하기 위해 어떻게 해야 할까? 설계를 효율적으로 하는 방법은 **추상화**로부터 시작된다.  

모든 라면은 특정한 음식을 추가하는 기능을 가진다. 또한, 토핑으로 만들 수 있는 모든 라면은 기본적으로 기본 라면이라는 틀을 가진다.  

![데코레이터2](https://user-images.githubusercontent.com/53072057/109912684-e242f900-7cef-11eb-936a-cb89e9054622.JPG)  

그렇다면 데코레이터 패턴을 적용시켜 개선해보자.  

먼저 모든 라면은 음식을 추가하는 기능을 공통적으로 가진다. 따라서 addSomething이라는 메서드를 가지는 **Component 인터페이스**를 구현해준다.  

```java
public interface RamenComponent {
	String addSomething();
}
```

다음으로 기본 라면 틀인 기본 기능을 제공하는 **ConcreteComponent 클래스**를 구현해준다. 이때 Client에서 RamenComponent 객체를 통해 접근할 수 있도록 RamenComponent 인터페이스를 상속받는다.  

```java
public class BaseComponent implements RamenComponent {
	
	@Override
	public String addSomething() {
		return "기본 라면";
	}
}
```

다음으로 **Decorator** 부분이다. 다양한 종류의 음식을 토핑 할 수 있기 때문에 **추상 클래스**로 구현해준다. 또한, Decorator도 Client가 RamenComponent 객체를 통해 접근할 수 있도록 RamenComponent의 객체를 주입받아준다.   

```java
abstract public class AddDecorator implements RamenComponent {
	
	private RamenComponent ramenComponent;
	
	public AddDecorator(RamenComponent ramenComponent) {
		super();
		this.ramenComponent = ramenComponent;
	}
	
	public String addSomething() {
		return ramenComponent.addSomething();
	}
}
```

마지막으로 계란과 치즈를 추가해주는 기능을 구현한 **ConcreteDecorator 클래스**이다.  

```java
//계란을 추가해주는 클래스
public class EggDecorator extends AddDecorator {

	public EggDecorator(RamenComponent ramenComponent) {
		super(ramenComponent);
	}
	
	public String addSomething() {
		return super.addSomething() + " + Egg";
	}
}

//치즈를 추가해주는 클래스
public class CheeseDecorator extends AddDecorator {

	public CheeseDecorator(RamenComponent ramenComponent) {
		super(ramenComponent);
	}

	public String addSomething() {
		return super.addSomething() + " + Cheese";
	}
}
```

**Main 클래스**는 다음과 같다. Client는 Component 객체만 사용하여도 다양한 라면의 조합을 주문할 수 있다.  

```java
public class Main {

	public static void main(String[] args) {
		RamenComponent baseRamen = new BaseComponent();
		System.out.println("Base Ramen : " + baseRamen.addSomething());
		
		RamenComponent baseEggRamen = new EggDecorator(new BaseComponent());
		System.out.println("Base Ramen with Egg : " + baseEggRamen.addSomething());
		
		RamenComponent baseCheeseRamen = new CheeseDecorator(new BaseComponent());
		System.out.println("Base Ramen with Cheese : " + baseCheeseRamen.addSomething());
		
		RamenComponent baseEggCheeseRamen = new CheeseDecorator(new EggDecorator(new BaseComponent()));
		System.out.println("Base Ramen with Egg and Cheese : " + baseEggCheeseRamen.addSomething());
	}
}
```

![데코레이터3](https://user-images.githubusercontent.com/53072057/109912686-e2db8f80-7cef-11eb-8320-a72d3e0c359d.JPG)  

데코레이터 패턴을 통해 기본 기능에서 다양한 기능을 추가할 경우 매번 새로운 클래스를 구현해주지 말고 추상화를 통해 새로운 기능을 유연하게 만들고 추가할 수 있게 되었다.  
