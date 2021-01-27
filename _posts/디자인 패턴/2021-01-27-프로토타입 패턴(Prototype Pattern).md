---
title:  "프로토타입 패턴(Prototype Pattern)"
excerpt: "프로토타입 패턴(Prototype Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-01-27T18:35:00
---

프로토타입 패턴은 **생성(Creational) 패턴 중 하나로써 생성할 객체들의 타입이 프로토타입인 인스턴스로부터 결정되도록 하며, 인스턴스는 새 객체를 만들기 위해 자신을 복제(clone) 하는 패턴**이다.  

프로토타입은 완제품을 출시하기 전에 테스트할 수 있는 시제품, 원형을 의미한다.  

간단하게 얘기하면 객체를 수정하거나 테스트할 때마다 매번 새로운 객체를 생성하지 않고 기존에 만들어놨던 객체(시제품)를 복제하여 필요한 부분만 수정하는 디자인 패턴이다.  

프로토타입 패턴의 구조는 다음과 같다.  

![프로토1](https://user-images.githubusercontent.com/53072057/105935331-21e54800-6095-11eb-9f76-2bde01dd9ac0.JPG)  

* Prototype : 시제품(공통적인 기능)을 정의하는 인터페이스  
* ConcretePrototype : 시제품을 실제로 구현하는 클래스  

간단한 예시를 통해 프로토타입 패턴을 이해해 보자.  

2차원 좌표에 도형을 그리는 상황이라고 가정하자. 다음과 같이 자신을 표현하는 whoAmI() 메서드를 가지는 Shape 인터페이스와 이를 상속받는 Rectangle 클래스가 있다고 하자.  

```java
public interface Shape {
	void whoAmI();
}

public class Rectangle implements Shape {

	private int start_Y, start_X, width, height;
	
	public Rectangle(int start_Y, int start_X, int width, int height) {
		this.start_Y = start_Y;
		this.start_X = start_X;
		this.width = width;
		this.height = height;
	}

	public int getStart_Y() {
		return start_Y;
	}

	public void setStart_Y(int start_Y) {
		this.start_Y = start_Y;
	}

	public int getStart_X() {
		return start_X;
	}

	public void setStart_X(int start_X) {
		this.start_X = start_X;
	}

	public int getWidth() {
		return width;
	}

	public void setWidth(int width) {
		this.width = width;
	}

	public int getHeight() {
		return height;
	}

	public void setHeight(int height) {
		this.height = height;
	}

	@Override
	public void whoAmI() {
		System.out.println("I am Rectangle");
		System.out.println("Rectangle [start_Y=" + start_Y + ", start_X=" + start_X + 
                           ",  width=" + width + ", height=" + height+ "]");
	}
}
```

Main 클래스에서 시작 좌표(3, 3), 너비(5), 높이(5)를 가지는 직사각형 객체를 하나 생성했다고 하자.  

```java
public class Main {
	public static void main(String[] args) {
		Rectangle rectangle1 = new Rectangle(3, 3, 5, 5);
		rectangle.whoAmI();
	}
}
```

만약 방금 생성했던 직사각형을 좌측으로 1만큼 이동시킨 새로운 직사각형 객체가 필요하다면 어떻게 해야 할까? 새로운 객체에 rectangle1의 모든 값들을 다 복사한 후 start_X의 값을 1만큼 증가시켜야 한다.  

```java
public class Main {
	public static void main(String[] args) {
		Rectangle rectangle1 = new Rectangle(3, 3, 5, 5);

		Rectangle rectangle2 = new Rectangle(rectangle1.getStart_X(), rectangle1.getStart_Y()
				,rectangle1.getWidth(), rectangle1.getHeight());
		rectangle2.setStart_X(rectangle1.getStart_X() + 1);

		rectangle1.whoAmI();
		rectangle2.whoAmI();
	}
}
```

![프로토2](https://user-images.githubusercontent.com/53072057/105935333-227dde80-6095-11eb-90cb-0aa32b40d8c2.JPG)  

지금은 직사각형의 멤버 변수들이 4개뿐이라서 간단해 보이지만 멤버 변수가 굉장히 많다면 상당히 코드가 지저분해질 것이다. 그렇다면 프로토타입 패턴을 적용시켜 개선해보자.  

Java에서는 Cloneable 인터페이스를 상속하여 복제하는 clone() 메서드를 사용할 수 있다.  

```java
public interface Shape extends Cloneable {
	void whoAmI();
}
```

Rectangle 클래스에 clone() 메서드를 활용하여 복제하는 메서드 copy()를 구현해준다. 다만, Cloneable의 clone() 메서드는 Object 클래스의 메서드이기 때문에 해당 클래스 타입으로 형변환해줘야 한다.  

```java
public Rectangle copy() throws CloneNotSupportedException {
		Rectangle rectangle = (Rectangle) clone();
		return rectangle;
}
```

Main 클래스에서 실행해보자.  

```java
public class Main {
	public static void main(String[] args) throws CloneNotSupportedException {
		Rectangle rectangle1 = new Rectangle(3, 3, 5, 5);
		Rectangle rectangle2 = rectangle1.copy();
		rectangle2.setStart_X(rectangle2.getStart_X() + 1);
		rectangle1.whoAmI();
		rectangle2.whoAmI();
	}
}
```

![프로토3](https://user-images.githubusercontent.com/53072057/105935335-23167500-6095-11eb-9509-1dcbe99fae74.JPG)  

간단한 copy() 메서드를 통해 원하는 대로 잘 동작하는 것을 확인할 수 있다.  

프로토타입 패턴은 비슷한 기능을 하는 객체일 경우 new 연산을 통해 매번 객체를 생성하지 않고 기존의 객체(시제품)의 값을 복제하는 방법으로 객체를 생성한다. new 연산을 통해 생성자를 호출하는 경우 생성자 내의 코드를 실행해야 하기 때문에 연산이 많아진다. 하지만 복제를 통해 객체를 생성한다면 연산을 줄일 수 있는 장점이 있다.  

다만, 프로토타입을 활용해 객체를 복제할 경우 **깊은 복사(Deep copy), 얕은 복사(Shallow copy)**를 주의해야 한다.  

깊은 복사는 **두 객체가 같은 주소를 참조하지 않기 때문에 하나의 객체를 수정하여도 다른 객체에 영향을 끼치지 않는다.**  

```java
public class Main {
	public static void main(String[] args) throws CloneNotSupportedException {
		int[] object1 = {1, 2};
		
		//object1의 값을 object2에 Deep_copy
		int[] object2 = new int[2];
		object2[0] = object1[0];
		object2[1] = object1[1];
		
		//objcet2의 0번 원소를 변경
		object2[0] = 3;
		
		//다른 주소이기 때문에 서로 영향 x
		System.out.println(object1[0] + " " + object1[1]);
		System.out.println(object2[0] + " " + object2[1]);
	}
}
```

![프로토4](https://user-images.githubusercontent.com/53072057/105935337-23af0b80-6095-11eb-9df9-709d85d28590.JPG)  
​
반대로 얕은 복사는 **두 객체가 같은 주소를 참조하기 때문에 하나의 객체를 수정한다면 다른 객체에도 영향을 끼친다.**  

```java
public class Main {
	public static void main(String[] args) throws CloneNotSupportedException {
		int[] object1 = {1, 2};
		
		//object1의 값을 object2에 Shallow copy
		int[] object2 = object1;
		
		//objcet2의 0번 원소를 변경
		object2[0] = 3;
		
		//같은 주소이기 때문에 서로 영향 o
		System.out.println(object1[0] + " " + object1[1]);
		System.out.println(object2[0] + " " + object2[1]);
	}
}
```

![프로토5](https://user-images.githubusercontent.com/53072057/105935338-23af0b80-6095-11eb-899e-bef8bb8d6225.JPG)  

자바에서 제공해 주는 Wrapper class인 경우 자동으로 깊은 복사를 지원해 주지만 int, boolean 같은 기본 타입인 경우는 자동으로 지원해 주지 않기 때문에 copy()에서 명시적으로 재정의 해줘야 한다.  

그렇다면 깊은 복사, 얕은 복사 중 어떤 것을 사용해야 할까? 정답은 없다. 만약 복사할 객체가 immutable 객체라면 얕은 복사를 사용하고 객체가 다른 mutable 객체를 참조한다면 상황에 맞게 적절히 활용해야 한다.  
