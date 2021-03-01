---
title:  "[Java] String 객체 선언 방법 & String Constant Pool"
excerpt: "[Java] String 객체 선언 방법 & String Constant Pool를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-03-01T18:35:00
---

Java에서 문자열을 나타내는 String 객체를 선언하는 방법은 **new 연산을 통해 객체를 생성하는 방법**과 **""를 이용해서 생성하는 리터럴 방법**이 존재한다.  

```java
String a = new String("Hello"); //new 연산
String b = "Hello"; //리터럴 방식
```

그렇다면 이 둘의 차이는 무엇일까? 이에 대해 알아보기 전에 간단한 예제를 한번 보도록 하자.  

Java에서 객체 간의 '==' 연산은 두 객체의 주소가 같은지를 비교한다고 하였다. 다음과 같이 4개의 String 객체를 선언하여 각각 주소를 비교해보자. 분명히 a, b, c, d는 다른 객체니 주소가 달라야 할 것이다.  

```java
public class Main {
	public static void main(String[] args) {
		String a = new String("Hello");
		String b = new String("Hello");
		String c = "Hello";
		String d = "Hello";
		
		System.out.println("a == b : " + (a == b));
		System.out.println("b == c : " + (b == c));
		System.out.println("c == d : " + (c == d));
	}
}
```

하지만 실행 결과는 다음과 같다.  

![String1](https://user-images.githubusercontent.com/53072057/109451383-a65e2880-7a90-11eb-9237-688a36c507f3.JPG)  

new 연산을 통해 생성된 String 객체는 모두 다른 주소를 가리키지만 리터럴 방식으로 생성된 c와 d는 같은 주소를 가리키는 것을 확인할 수 있다.  

분명히 다른 객체인데 리터럴 방식으로 생성된 c와 d는 같은 주소를 가리키는 것일까? 이는 객체를 생성 시 **메모리에 어떻게 할당되는지의 차이**이다.  

new 연산을 통해 객체를 생성하면 Heap 영역에 객체가 선언된다. 즉, a와 b는 Heap 영역에 각각 할당된다.  

![String2](https://user-images.githubusercontent.com/53072057/109451386-a78f5580-7a90-11eb-8748-b9c38093fae1.JPG)  

반대로 리터럴 방식은 Heap 영역의 **String Constant Pool이라는 영역에 할당**된다.  

String Constant Pool 이란 **Heap 영역에서 String 객체를 별도로 관리하는 장소**이다.  

리터럴 방식으로 String 객체를 생성하면 String 클래스의 intern() 메서드를 통해 String Constant Pool 영역에 객체를 생성한다.  

![String3](https://user-images.githubusercontent.com/53072057/109451387-a827ec00-7a90-11eb-8155-9812102c6113.JPG)  

대충 해석해보면 **해당 객체가 String Constant Pool에 존재하면 해당 객체의 주소를 반환하고 존재하지 않는다면 해당 객체를 저장한다는 의미**이다.  

따라서 c 객체를 생성하면 JVM에서 String Constant Pool을 확인하여 해당 문자열을 가진 객체가 존재하지 않기 때문에 c 객체를 저장한다.  

하지만 d 객체를 생성하면 String Constant Pool에 "Hello" 문자열을 가지는 c 객체가 존재하기 때문에 새롭게 객체를 생성하지 않고 c 객체의 주소를 반환받기 때문에 c와 d 객체는 같은 주소를 가리키는 것이다.  

![String4](https://user-images.githubusercontent.com/53072057/109451389-a8c08280-7a90-11eb-831e-942ed5125061.JPG)  

또한, intern() 메서드를 통해 new 연산으로 생성된 객체를 String Constant Pool에 저장시켜 같은 주소를 가리키도록 할 수 있다.  

```java
public class Main {
	public static void main(String[] args) {
		String a = new String("Hello");
		String b = "Hello";
		System.out.println("a == b : " + (a == b));
		a = a.intern();
		System.out.println("a == b : " + (a == b));
	}
}
```

![String5](https://user-images.githubusercontent.com/53072057/109451390-a8c08280-7a90-11eb-8b24-8c208a6ca744.JPG)  

new 연산으로 생성된 a 객체를 intern() 메서드를 통해 String Constant Pool 영역으로 이동시켰기 때문에 a와 b는 같은 주소를 가리키게 된다.  

![String6](https://user-images.githubusercontent.com/53072057/109451391-a9591900-7a90-11eb-9079-abcce1610dae.JPG)  

리터럴 방식으로 String 객체를 생성하면 매번 Heap 영역에 새로운 객체를 할당하지 않아도 되기 때문에 메모리를 절약할 수 있는 장점이 존재한다.   

이러한 리터럴 방식이 가능한 이유는 String의 특징인 **불변(immutable)** 때문이다.  

String 객체는 불변하기 때문에 멀티 스레드 환경에서 같은 문자열을 사용하기 위해 동일한 객체를 참조하여도 안전함이 보장된다.  

또한, Heap 영역에 존재하기 때문에 **Garbage Collection의 대상이 될 수 있는 장점**도 존재한다.  

원래 String Constant Pool은 Perm 영역에 존재하였는데 Java 7부터 Heap 영역으로 옮겨졌다.  

Perm 영역은 실행 시간(Runtime)에 가변적으로 변경할 수 없는 고정된 사이즈이기 때문에 intern 메서드의 호출은 저장할 공간이 부족하게 만들 수 있었다. 즉, Out Of Memory가 발생할 수 있는 위험이 존재한다.  

하지만 Heap 영역으로 옮겨지면서 GC 대상이 될 수 있기 때문에 메모리를 관리할 수 있게 되었다.  


<h2>[ Reference ]</h2>  
* <https://madplay.github.io/post/java-string-literal-vs-string-object>  