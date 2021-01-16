---
title:  "추상 클래스(Abstract Class) vs 인터페이스(Interface)"
excerpt: "추상 클래스(Abstract Class) vs 인터페이스(Interface)를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-01-16T18:35:00
---

추상 클래스와 인터페이스는 비슷하면서도 다른 개념이다. 둘의 차이를 알고 있지만 이번 기회에 제대로 한 번 공부 및 정리해보려고 한다.  

먼저 추상 클래스이다. Java에서 클래스는 일반 클래스와 추상 클래스로 나뉘는데 추상 클래스는 **추상 메서드(완전하게 구현되어 있지 않은 메서드)를 0개 이상 가지고 있는 클래스**를 의미한다. 추상 클래스는 추상 메서드뿐만 아니라 멤버 변수와 일반 메서드를 가질 수 있다. 추상 클래스와 추상 메서드는 **abstract** 키워드를 사용하여 구현한다.  

추상 클래스는 메서드가 미완성되어 있기 때문에 객체를 생성할 수 없다. 오직 상속받은 자식 클래스에서만 객체를 생성할 수 있다.  

추상 클래스는 **상속을 통해 추상 메서드를 자식 클래스가 재정의하도록 기능을 확장시키는 것이 목적**이다. 클래스이기 때문에 상속을 받기 위해서 자식 클래스는 **extends** 키워드를 사용한다.  

그렇다면 간단한 예시를 통해 추상 클래스를 이해해 보자. 동물을 나타내는 상속 계층도를 생각해 보자. 동물을 나타내는 Animal 클래스를 일반 클래스로 정의한다면 어떤 문제가 발생할까?  

예를 들어 동물의 움직임을 나타내는 move() 메서드를 구현한다고 해보자. 과연 구체적인 동작을 하는 move() 메서드를 구현할 수 있을까? 동물마다 움직임이는 방법이 다를 것이다. 강아지는 네 발로 걸을 것이고 원숭이는 두 발로 걸을 것이다.  

```java
class Animal {
    //구체적인 동작을 정의하기 어려움
    //강아지는 네 발, 원숭이는 두 발, 음...
	public void move() {
		System.out.println("?발로 걷기"); 
	}
}
```

물론 하위 클래스에서 move() 메서드를 오버라이딩하여 재정의하는 방법이 존재한다.  

```java
class Dog extends Animal {
	public void move() {
		System.out.println("네 발로 걷기");
	}
}

class Monkey extends Animal {
	public void move() {
		System.out.println("두 발로 걷기");
	}
}
```

하지만 이게 과연 좋은 방법일까? 우리는 지금 Java를 이용한 **객체지향 프로그래밍**을 한다는 것을 명심해라. 객체지향 프로그래밍은 **프로그램에서 필요한 데이터들을 추상화시켜 상태와 행위를 가진 객체로 나누고 이 객체들 간의 상호작용을 통해 프로그래밍하는 방법**이라고 하였다.  

동물(Animal)이라는 객체가 과연 현실 세계에 존재할까? 존재하지 않는다. 동물은 우리가 강아지, 고양이, 원숭이 등을 추상화시킨 표현 방법이다. 그렇기 때문에 동물이라는 객체는 생성돼서는 안된다.  

이를 해결하기 위한 방법이 바로 객체를 생성할 수 없는 **추상 클래스**를 사용하는 것이다. 객체 지향 프로그래밍의 특징 중 하나가 바로 **추상화**라고 하였다. **객체들의 불필요한 특징을 제거하며 공통적인 특징(속성과 기능)을 추출하는 것**이다. 공통적인 특징인 동물들의 움직임을 표현하는 move() 메서드를 추상 메서드로 추상화시킨 후 이를 상속받는 클래스에게 재정의시키는 것이다.  

```java
abstract class Animal {
	public abstract void move(); //추상 메서드 : 몸통은 없고 껍데기만 존재
                                 //상속받는 자식 클래스에서 재정의하여 구현
}

class Dog extends Animal {
	@Override
	public void move() {
		System.out.println("네 발로 걷기");
	}
}

class Monkey extends Animal {
	@Override
	public void move() {
		System.out.println("두 발로 걷기");
	}
}
```

추상 클래스를 사용함으로써 객체지향 프로그래밍의 특징을 잘 살려낸 것을 확인할 수 있다. 또한 추상 메서드를 통해 자식 클래스는 반드시 추상 메서드를 재정의하도록 유도하여 오류를 줄일 수 있다.  

이번에는 인터페이스이다. 인터페이스는 추상 클래스의 극단적인 경우라고 생각하면 쉽다. 추상 클래스는 추상 메서드가 없어도 됐으며 멤버 변수나 일반 메서드를 가질 수 있었다. 하지만 인터페이스는 오직 **상수와 추상 메서드**만 가질 수 있다. 인터페이스는 **interface** 키워드를 사용하여 구현한다.  

인터페이스 내부의 모든 변수와 메서드는 컴파일 시 자동으로 상수와 추상 메서드로 간주하고 키워드를 추가해 주기 때문에 **public static final과 public abstract** 키워드를 생략할 수 있다.  

인터페이스 또한 미완성된 메서드가 존재하기 때문에 객체를 생성할 수 없다.  

인터페이스는 **자식 클래스가 부모 클래스의 기능을 반드시 구현하도록 하는 것이 목적**이다. 간단하게 생각하면 설계도와 같다. 설계도를 통해 원하는 기능을 구현하는 것이다. 또한 자식 클래스는 반드시 부모 클래스의 추상 메서드를 모두 구현해야 한다.  

인터페이스는 **다중 상속**이 가능하다. 인터페이스는 인터페이스로부터만 상속받을 수 있으며 **extends** 키워드를 통해 상속받고 인터페이스를 상속받는 일반 클래스는 **implements** 키워드를 통해 상속받는다.  

```java
interface A {}
interface B {}
interface C extends B {} //interface B를 상속
class D {}
class E extends D implements A, C {} //class D와 interface A, C를 상속
```

jdk 1.8부터 **default** 키워드를 사용하여 일반 메서드를 구현할 수 있다. 인터페이스를 상속받는 클래스들의 공통된 기능을 구현함으로써 반복되는 코드의 작성을 줄여준다.  

```java
interface Animal {
	public default void whoAmI() { System.out.println("I'm Animal"); } //default 메서드
	public abstract void move();
}
```

위의 동물 예제를 인터페이스를 이용하여 구현한 코드는 다음과 같다.  

```java
interface Animal {
	public default void whoAmI() {System.out.println("I am Animal");}
	public abstract void move();
}

class Dog implements Animal {
	@Override
	public void move() {
		System.out.println("네발로 걷기");
	}
}

class Monkey implements Animal {
	@Override
	public void move() {
		System.out.println("두발로 걷기");
	}
}
```

추상 클래스와 인터페이스 모두 추상화를 활용한 개념으로 비슷해 보이지만 차이점이 확연히 존재한다는 것을 확인하였다.
