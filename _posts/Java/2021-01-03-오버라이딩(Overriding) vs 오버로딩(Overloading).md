---
title:  "오버라이딩(Overriding) vs 오버로딩(Overloading)"
excerpt: "오버라이딩(Overriding) vs 오버로딩(Overloading)을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-01-03T18:35:00
---

Java를 배웠다면 위의 두 용어를 한 번쯤은 들어봤을 것이다. 두 용어 모두 발음도 비슷하고 개념도 비슷해 보이지만 전혀 다른 개념이다. 당연한 소리지만 다른 개념이니 구분하여 사용하고 있지 않을까?  

솔직히 나도 오버라이딩과 오버로딩이 헷갈리기 때문에 이번 기회에 한번 개념을 공부해보고 정리해 보려고 한다.  

먼저 오버라이딩(Overriding)이다. 오버라이딩은 **상속 관계에 있는 클래스 간에 같은 이름을 재정의하는 기술**이다. 자바의 경우 오버라이딩 시 동적 바인딩 된다. 그렇다면 한번 코드를 통해 이해해 보자. 다음과 같이 이름과 나이를 가지는 Person 클래스와 학교를 가지며 Person 클래스를 상속받는 Student 클래스가 있다고 하자.   

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

public class Blog {
    public static void main(String[] args) {
 
    	Student s = new Student("Libi", 26, "ABC");
    	s.print();
        //실행 결과 : Student [name=Libi, age=26, school=ABC]
        //만약 Student 클래스에서 print() 메서드를 재정의 하지 않았다면 Person의 print() 메서드를 호출
        //실행 결과 : Person [name=Libi, age=26]

    }
}
```

Student 클래스는 부모 클래스인 Person 클래스의 print() 메서드를 새롭게 재정의하여 사용하고 있다. 이것이 바로 오버라이딩(Overriding)이다. 부모 클래스의 print()를 사용할 수도 있지만 자식 클래스의 입맛에 맞도록 새롭게 print() 메서드를 구현하여 객체지향 언어의 특징인 **다형성**을 잘 살려내고 있는 것을 확인할 수 있다.  

다형성이란 **객체들의 타입이 다르면 똑같은 메시지가 전달되더라도 서로 다른 동작을 하는 것**을 말한다. 예를 들어 강아지가 speak라는 메시지를 받는다면 "멍멍"이라고 할 것이고 고양이가 speak라는 메시지를 받는다면 "야옹"이라고 할 것이다.  

오버라이딩을 사용하기 위한 조건은 다음과 같다.  

1. 자식 클래스에서 오버라이드 하고자 하는 메서드는 반드시 부모 클래스에 존재해야 한다.

2. 오버라이드 하고자 하는 메서드와 메서드의 이름, 매개변수 개수, 매개변수의 자료형, 리턴형이 동일해야 한다.

3. 오버라이드 하고자 하는 부모 클래스의 메서드와 동일하거나 내용이 추가되어야 한다.  

이번에는 오버로딩(Overloading)이다. 오버로딩은 **같은 이름의 메서드를 매개변수의 타입과 개수를 다르게 정의하여 다양한 메서드를 구현하는 것**을 의미한다. 자바의 경우 오버로딩 시 정적 바인딩 된다. 오버로딩 또한 코드를 통해 알아보자.  

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
	
	public void print(String name, int age, String school) {
		System.out.println("Student [name=" + name + ", age=" + age + ", school=" + school + "]");
	}
}

public class Blog {
    public static void main(String[] args) {
 
    	Student s = new Student();
    	s.print("Libi");
    	s.print("Libi", 26);
    	s.print("Libi", 26, "ABC");
        //실행 결과
        //Student [name=Libi]
        //Student [name=Libi, age=26]
        //Student [name=Libi, age=26, school=ABC]
    }
}
```

Student 클래스에서 매개변수의 개수에 따라 3가지의 print() 메서드를 구현하였다. 이것이 바로 오버로딩(Overloading)이다. 같은 메서드지만 매개변수에 따라 다른 실행 결과를 얻도록 정의하여 프로그램의 가독성을 증가시키는 장점이 있다.  

오버로딩을 사용하기 위한 조건은 다음과 같다.  

1. 메서드 이름이 같아야 하며 반환형은 같아도 되고 달라도 된다.

2. 매개변수의 개수가 달라야 한다. 만약 개수가 같다면 매개변수 타입이 달라야 한다.  