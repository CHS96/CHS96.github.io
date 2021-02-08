---
title:  "추상 팩토리 패턴(Abstract Factory Pattern)"
excerpt: "추상 팩토리 패턴(Abstract Factory Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-02-08T18:35:00
---

추상 팩토리 패턴은 **생성(Creational) 패턴 중 하나로써 구체적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공하는 패턴**이다.  

간단하게 얘기하면 관련성이 높은 여러 종류의 객체를 따로 생성하지 않고 팩토리라는 공장을 통해 생성하는 디자인 패턴이다.  

추상 팩토리 패턴의 구조는 다음과 같다.  

![추상팩토리패턴1](https://user-images.githubusercontent.com/53072057/107172112-ea1cbf80-6a07-11eb-91eb-da4059fb7453.JPG)  

이전에 공부한 팩토리 메서드 패턴과 이름이 굉장히 비슷하다. 두 패턴 모두 객체의 생성을 캡슐화를 통해 Factory 클래스에게 위임하여 객체를 생성하는 공통점이 있지만 팩토리 메서드 패턴은 한 팩토리당 한 종류의 객체만 생성할 수 있고 추상 팩토리 패턴은 **한 팩토리에서 서로 연관된 여러 종류의 객체를 생성할 수 있다**는 차이가 존재한다.  

무슨 말인지 이해가 안 될 수도 있으니 간단하게 이해하고 넘어가자.  

이전 팩토리 메서드 패턴에서 사용한 자동차를 생성하는 Factory 클래스이다.  

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

위의 Factory 클래스에서는 create_Car 메서드를 통해 Car 타입의 객체만 생성할 수 있다. 만약 공통적인 기능을 가지는 Car 타입의 자동차를 생성하는 것이 아니라 자동차와 관련이 있지만 조금 느낌이 다른 자동차의 부품을 생성한다고 한다면 어떨까?  

자동차 차제, 바퀴, 창문, 엔진 등 다양한 부품이 존재한다. 물론 Factory 클래스에 각 부품을 생성하는 메서드를 만들어줘서 해결할 수도 있다.  

```java
package concrete;

import framework.Car;

public class CarPartFactory {
	public CarBody create_CarBody(String type) {
		CarBody carBody = null;
		
		if (type.equals("A")) {
			carBody = new ACreator().create_CarBody();
		} else if (type.equals("B")) {
			carBody = new BCreator().create_CarBody();
		}
		
		return carBody;
	}

    public CarWheel create_CarWheel(String type) {
		CarWheel carWheel = null;
		
		if (type.equals("A")) {
			carWheel = new ACreator().create_CarWheel();
		} else if (type.equals("B")) {
			carWheel = new BCreator().create_CarWheel();
		}
		
		return carWheel ;
	}

    //각 부품마다 메서드를 만들어줌
    //매번 부품을 추가할때마다 코드가 길어지고 복잡해짐...
}
```

하지만 부품이 많을수록 계속해서 메서드를 추가해야 하기 때문에 코드가 길어지는 단점이 있다. 또한, 결국 각 부품마다 if-else 문을 통해 종류를 분류하기 때문에 비효율적인 코드이다. 그렇다면 한 번 개선시켜 보도록 하자.  

객체지향 프로그래밍에서 가장 중요하다고 생각하는 특징이 바로 **추상화**이다. 결국 위 코드는 부품을 생성하는 코드가 중복된다는 것을 알 수 있다. 또한, 부품의 타입도 마찬가지로 따지고 보면 부품의 타입이라는 큰 틀로 추상화 시킬 수 있을 것 같다. 여기서 나온 것이 바로 **추상 팩토리 패턴**이다.  

이름 그대로 객체를 생성하는 팩토리를 추상화시키겠다는 의미이다. 자동차의 부품을 생성하는 Factory를 추상화 시켜 Factory 인터페이스라는 큰 틀을 정의하고 이를 상속받는 각 부품의 Factory 클래스를 구현하여 연관된 객체들을 생성하는 것이다. 즉, **추상화를 통해 한 팩토리에서 서로 연관된 여러 종류의 객체를 생성할 수 있게 된다.**  

그렇다면 간단하게 추상 팩토리 메서드 패턴을 적용한 자동차의 부품들을 생성하는 예제를 살펴보자.  

먼저 **Product**이다. 자동차의 부품 중 차체와 바퀴를 생산한다고 가정하자. 각 부품도 다양한 종류가 존재하기 때문에 인터페이스라는 큰 틀로 구현해준다.  

```java
package Product;

public interface Body {
	public void whoAmI();
}

public interface Wheel {
	public void whoAmI();
}
```

Benz와 BMW 두 종류의 자동차 부품을 생성한다고 가정하고 각 부품을 클래스로 구현해준다.  

```java
package Product;
------------------------- [ Benz ] -------------------------
public class BenzBody implements Body {
	@Override
	public void whoAmI() {
		System.out.println("I'm Benz Body");
	}
}

public class BenzWheel implements Wheel {
	@Override
	public void whoAmI() {
		System.out.println("I'm Benz Wheel");
	}
}
------------------------- [ BMW ] --------------------------
public class BmwBody implements Body {
	@Override
	public void whoAmI() {
		System.out.println("I'm Bmw Body");
	}
}

public class BmwWheel implements Wheel {
	@Override
	public void whoAmI() {
		System.out.println("I'm Bmw Wheel");
	}
}
```

다음으로 **Factory**이다. 먼저 부품을 생산하는 메서드를 가지는 큰 틀인 **CarAbstractFactory** 인터페이스를 구현해준다.  

```java
package Factory;

import Product.Body;
import Product.Wheel;

public interface CarAbstractFactory {
	public Body create_Body();
	public Wheel create_Wheel();
}
```

다음으로 Benz와 Bmw 두 종류의 부품을 생산해주는 **Factory** 클래스를 구현해준다.  

```java
package Factory;

import Product.BenzBody;
import Product.BenzWheel;
import Product.Body;
import Product.Wheel;

public class BenzFactory implements CarAbstractFactory {

	@Override
	public Body create_Body() {
		return new BenzBody();
	}

	@Override
	public Wheel create_Wheel() {
		return new BenzWheel();
	}
}
```

```java
package Factory;

import Product.BmwBody;
import Product.BmwWheel;
import Product.Body;
import Product.Wheel;

public class BmwFactory implements CarAbstractFactory {

	@Override
	public Body create_Body() {
		return new BmwBody();
	}

	@Override
	public Wheel create_Wheel() {
		return new BmwWheel();
	}
}
```

마지막으로 자동차를 실제로 찍어내는 공장인 **CarFactory** 클래스를 구현해준다. Main 클래스에서 사용할 것이기 때문에 **static** 키워드를 달아준다. 매개변수로 CarAbstractFactory 타입의 객체를 주입받아 각 부품을 생산해준다.  

```java
package Factory;

import Product.Body;
import Product.Wheel;;

public class CarFactory {
	public static Body getBody(CarAbstractFactory carAbstractFactory) {
		return carAbstractFactory.create_Body();
	}
	
	public static Wheel getWheel(CarAbstractFactory carAbstractFactory) {
		return carAbstractFactory.create_Wheel();
	}
}
```

**Main** 클래스에서는 다음과 같이 CarFactory 클래스를 통해 원하는 종류의 부품을 생산할 수 있게 된다.  

```java
import Factory.BenzFactory;
import Factory.BmwFactory;
import Factory.CarFactory;
import Product.Body;
import Product.Wheel;

public class Main {

	public static void main(String[] args) {
		Body benzBody = CarFactory.getBody(new BenzFactory());
		Wheel benzWheel = CarFactory.getWheel(new BenzFactory());
		benzBody.whoAmI();
		benzWheel.whoAmI();
		System.out.println("===============");
		Body bmwBody = CarFactory.getBody(new BmwFactory());
		Wheel bmwWheel = CarFactory.getWheel(new BmwFactory());
		bmwBody.whoAmI();
		bmwWheel.whoAmI();
	}
}
```

![추상팩토리패턴2](https://user-images.githubusercontent.com/53072057/107172116-eab55600-6a07-11eb-945a-9201337409ce.JPG)  

팩토리 메서드 패턴은 한 객체의 종류만 생성할 수 있었지만 추상 팩토리 패턴은 객체를 생성하는 Factory를 추상화시켜 다양한 종류의 객체를 생성할 수 있다는 장점이 있다.  

