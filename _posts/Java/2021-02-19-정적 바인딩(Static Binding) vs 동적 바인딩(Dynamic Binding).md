---
title:  "정적 바인딩(Static Binding) vs 동적 바인딩(Dynamic Binding)"
excerpt: "정적 바인딩(Static Binding) vs 동적 바인딩(Dynamic Binding) 차이를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-02-19T18:35:00
---

이전 오버로딩(Overloading)과 오버라이딩(Overriding)을 공부할 때 오버로딩은 정적 바인딩 되고 오버라이딩은 동적 바인딩 된다고 하였다. 그렇다면 여기서 말하는 바인딩은 무엇일까? 이에 대해서 한번 공부 및 정리해 보려고 한다.  

**바인딩(Binding)이란 프로그램에 사용된 구성 요소의 실제 값 또는 프로퍼티를 결정짓는 행위**를 의미한다. 즉, 프로그램에서 사용되는 변수나 메서드 등 모든 것들이 결정되도록 연결해주는 것을 뜻한다.  

이러한 바인딩은 바인딩이 되는 시점에 따라 정적 바인딩과 동적 바인딩으로 구분할 수 있다.  

![정적 바인딩(Static Binding) vs 동적 바인딩(Dynamic Binding)1](https://user-images.githubusercontent.com/53072057/108449724-4a80ec00-72a7-11eb-8ac7-06e39a48c8e9.JPG)  

그렇다면 왜 오버로딩은 정적 바인딩이며 오버라이딩은 동적 바인딩일까?  

오버로딩은 **같은 이름의 메서드를 매개변수의 타입과 개수를 다르게 정의하여 다양한 메서드를 구현하는 것**을 의미한다.  

```java
class Student{
	String name, school;
	int age;

	//매개변수에 따라 다른 3개의 print() 메서드를 구현
	public void print(String name) {
		System.out.println("Student [name=" + name + "]");
	}
	
	public void print(String name, int age) {
		System.out.println("Student [name=" + name + ", age=" + age + "]");
	}
}
```

Student 객체를 생성하여 print() 메서드를 호출한다고 해보자.  

```java
Student student = new Student();
student.print("Libi");
student.print("Libi", 26);
```

컴파일러가 컴파일 과정에서 이를 구분할 수 없을까? 2개의 print() 메서드는 입력으로 받는 매개변수 종류와 개수가 다르기 때문에 컴파일 과정에서 구분할 수 있다.  

즉, **컴파일 과정에서 어떤 메서드를 호출할지 결정되기 때문에 오버로딩은 정적 바인딩**이다.  

이와 반면에 오버라이딩을 살펴보자. 오버라이딩은 **상속 관계에 있는 클래스 간에 같은 이름을 재정의하는 기술**을 의미한다.  

```java
class Person {
	String name;
	int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public void print() {
		System.out.println("Person [name=" + name + ", age=" + age + "]");
	}
}

class Student extends Person {
	String school;
	
	public Student(String name, int age, String school) {
		super(name, age);
		this.school = school;
	}

    //부모 클래스의 print() 메서드를 새롭게 재정의하여 사용 - 오버라이딩(Overriding)
	public void print() {
		System.out.println("Student [name=" + name + ", age=" + age + ", school=" + school + "]");
	}
}
```

이번에는 Student가 아닌 Person 객체를 하나 생성하여 print() 메서드를 호출한다고 해보자.  

```java
Person student = new Student("Libi", 26, "ABC");
student.print();
```

과연 컴파일 과정에서 Person 객체의 print() 메서드가 Person 클래스의 print() 메서드인지 Student 클래스의 print() 메서드인지 판단할 수 있을까?  

이는 불가능하다. 왜냐하면 Person 객체가 Student 객체가 될 수도 있기 때문에 타입을 확정 지을 수 없기 때문이다. 단지 컴파일러는 student 객체의 타입 오류를 확인할 뿐이다. 실행 시간 때 객체의 타입이 확정되면서 Person의 print() 메서드를 호출하는 것이다.   
​
즉, **실행 시간에 어떤 메서드를 호출할지가 정해지기 때문에 오버라이딩은 동적 바인딩**이다.  

공부하다 보니 한 가지 궁금한 점이 생겼다. 자식 클래스가 부모 클래스의 메서드를 오버라이딩한 경우는 자식 클래스의 객체는 부모 클래스의 메서드에 접근할 수 없는가?이다. 이는 **super** 키워드를 사용하면 정적 바인딩을 통해 부모 클래스의 메서드를 접근할 수 있다.  

```java
public void print() {
	super.print(); //정적 바인딩을 통해 부모의 print() 메서드를 접근
	System.out.println("Student [name=" + name + ", age=" + age + ", school=" + school + "]");
}
```

super 키워드를 사용하면 메서드뿐만 아니라 부모 클래스의 멤버에 접근이 가능해진다.  
​
동적 바인딩이 아무래도 실행 시간에 바인딩 되다 보니 정적 바인딩보다는 성능상 안 좋을 수도 있다. 하지만 동적 바인딩을 통해 상속과 다형성 등 다양한 기능을 사용할 수 있는 장점이 존재하기 때문에 사용하는 것 같다.  

마지막으로 특정 메서드를 오버라이딩이 불가능하게 만드는 방법은 무엇일까? 메서드에 **private, final, static 키워드**를 붙여주면 된다. 이 키워드를 붙인 메서드는 정적 바인딩으로 컴파일 시점에 결정된다.  
