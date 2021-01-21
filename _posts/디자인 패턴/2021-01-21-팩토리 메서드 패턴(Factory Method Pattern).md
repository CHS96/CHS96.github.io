---
title:  "팩토리 메서드 패턴(Factory Method Pattern)"
excerpt: "팩토리 메서드 패턴(Factory Method Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-01-21T18:35:00
---

팩토리 메서드 패턴은 **생성(Creational) 패턴 중 하나로써 상위 클래스에게 알려지지 않은 구체 클래스를 생성하는 패턴이며, 하위 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴**이다.  

간단하게 얘기하면 상위 클래스에서 객체를 직접 생성하지 않고, 팩토리(공장)라는 하위 클래스에게 위임하여 객체를 생성하도록 하는 디자인 패턴이다.  

팩토리는 결국 비슷한 기능을 수행하는 객체를 만들어내기 때문에 템플릿 메서드 패턴을 활용할 수 있다.  

팩토리 메서드 패턴의 구조는 다음과 같다.  

![팩토리 메서드 패턴1](https://user-images.githubusercontent.com/53072057/105272816-74c68780-5bdd-11eb-8849-982a99be00fa.JPG)  

* Product : 팩토리 메서드로 생성할 객체의 공통 인터페이스  
* ConcreteProduct : 생성할 객체를 구현하는 클래스  
* Creator : 팩토리 메서드를 정의하는 추상 클래스  
* ConcreteCreator : 팩토리 메서드의 기능을 구현하는 클래스, ConcreteProduct 객체를 생성  

간단한 예시를 통해 팩토리 메서드 패턴을 이해해 보자.  

Truck과 Van 두 종류의 자동차(객체)를 생산하는 자동차 공장이 있다고 하자. 팩토리 메서드 패턴을 적용시켜 코드로 구현해보자.  

먼저 **Product** 부분이다. 자신이 누군지를 나타내는 메서드를 가지는 interface Car를 구현한다.  

```java
package framework;

public interface Car {
	void whoAmI();
}
```

다음으로 **Cr0eator** 부분이다. Product인 자동차를 생산하는 방법인 CreatorCar 추상 클래스를 구현하고 팩토리 메서드를 정의해준다. 자동차를 생성하는 과정은 비슷하기 때문에 우리는 이전에 배운 템플릿 메서드 패턴을 활용할 수 있다.  

```java
package framework;

public abstract class CreatorCar {

	//FactoryMethod -> Template Method Pattern 활용한 것을 볼 수 있음
	public Car create_Car() {
		ready_Template();
		assemble_Parts();
		return complete();
	}
	
    //operation1
	abstract protected void ready_Template();

    //operation2
	private void assemble_Parts() { System.out.println("2. 부품 조립"); }
	
    //operation3
	abstract protected Car complete();
}
```

다음으로 **ConcreteProduct** 부분이다. 자동차 종류인 Truck과 Van 객체를 구현해준다.  

```java
package concrete;

import framework.Car;

public class Truck implements Car {
	@Override
	public void whoAmI() {
		System.out.println("I'm Truck!");
	}
}

public class Van implements Car {
	@Override
	public void whoAmI() {
		System.out.println("I'm Van!");
	}
}
```

다음으로 **ConcreteCreator** 부분이다. Truck과 Van를 생성하는 팩토리 메서드를 재정의해준다.  

```java
package concrete;

import framework.Car;
import framework.CreatorCar;

public class TruckCreator extends CreatorCar {

	@Override
	protected void ready_Template() {
		System.out.println("1. Truck 틀을 준비");
	}

	@Override
	protected Car complete() {
		System.out.println("3. Truck 완성!");
		return new Truck();
	}
}

public class VanCreator extends CreatorCar {

	@Override
	protected void ready_Template() {
		System.out.println("1. Van 틀을 준비");
	}
	
	@Override
	protected Car complete() {
		System.out.println("3. Van 완성!");
		return new Van();
	}
}
```

마지막으로 자동차 객체를 생성해줄 **Factory** 클래스를 구현해준다.  

```java
package concrete;

import framework.Car;

public class Factory {
	public Car create_Car(String type) {
		Car car = null;
		
		if (type.equals("Truck")) {
			car = new TruckCreator().create_Car();
		} else if (type.equals("Van")) {
			car = new VanCreator().create_Car();
		}
		
		return car;
	}
}
```

**Main** 클래스에서 이를 실행한 결과는 다음과 같다.  

```java
import concrete.Factory;
import framework.Car;

public class Main {

	public static void main(String[] args) {
		Factory factory = new Factory();
		Car car = factory.create_Car("Truck");
		car.whoAmI();
		System.out.println("-----------------");
		car = factory.create_Car("Van");
		car.whoAmI();
	}
}
```

![팩토리 메서드 패턴2](https://user-images.githubusercontent.com/53072057/105272819-75f7b480-5bdd-11eb-937d-29828d55d791.JPG)  

팩토리 메서드 패턴을 적용시켜 Main 클래스에서 자동차 객체를 직접 생성하지 않고 factory 클래스를 통해 객체를 생성하도록 하였다. 이렇게 구현한다면 객체 간의 결합도가 낮아지고 유지 보수가 용이해지는 장점이 있다.  
