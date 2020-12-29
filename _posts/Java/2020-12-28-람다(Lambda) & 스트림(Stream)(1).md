---
title:  "람다(Lambda) & 스트림(Stream)(1)"
excerpt: "람다 표현식(Lambda Expression)을 알아보자!"

categories:
  - Java
  
last_modified_at: 2020-12-28T18:35:00
---

이번에 배워볼 내용은 JDK 8의 가장 큰 특징인 **람다(Lambda)와 스트림(Stream)**이다. 프로그래머스에서 다른 사람들의 풀이를 보면 가끔씩 보지 못한 이상한 문법들로 문제를 풀어낸 것을 볼 수 있다. 매번 저런 것도 있구나 하고 넘어갔었지만 이번 기회에 간단하게 한번 짚고 넘어가 보려 한다.  

람다 표현식(Lambda Expression)을 익명 클래스에서 발전된 개념으로 익명 클래스에 대해서 먼저 알아보자.  

익명 클래스는 **단 하나의 객체만을 생성할 수 있는 클래스**를 뜻한다. 자바에서 인터페이스를 구현하기 위해서는 인터페이스를 구현하는 클래스를 구현해 줘야 한다. 만약 인터페이스의 객체를 한 번만 사용할 것인데 이를 위해 클래스를 구현해야 한다면 비효율적일 것이다. 이를 개선하기 위해 굳이 클래스를 구현하지 않고 객체를 사용할 수 있도록 해주는 것이 익명 클래스이다.  

```java
interface Test {
	public void test();
}

class TestClass implements Test {
	@Override
	public void test() {
		System.out.println("인터페이스를 구현한 클래스를 선언하여 Test객체 선언 및 사용");
	}
}

public class Blog {
		
	public static void main(String[] args) {
		//Test 인터페이스를 구현한 TestClass 클래스를 구현해야함
		TestClass tc = new TestClass();
		tc.test();

		//클래스를 구현하지 않고 익명 클래스를 활용 
		Test test = new Test() {
			public void test() {
				System.out.println("익명 클래스를 통해 클래스 구현없이 Test객체 선언 및 사용");
			}
		};
		test.test();
	}
}
```

익명 클래스를 통해 클래스를 따로 선언할 필요가 없어지면서 상당히 편리해졌다. 하지만 익명 클래스를 구현한 부분을 더 간단하게 개선할 수 있지 않을까라는 생각으로 나온 것이 바로 람다식이다.  


람다식은 메서드를 하나의 식으로 표현하여 객체를 통해서 접근할 필요 없이 변수처럼 사용 가능하도록 하는 것이다. 메서드의 이름과 반환값이 없어지기 때문에 **익명 함수(Anonymous function)**라고도 불린다. 단, 람다식이 구현할 인터페이스는 **1개의 추상 메서드**만 가지고 있어야 한다.  

람다 표현식은 형태에 따라 생략이 가능하기 때문에 다양한 형태들을 가진다.  

1. ("매개변수 목록") -> {"함수 몸체"} //기본 형태, 매개변수 자료형 생략 O  
2. "매개변수 목록" -> {"함수 몸체"} //매개변수가 하나일 경우 괄호() 생략 O  
3. ("매개변수 목록") -> "함수 몸체" //함수 몸체가 단일 실행문이면 괄호{} 생략 O  
4. ("매개변수 목록") -> {0;} //함수 몸체가 단일 실행문이며, 그 실행문이 return문일 경우 괄호{}, return 생략 O  

Say 인터페이스의 something 메서드를 사용하기 위해선 익명 클래스를 선언하여 메서드를 이용해야 했지만 람다식을 사용하면 아주 간단하게 something 메서드를 재정의하여 사용할 수 있다.  

```java
interface Say {
	void something(int a);
}

class Person {
	public void greeting(Say line) {
		line.something(3);
	}
}

public class Blog {
	
	public static void main(String[] args) {
		
		Person per = new Person();
		per.greeting(new Say() {
			@Override
			public void something(int a) {
				System.out.println("Hello!");
				System.out.println("Nice to meet you!");
				System.out.println(a);
			}
		});
		System.out.println("------------------");
		per.greeting((int a) -> {
			System.out.println("Hello!");
			System.out.println("Nice to meet you!");
			System.out.println(a);
		});
	}
}
```

람다식을 통해 인터페이스의 메서드를 간단하게 구현할 수 있게 되었다. 하지만 람다식이 편하다고 해서 무조건 좋은 점만 잇는 것은 아니다. 람다식의 장단점은 다음과 같다.  

![람다1](https://user-images.githubusercontent.com/53072057/103190643-30650600-4915-11eb-835d-c2f4b54f347f.JPG)  

사실 람다를 배우는 이유는 **스트림(Stream)**을 위해서라고 생각한다. 솔직히 람다, 스트림을 모른다 하여도 개발에 아무런 지장이 없다. 왜냐하면 지금까지 나도 잘 모르고 있었고 사용해본 적이 없기 때문이다. 하지만 스트림을 적절히 사용한다면 굉장히 이점이 많기 때문에 한번 알아보는 것도 나쁘진 않을 것 같다. 다음 글에서 스트림에 대해 알아보도록 하자.  
