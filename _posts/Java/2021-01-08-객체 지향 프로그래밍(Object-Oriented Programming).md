---
title:  "객체 지향 프로그래밍(Object-Oriented Programming)"
excerpt: "객체 지향 프로그래밍(Object-Oriented Programming)을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-01-08T18:35:00
---

Java를 공부하면서 Java는 객체 지향 언어라고 배웠다. Java는 클래스와 객체를 기반으로 프로그래밍을 하기 때문에 객체 지향 프로그래밍이라고 불리는구나 정도로만 생각했지만 Java를 주언어로 삼은 김에 객체 지향 프로그래밍에 대해서 공부 및 정리해보려고 한다.  

객체 지향 프로그래밍(Object-Oriented Programming)은 **프로그램에서 필요한 데이터들을 추상화시켜 상태와 행위를 가진 객체로 나누고 이 객체들 간의 상호작용을 통해 프로그래밍하는 방법**을 뜻한다.  

객체 지향 프로그래밍을 알아보기 전에 가장 중요한 클래스와 객체에 대해서 이해하고 넘어가자.  

클래스는 **프로그램에서 필요한 데이터를 추상화시켜 상태와 행위를 가진 특정 객체를 생성하기 위해 변수와 메서드를 정의하는 일종의 틀**이다.  

객체는 **클래스에서 정의한 것을 토대로 메모리에 할당된 것으로 프로그램에서 사용되는 데이터**를 뜻한다.  

클래스와 객체를 간단한 예시를 통해 알아보도록 하자. 먼저 실제 세계에서 사람이란 데이터를 추상화 시켜보자. 사람은 보통 이름, 나이, 성별을 가지고 있다. 또한 간단하게 "안녕하세요!"라고 인사하는 기능을 가지고 있다고 하자. 이러한 특징을 가지는 사람을 정의하는 것이 클래스이다.  

```java
class Person{
	
	String name, sex;
	int age;

	public Person(String name, int age, String sex) {
		this.name = name;
		this.age = age;
		this.sex = sex;
	}
	
	public void greeting() {
		System.out.println("안녕하세요!");
	}
}
```

만약 프로그램을 개발하다 보면 사람이라는 데이터를 사용해야 할 필요가 있을 것이다. 사람은 서로 특징이 다르기 때문에 매번 새로운 사람을 생성해야 할 것이다. 클래스라는 틀에서 사람을 생성한 것이 바로 객체이다.  

```java
public class Blog {
	public static void main(String[] args) {
    	Person p1 = new Person("Libi", 26, "남자");
    	p1.greeting();
    	
    	Person p2 = new Person("Libi2", 19, "여자");
    	p2.greeting();
    }
}
```

객체 지향 프로그래밍과 비교되는 대표적인 방법론은 절차 지향 프로그래밍이다. 둘을 간단하게 비교해보자.  

![객체 지향 프로그래밍1](https://user-images.githubusercontent.com/53072057/103962742-01635580-519b-11eb-967e-868f8702920f.JPG)  

다음으로 객체 지향 프로그래밍의 특징을 살펴보자.  

* 추상화  

ㅁ **객체들의 불필요한 특징을 제거하며 공통적인 특징(속성과 기능)을 추출하는 것**을 의미한다.  

ㅁ 앞에서 배운 클래스를 생각하면 이해하기 쉬울 것이다.  

* 캡슐화 + 정보 은닉  

ㅁ **데이터와 기능을 외부로부터 알 수 없도록 클래스라는 하나의 캡슐 형태로 만드는 것**을 의미한다.  

ㅁ 캡슐화와 비슷한 개념으로 정보 은닉이라는 개념이 있다. 하지만 둘은 다른 개념이다. 캡슐화가 관련된 데이터와 기능을 묶어주는 것이라면 정보 은닉은 캡슐 속에 있는 데이터와 기능을 외부에 노출시키지 않고 내부에 숨겨서 보호하는 것을 뜻한다.  

ㅁ 클래스 내 변수의 접근을 private 제한하고 public으로 getter와 setter 메서드를 통해 변수에 접근하도록 하는 것이 정보 은닉의 일종이다.  

```java
class Person{
	
	private String name, sex;
	private int age;
	
	public int getAge() {
		return age;
	}
	
	public void setAge(int age) {
		this.age = age;
	}
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public String getSex() {
		return sex;
	}
	
	public void setSex(String sex) {
		this.sex = sex;
	}
}

public class Blog {
	public static void main(String[] args) {
    	Person p = new Person();
    	p.setName("Libi");
    	p.setAge(26);
    	p.setSex("남자");
    	System.out.println("이름 : " + p.getName() + " 나이 : " + p.getAge() + " 성별 : " + p.getSex());
	}
}
```

ㅁ 다른 클래스에서 데이터나 기능에 접근하는 것을 제한시킴으로써 객체지향의 목표 중 하나인 모듈 간의 결합도는 낮추며 응집도를 높여준다.  

* 상속성

ㅁ **상위 클래스의 데이터와 기능을 하위 클래스가 모두 이어받는 것**을 의미한다.  

ㅁ 상속을 받음으로써 불필요한 코드를 작성할 필요가 없기 때문에 코드의 재사용성이 증가한다.  

ㅁ 상위 클래스의 기능을 일부분 변경해야 할 경우 하위 클래스에서 재정의하여 사용할 수 있다 → 다형성  

ㅁ Java에서는 다중 상속이 허용되지 않기 때문에 상속은 하나만 가능하고 필요에 따라 인터페이스를 이용한다.  

* 다형성  

ㅁ **객체들의 타입이 다르면 똑같은 메시지가 전달되더라도 서로 다른 동작을 하는 것**을 의미한다.  

ㅁ 예를 들어 강아지가 speak라는 메시지를 받는다면 "멍멍"이라고 할 것이고 고양이가 speak라는 메시지를 받는다면 "야옹"이라고 할 것이다.  

ㅁ 이전에 배운 오버로딩(Overloading)과 오버라이딩(Overriding)이 다형성을 활용하는 기법이다.  

조금 심화적인 개념으로 **객체지향 5원칙(SOLID)**이라는 것이 있다. 간단하게 정의만 짚고 넘어가도록 하자.  

* 단일 책임 원칙(Single Responsiblity Principle. SRP)  

ㅁ 모든 클래스와 함수들은 각각 하나의 기능만 가져야 한다는 의미이다.  

* 개방-폐쇄 원칙(Open-Closed Principle, OCP)  

ㅁ 모든 구성요소(클래스, 모듈, 함수)는 확장에는 열려있고, 변경에는 닫혀있어야 한다는 의미이다.  

* 리스코프 치환 원칙(Liskov Substitution Principle, LSP)  

ㅁ 자식 클래스는 부모 클래스에서 가능한 행위를 수행할 수 있어야 한다는 의미이다.  

ㅁ 상속 관계에서의 일반화 관계(IS-A)가 성립해야 한다는 뜻이다.  

* 인터페이스 분리 원칙(Interface Segregation Principle, ISP)  

ㅁ 자신이 사용하지 않는 인터페이스는 구현하지 말아야 한다는 의미이다.  

* 의존 역전 원칙(Dependency Inversion Priciple, DIP)  

ㅁ 클래스 사이에 의존 관계는 존재하더라도, 구체적인 클래스에 의존하지 말고 최대한 추상 클래스나 인터페이스와 관계를 맺어라는 의미이다.  
