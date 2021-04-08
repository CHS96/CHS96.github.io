---
title:  "[Java] 싱글톤 패턴(Singleton Pattern) 구현 방법"
excerpt: "[Java] 싱글톤 패턴(Singleton Pattern) 구현 방법을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-04-08T18:35:00
---

싱글톤 패턴은 생성(Creational) 패턴 중 하나로써 객체를 오직 하나만 생성하여 생성된 객체를 프로그램 어디에서나 접근하여 사용할 수 있도록 하는 패턴이다.  

간단히 말해서 애플리케이션 전체에서 단 하나의 객체만 생성하고 필요할 때마다 이 객체에 접근하여 사용하겠다는 의미이다.  

이번 글은 싱글톤 패턴을 구현하는 방법을 중점으로 정리할 것이기 때문에 싱글톤 패턴에 대해 좀 더 자세히 알고 싶다면 다음 글을 참고하길 바란다.  

[![싱글톤 패턴](https://user-images.githubusercontent.com/53072057/113970177-60f00080-9871-11eb-93b2-7c1f4810491e.JPG)](https://blog.naver.com/djena5078/222218160211)  

![초기화블록](https://user-images.githubusercontent.com/53072057/113646571-08c7cb80-96c4-11eb-888f-d62f6683e6be.JPG)  

싱글톤 패턴을 구현하는 방법은 크게 6가지가 존재한다. 모든 것을 배제하고 단순하게 구현하는 방법부터 동기화 문제나 자원의 효율성 등을 고려하여 구현하는 방법까지 단계별로 존재한다.  

그렇다면 단계별로 싱글톤 패턴을 구현하는 방법에 대해서 알아보자.  



<h2>1. Eager Initialization</h2>  

첫 번째 방법은 Eager Initialization으로 싱글톤 패턴을 구현하는 가장 간단한 방법이다. **static을 통해 해당 클래스를 Class Loader가 로딩할 때 객체를 생성해 준다**.  

```java
class Singleton {
	//static을 통해 class가 로드될때 객체를 생성 
	private static Singleton singleton = new Singleton(); 
	
	private Singleton() {} //생성자에 접근 x
	
	public static Singleton getInstance() {
		return singleton;
	}
}
```

하지만 이 방법은 객체를 사용하지 않더라도 객체가 무조건 생성되기 때문에 **자원 낭비**가 될 수 있는 단점이 존재한다. 또한 **Exception에 대한 처리**를 하지 않는다.  



<h2>2. Static Block Initialization</h2>  

두 번째 방법은 Static Block Initialization으로 Eager Initialization 방법과 비슷하지만 **Static Block을 사용하여 Exception 처리를 해주는 방법**이다.  

```java
class Singleton {
	private static Singleton singleton; 
	
	private Singleton() {} //생성자에 접근 x
	
	//static block을 통해 클래스가 처음 로딩 될때 객체를 생성
	static {
		try {
			singleton = new Singleton();
		} catch (Exception e) {
			throw new RuntimeException("Exception occured in creating singleton instance");
		}
	}
	
	public static Singleton getInstance() {
		return singleton;
	}
}
```

Static Bolck은 **초기화 블록(Initialization Block)**이라고 불리며 클래스가 처음 로딩될 때 한 번만 수행되는 블록을 의미한다. 잘 사용하지는 않지만 클래스 변수의 복잡한 초기화에 사용된다. 비슷한 개념으로 **Instance Block**이 존재한다.  

```java
static  { //초기화할 내용 } //Static Block : 클래스가 로딩될때 한번만 수행
{ //초기화할 내용 } //Instance Block : 인스턴스가 생성될때마다 수행
```

하지만 이 방법도 클래스 로딩 단계에서 객체를 생성하기 때문에 자원의 비효율성을 해결할 수 없다.  



<h2>3. Lazy Initialization</h2>  

세 번째 방법은 **static으로 선언된 getInstance() 메서드를 통해 객체를 생성해 주는 방법**이다.  

```java
class Singleton {
	private static Singleton singleton; 
	
	private Singleton() {} //생성자에 접근 x
	
	//객체가 존재하지 않으면 생성해주고 존재하면 기존 객체를 반환
	public static Singleton getInstance() {
		if (singleton == null) singleton = new Singleton();
		return singleton;
	}
}
```

getInstance() 메서드를 호출하여 객체가 존재하지 않으면 새로운 객체를 하나 생성해 주고, 존재하면 기존 객체를 반환해 준다.  

이 방법은 1, 2단계의 문제점인 자원의 비효율성을 해결해 줄 수 있다. 하지만 다른 문제가 존재한다.  

싱글톤 패턴은 한 객체를 여러 곳에서 접근할 수 있기 때문에 **동기화 문제**가 발생한다. 만약 한 번에 여러 곳에서 getInstance() 메서드를 호출한다면 여러 개의 객체가 생성될 수 있기 때문이다.  

즉, 이 방법은 **single-thread 환경에서는 괜찮은 방법이지만 multi-thread 환경에서는 동기화 문제가 발생할 수 있다**.  



<h2>4. Thread Safe Singleton</h2>  

네 번째 방법은 세 번째 방법의 동기화 문제를 해결하기 위한 방법으로 Java에서 동기화를 해결하기 위한 키워드인 **synchronized를 걸어주는 방법**이다.  

```java
class Singleton {
	private static Singleton singleton; 
	
	private Singleton() {} //생성자에 접근 x
	
	//객체가 존재하지 않으면 생성해주고 존재하면 기존 객체를 반환
	public static synchronized Singleton getInstance() {
		if (singleton == null) singleton = new Singleton();
		return singleton;
	}
}
```

synchronized 키워드를 사용하면 **어떤 한순간에는 하나의 스레드 만이 임계 영역(Critical Section) 안에서 실행하는 것이 보장된다**.

따라서 이 방법은 multi-thread 환경에서도 안전하게 동작하는 것을 보장해 준다.   

하지만 이 방법도 문제가 존재한다. synchronized를 사용하는 비용은 저렴한 편은 아니다. 우리는 해당 객체를 안전하게 생성하기 위해  synchronized를 사용하는 것인데 이 방법은 해당 객체를 생성한 후 접근할 때에도 계속해서  synchronized를 호출하게 된다.  

즉, 싱글톤 객체를 자주 사용해야 한다면 synchronized가 자주 호출되면서 많은 비용이 발생하게 되고 이에 따른 성능 저하가 발생하게 된다.  

**Double Checked Locking 방식**을 사용하면 이를 해결할 수 있다. 메서드에 synchronized를 붙이지 말고 메서드 내부에 synchronized를 사용하여 이름 그대로 두 번의 검사를 통해 싱글톤 객체를 생성 및 반환하는 방법이다.  

```java
class Singleton {
	private static Singleton singleton; 

	private Singleton() {} //생성자에 접근 x

	//객체가 존재하지 않으면 생성해주고 존재하면 기존 객체를 반환
	public static Singleton getInstance(){
	    if(singleton == null){
            //synchronized(인스턴스 변수 혹은 클래스 타입)
            //특정 영역만 동기화, 메서드 영역보다 범위가 작음
	        synchronized (Singleton.class) {
	            if(singleton == null) singleton = new Singleton();
	        }
	    }
	    return singleton;
	}
}
```

객체가 null 일 경우에만 synchronized가 실행되도록 하여 객체가 생성된 후에는 synchronized가 실행되지 않는다. 즉, 무분별한 synchronized 호출의 비용을 절약할 수 있다.  



<h2>5. Bill Pugh Singleton Implementation</h2>  
​
다섯 번째 방법은 **Inner Static Helper Class를 사용하는 방식**으로 현재 가장 널리 사용되고 있는 싱글톤 패턴 구현 방법이다.  

```java
class Singleton {

	private Singleton() {} //생성자에 접근 x

	private static class SingletonHelper {
		private static final Singleton SINGLETON = new Singleton();
	}
	
	public static Singleton getInstance(){
	    return SingletonHelper.SINGLETON;
	}
}
```

SingletonHelper 클래스는 Inner Class로 선언되었기 때문에 Singleton 클래스가 Class Loader에 의해 로딩될 때 로딩되지 않다가 getInstance()가 호출될 때 JVM 메모리에 로드되고 객체를 생성하게 된다.  

또한 클래스가 로드될 때 객체가 생성되기 때문에 multi-thread 환경에서도 안전하게 사용이 가능하다.  

지금까지의 문제점을 모두 해결해 줄 수 있으며 Double Checked Locking 방식보다 구현도 간단하기 때문에 굉장히 좋은 방법이다.  

하지만 이 방법도 Java의 **Reflection**을 사용하면 private 생성자, 메서드에 접근이 가능해지며 단 하나의 객체라는 조건을 깨뜨려버린다. (Reflection에 대해선 다음에 알아보도록 하자.)  

다음의 코드는 Bill Pugh Singleton Implementation의 코드에 Reflection을 사용한 코드이다.  

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

class Singleton {

	private Singleton() {} //생성자에 접근 x

	private static class SingletonHelper {
		private static final Singleton SINGLETON = new Singleton();
	}

	public static Singleton getInstance(){
		return SingletonHelper.SINGLETON;
	}
}

public class Main {

	public static void main(String args[]) throws ClassNotFoundException, NoSuchMethodException, SecurityException, InstantiationException, IllegalAccessException, IllegalArgumentException, InvocationTargetException{
		Singleton singleton1 = Singleton.getInstance();
		Singleton singleton2 = Singleton.getInstance();

		System.out.println("singleton1 : " + singleton1);
		System.out.println("singleton2 : " + singleton2);

		//Breaking down singletons using reflection
		Class<?> speakerClass = Class.forName("Singleton");
		Constructor<?> constructor = speakerClass.getDeclaredConstructor();
		
		constructor.setAccessible(true); //어떤 접근 제한자든지 접근 가능
		Singleton singleton3 = (Singleton) constructor.newInstance();
		System.out.println("singleton3 : " + singleton3);
	}
}
```

![싱글톤 패턴1](https://user-images.githubusercontent.com/53072057/113970179-62212d80-9871-11eb-96d5-b4a1b00ae24d.JPG)  

실행 결과를 보면 싱글톤 객체임에도 불구하고 주소값이 다른 새로운 객체가 생성된 것을 확인할 수 있다.  



<h2>6. Enum Singleton</h2>  

마지막 방법으로 **Enum을 사용하여 싱글톤 패턴을 구현하는 방법**이다.  

```java
enum EnumSingleton {
	INSTANCE;

	public static void doSomething(){
        //do something
    }
}
```

Enum Singleton 방법은 구현하기가 매우 간단하며 동기화, Reflection의 문제점도 해결해 줄 수 있다.  

하지만 Eager Initialization, Static Block Initialization 방식처럼 **Lazy Loading**이 아니기 때문에 자원의 비효율성을 해결해 주지 못하는 단점도 존재한다.  



<h2>[ Reference ]</h2>  
* <https://readystory.tistory.com/116>  
* <https://it-mesung.tistory.com/157?category=847780>  

