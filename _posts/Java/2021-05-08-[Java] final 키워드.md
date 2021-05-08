---
title:  "[Java] final 키워드"
excerpt: "[Java] final 키워드를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-05-08T18:35:00
---

Java에서는 상수(변경할 수 없는 값)를 표현하기 위해서 final 키워드를 사용한다. final 키워드는 변수, 메서드, 클래스에 사용할 수 있는데 각각 조금씩 의미가 다르다.  

이들의 차이에 대해서 한 번 알아보도록 하자.  
<br>
<br>
<h1>final 변수</h1>  

흔히 mod, maxSize 등 **변하지 않고 정해진 값(상수)을 표현하기 위해 사용하는 방식**으로 변수 앞에 final 키워드를 붙여서 사용한다.  

상수를 표현할 때는 자바 명명 관습(Java Naming Conventions)에 따라서 대문자를 사용하고 두 단어 이상 사용될 시 '_'를 사용하여 표현해 준다.  

```java
public class Main {

	final int MOD = 10009;

	public static void main(String[] args) {
		final int MAX_SIZE = 100;
	}
}
```

만약 final로 선언한 상수를 변경하려고 하면 에러가 발생하게 된다.  

```java
public class Main {

	final int MOD = 10009;

	public static void main(String[] args) {
		final int MAX_SIZE = 100;
        //MAX_SIZE = 10; error
	}
}
```

또한, 메서드 내에 선언한 final 변수는 사용하기 전에만 초기화를 해주면 돼지만, 클래스 멤버 변수로 선언한다면 반드시 선언과 동시에 초기화를 해줘야 한다.  

```java
public class Main {

	//final int MOD; error
	final int MOD = 10009;

	public static void main(String[] args) {

		final int MAX_SIZE;
		MAX_SIZE = 100;
		//do Something with MAX_SIZE
	}
}
```

초기화해주는 방법은 **선언과 동시에 초기화, 초기화 블록, 생성자** 등을 이용해서 초기화할 수 있다.  

```java
public class Main {

	final int MOD = 10009; //1. 선언과 동시에 초기화
	
	{
		MOD = 10009; //2. 초기화 블록
	}
	
	public Main() {
		MOD = 10009; //3. 생성자
	}

	public static void main(String[] args) {
         //do Something	
	}
}
```

static 멤버 변수도 final을 사용하여 프로그램 전체에서 사용할 수 있는 상수로 선언할 수 있다. 이 또한 마찬가지로 멤버 변수로 선언되기 때문에 반드시 초기화를 해줘야 한다.  

```java
public class Main {

	final static int MOD = 10009; //1. 선언과 동시에 초기화

	static {
		MOD = 10009; //2. 초기화 블록
	}

	public static void main(String[] args) {
		//do Something
	}
}
```
<br>
<br>
<br>
<h1>final 메서드</h1>  

final 메서드는 **해당 클래스를 상속받는 자식 클래스에서 이 메서드를 오버라이드(Override)할 수 없도록 해주는 기능**이다.  

다음과 같이 A를 상속받는 B 클래스에서 print() 메서드를 오버라이드하려고 하지만 A의 print() 메서드가 final로 선언되어 있기 때문에 오버라이드할 수 없다.  

```java
class A {
	
	public final void print() {
		System.out.println(getClass());
	}
}

class B extends A {
	
    //error : final 키워드로 인해서 오버라이드로 재정의할 수 없음
	@Override
	public void print() {
		System.out.println(getClass());
	}
}
```
<br>
<br>
<br>
<h1>final 클래스</h1>  

final 클래스는 **어떤 클래스도 final로 선언한 클래스를 상속할 수 없도록 해주는 기능**이다.  

```java
final class A {
	
	public void print() {
		System.out.println(getClass());
	}
}

//error : 상속받을 A 클래스가 final 클래스이기 때문에 상속받을 수 없음
class B extends A {}
```

<br>
즉, final 키워드의 기능을 정리하면 다음과 같다.  

![Java  final 키워드](https://user-images.githubusercontent.com/53072057/117524448-3623d400-aff8-11eb-941d-f1d1265c01ab.JPG)  
