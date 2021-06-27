---
title:  "프록시 패턴(Proxy Pattern)"
excerpt: "프록시 패턴(Proxy Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-06-27T18:35:00
---

프록시 패턴(Proxy Pattern)은 **구조(Structural) 패턴 중 하나로써 접근이 어려운 객체와 여기에 연결하려는 객체 사이에서 인터페이스 역할을 수행하는 패턴**이다.  
​
프록시는 대리인을 의미한다. 어떤 업무가 발생하였을 경우 그 업무가 사장이 직접 처리해도 되지 않을 정도의 업무라면 굳이 사장에게 보고하지 않고 대리인을 통해 처리할 것이다. 프록시 패턴 역시 이러한 원리이다.  

중요한 것은 프록시는 대리인의 역할로 요청을 처리해 줘야 한다. 즉, 사장과 고객을 연결해 주는 인터페이스 역할을 수행할 뿐 자신이 고객의 요청에 대해 관여해서는 안 된다.   

주로 네트워크 연결, 메모리의 대용량 객체로의 접근 등에 이용한다.  

프록시 패턴의 구조는 다음과 같다.  
![프록시](https://user-images.githubusercontent.com/53072057/123533544-572fb800-d751-11eb-8f64-f829bb3f774c.JPG)  

* Subject : Proxy와 RealSubject가 구현해야 하는 인터페이스, Client의 요청을 나눠서 처리하기 위함  

* Proxy : 간단한 요청을 처리해 주는 객체, 요청이 복잡할 경우 내부의 RealSubject 객체에게 요청을 위임  

* RealSubject : 복잡한 요청을 처리해 주는 객체, Client는 Proxy를 통해 접근  

*****

간단한 작업과 복잡한 작업을 처리해 주는 예제를 통해 알아보도록 하자.  

먼저 **Subject** 인터페이스이다. 간단, 복잡한 요청을 처리해 주는 메서드를 가진다.  

```java
public interface Subject {
	
	//간단한 업무
	public void doAction1();

	//복잡한 업무
	public void doAction2();
}
​
```

다음으로 **Proxy** 클래스이다. 간단한 작업은 직접 처리해 주며, 복잡한 작업은 RealSubject 객체에게 위임해서 처리해 주도록 한다.  

```java
public class Proxy implements Subject {

	private Subject real;
	
    //생성자 주입을 통해 Proxy와 RealSubject를 연결
	public Proxy(Subject real) {
		this.real = real;
	}

	@Override
	public void doAction1() {
		System.out.println("간단한 업무 처리 by Proxy");
	}

	@Override
	public void doAction2() {
		real.doAction2(); //복잡한 요청은 RealSubject에게 위임
	}
}
```

마지막으로 **RealSubject** 클래스이다. 복잡한 업무를 처리해 주도록 한다.  

```java
public class RealSubject implements Subject {

	@Override
	public void doAction1() {
		System.out.println("간단한 업무 처리 by Real");
	}

	@Override
	public void doAction2() {
		System.out.println("복잡한 업무 처리 by Real");
	}
}
```
​
**Client** 클래스에서 Proxy 객체를 생성하여 간단, 복잡한 업무를 처리해 준다. Client 입장에서는 두 업무 모두 Proxy를 통해 처리해 주기 때문에 직접 RealSubject 객체에게 접근할 수 없도록 해준다.  

```java
public class Client {

	public static void main(String[] args) {
		Subject proxy = new Proxy(new RealSubject());
		proxy.doAction1();
		proxy.doAction2();
	}
}
```

![프록시1](https://user-images.githubusercontent.com/53072057/123533545-5860e500-d751-11eb-8248-c4d3242efd27.JPG)  
