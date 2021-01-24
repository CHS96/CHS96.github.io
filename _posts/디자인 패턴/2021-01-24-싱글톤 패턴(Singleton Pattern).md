---
title:  "싱글톤 패턴(Singleton Pattern)"
excerpt: "싱글톤 패턴(Singleton Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-01-24T18:35:00
---

싱글톤 패턴은 **생성(Creational) 패턴 중 하나로써 객체를 오직 하나만 생성하여 생성된 객체를 프로그램 어디에서나 접근하여 사용할 수 있도록 하는 패턴**이다.  

간단하게 얘기하면 필요할 때마다 똑같은 객체를 매번 생성하지 말고 기존에 생성한 하나의 객체를 활용하도록 하는 디자인 패턴이다.  

싱글톤 패턴의 구조는 다음과 같다.  

![싱글톤1](https://user-images.githubusercontent.com/53072057/105621635-0687fc00-5e4d-11eb-88e8-517ae7e8f86d.JPG)  

* Singleton : 하나의 객체만 생성하도록 보장해주며 getInstance() 메서드를 통해 다른 클래스에서 객체에 접근할 수 있도록 해주는 클래스  

간단한 예시를 통해 싱글톤 패턴을 이해해 보자.  

회사에서 직원들이 모두 사용하는 정수기가 있다고 하자. 정수기의 이름을 간단하게 A라고 하겠다. 정수기를 사용할 때마다 정수기의 물의 양을 1만큼 감소한다고 하자. 이를 코드로 구현하려면 어떻게 해야 할까?  

정수기 A는 분명히 회사에 1개만 존재하기 때문에 매번 사용할 때마다 기존의 물의 양을 알 수 있어야 한다. 정수기를 사용할 때마다 객체를 매번 생성해야 하는데 어떻게 기존의 물의 양을 알 수 있을까? 바로 싱글톤 패턴을 사용하는 것이다.  

싱글톤 패턴을 이해하기 위해선 **static**이라는 것을 이해해야 한다. 간단하게 설명하면 **자바에서 static 키워드를 사용하면 메모리에 한번 할당되어 프로그램이 종료될 때 해제가 된다.** 따라서 static을 통해 객체를 생성하면 프로그램이 종료되기 전까지 계속해서 메모리에 남아있기 때문에 다른 곳에서 똑같은 객체에 접근할 수 있도록 해주는 것이다.  

정수기 A 객체를 static을 사용해 선언하면 메모리에 정수기 A가 프로그램 종료 시까지 할당이 되면서 매번 정수기를 사용할 때마다 정수기 A의 메모리에 접근하여 물의 양을 1만큼 감소시켜주면 되는 것이다.  

이를 코드로 구현하면 다음과 같다.  

먼저 정수기의 객체와 생성자는 외부에서 접근하지 못하도록 **private**으로 선언해준다. 다만, 정수기 객체는 **static**를 붙여 만약 어떠한 방법으로 접근했다면 동일한 객체이도록 보장해준다.  

```java
public class Water_purifier {
    //private으로 선언하여 외부에서 접근 x
	//다만, 접근하였을때 동일한 객체를 위해 static으로 선언
    private static Water_purifier water_purifier;
	private int amount;

    //private으로 선언하여 외부에서 접근 x
	private Water_purifier() { setAmount(100); }

    public int getAmount() {
		return amount;
	}

	public void setAmount(int amount) {
		this.amount = amount;
	}
}
```

생성자가 private으로 선언되었기 때문에 우리는 외부에서 water_purifier의 객체를 생성할 수 없다. 이를 위해 우리는 static으로 선언한 water_purifier를 얻기 위해 **static 메서드**인 get_Instance를 하나 선언해준다. **static으로 선언한 변수는 static 메서드를 통해서만 접근할 수 있기 때문**이다.  

```java
//static 변수를 접근하도록 하기 위해 static 메서드로 선언
public static Water_purifier get_Instance() {
	//초기 호출 시 water_purifier 객체 생성
	if (water_purifier == null) {
		water_purifier = new Water_purifier();
	}
		
	return water_purifier;
}
```

water_purifier를 처음 생성할 때만 생성자를 통해 정수기 객체를 생성해 주고 이후부터는 메서드가 호출되면 기존의 water_purifier를 반환해준다.  

최종적인 정수기 클래스 코드는 다음과 같다.  

```java
public class Water_purifier {
	//private으로 선언하여 외부에서 접근 x
	//다만, 접근하였을때 동일한 객체를 위해 static으로 선언
	private static Water_purifier water_purifier;
	private int amount;
	
	//private으로 선언하여 외부에서 접근 x
	private Water_purifier() { setAmount(100); }
	
	//static 변수를 접근하도록 하기 위해 static 메서드로 선언
	public static Water_purifier get_Instance() {
		//초기 호출 시 water_purifier 객체 생성
		if (water_purifier == null) {
			water_purifier = new Water_purifier();
		}
		
		return water_purifier;
	}
	
	public int getAmount() {
		return amount;
	}

	public void setAmount(int amount) {
		this.amount = amount;
	}

	//정수기를 사용할때마다 물의 양을 1만큼 감소
	public void use_Water_Purifier() {
		amount--;
	}
}
```

그렇다면 Main 함수에서 동일한 객체를 사용하는지 테스트해보자.  

우선 private으로 선언하였기 때문에 직접 water_purifier 객체를 생성할 수 없다.  

```java
//private이기 때문에 직접 객체 생성 x
//Water_purifier water_purifier = new Water_purifier;
```

정수기 1과 정수기 2를 get_Instance() 메서드를 통해 생성한 후 정수기 2를 사용해보자.  

```java
Water_purifier water_purifier1 = Water_purifier.get_Instance();
Water_purifier water_purifier2 = Water_purifier.get_Instance();
		
System.out.println("water_purifier1 물의 양 : " + water_purifier1.getAmount());
System.out.println("water_purifier2 물의 양 : " + water_purifier2.getAmount());
		
System.out.println("==========================");
System.out.println("water_purifier2 사용");
water_purifier2.use_Water_Purifier();
System.out.println("==========================");
	
System.out.println("water_purifier1 물의 양 : " + water_purifier1.getAmount());
System.out.println("water_purifier2 물의 양 : " + water_purifier2.getAmount());
```

결과는 다음과 같다.  

![싱글톤2](https://user-images.githubusercontent.com/53072057/105621636-07b92900-5e4d-11eb-8577-715c8b7f4482.JPG)  

정수기 2를 사용했지만 정수기 1, 정수기 2 모두 같은 메모리를 참조하고 있기 때문에 두 정수기의 물의 양이 함께 1만큼 감소된 것을 확인할 수 있다.  

그렇다면 두 객체가 정말로 같은 메모리를 참조하는지 확인해보자.  

```java
System.out.println(water_purifier1.hashCode() + " == " + water_purifier2.hashCode());
System.out.println(water_purifier1.equals(water_purifier2));
```

![싱글톤3](https://user-images.githubusercontent.com/53072057/105621637-07b92900-5e4d-11eb-866f-29ad94b6f61a.JPG)  

두 객체가 같은 객체인 것을 가지는 것을 확인할 수 있다.  

Main 클래스의 전체 코드는 다음과 같다.  

```java
public class Main {
	
	public static void main(String[] args) {
		//private이기 때문에 직접 객체 생성 x
		//Water_purifier water_purifier = new Water_purifier;
		
		Water_purifier water_purifier1 = Water_purifier.get_Instance();
		Water_purifier water_purifier2 = Water_purifier.get_Instance();
		
		System.out.println("water_purifier1 물의 양 : " + water_purifier1.getAmount());
		System.out.println("water_purifier2 물의 양 : " + water_purifier2.getAmount());
		
		System.out.println("==========================");
		System.out.println("water_purifier2 사용");
		water_purifier2.use_Water_Purifier();
		System.out.println("==========================");
		
		System.out.println("water_purifier1 물의 양 : " + water_purifier1.getAmount());
		System.out.println("water_purifier2 물의 양 : " + water_purifier2.getAmount());
		System.out.println("==========================");
		
		System.out.println(water_purifier1.hashCode() + " == " + water_purifier2.hashCode());
		System.out.println(water_purifier1.equals(water_purifier2));
	}
}
```

싱글톤 패턴을 사용하면 매번 new 연산을 통해 객체를 생성하지 않아도 되기 때문에 메모리를 효율적으로 사용할 수 있는 장점이 있다. 또한, static 객체이기 때문에 외부에서 객체의 데이터를 공유하기가 쉽다. 따라서 DBCP (Database Connection Pool)처럼 공통된 객체를 여러 개 생성해서 사용해야 하는 상황에 많이 사용된다.  

다만, 싱글톤 패턴을 너무 많이 사용하면 클래스의 객체들 간의 결합도가 높아지는 단점이 있다. 또한, 다중 스레드 환경에서 동기화 문제가 발생할 수 있다. 물론 동기화 문제를 위해 싱글톤 객체를 선언하는 방법도 있지만 여기서는 싱글톤 패턴의 개념을 알아보기 위한 것이기 때문에 따로 다루지는 않겠다.  

따라서 싱글톤 패턴은 장단점이 존재하기 때문에 상황에 맞게 적절하게 활용하도록 하자.  
