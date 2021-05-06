---
title:  "메멘토 패턴(Memento Pattern)"
excerpt: "메멘토 패턴(Memento Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-05-06T18:35:00
---

메멘토 패턴(Memento Pattern)은 **행위(Behavioral) 패턴 중 하나로써 특정 시점에서의 객체 내부 상태를 객체화함으로써 이후 요청에 따라 객체를 해당 시점의 상태로 돌릴 수 있는 기능을 제공하는 패턴**이다.  

Ctrl + Z와 같은 되돌리기 기능을 생각하면 이해하기 쉽다. 어떤 동작을 수행하다가 이전 상태로 돌아가야 할 경우 Ctrl + Z를 눌려서 이전 상태로 복구하는 작업을 해주도록 구현하는 것이 메멘토 패턴이다.  

메멘토 패턴의 구조는 다음과 같다.  

![메멘토 패턴](https://user-images.githubusercontent.com/53072057/117246852-47de6d80-ae78-11eb-8f6b-2198a4786af1.JPG)  

* Originator : 현재 상태를 표현하는 State를 가지고 있으며, Memento 객체를 통해 현재 상태를 변경  

* Memento : Originator의 상태를 나타내는 클래스, Memento를 이용하여 Originator의 상태를 변경  

* CareTaker : Memento를 순서대로 저장하여 관리하는 클래스, Stack이나 List 같은 자료구조를 사용해도 무방  

*****

간단한 예시로 문자열을 입력하다가 이전 상태로 복구하려고 할 때 Ctrl+Z 기능처럼 이전 문자열 상태로 돌아가는 상황을 메멘토 패턴을 적용하여 구현해보자.  

먼저 **Originator**이다. 문자열을 표현할 것이기 때문에 String 타입의 state 변수를 하나 가지며 Memento 객체를 만들어주거나 복구해 주는 메서드를 가진다.  

```java
public class Originator {
	private String state;
	
	public Memento createMemento() {
		return new Memento(state);
	}
	
	public void restoreMemento(Memento memento) {
		this.state = memento.getState();
	}
	
	public String getState() {
		return this.state;
	}
	
	public void setState(String state) {
		this.state = state;
	}
}
```

다음으로 **Memento** 클래스이다. 특정 시점의 상태를 표현하는 객체를 생성해 준다.  

```java
public class Memento {
	private String state;
	
	public Memento(String state) {
		this.state = state;
	}
	
	public String getState() {
		return this.state;
	}
}
​```

마지막으로 **CareTaker** 클래스이다. CareTaker 클래스는 Memento 객체를 관리하는 클래스이지만 예시로 사용한 Memento 클래스가 간단하기 때문에 별도의 클래스로 선언해 주지 않고 Stack 자료구조를 사용하여 관리해 준다.  

Client 클래스에서 A, B, C, D를 순서대로 타이핑하여 "ABCD" 문자열을 생성한 후 차례대로 이전 상태로 복구하는 작업을 수행해 준다.  

```java
import java.util.Stack;

public class Client {

	public static void main(String[] args) {
		Originator originator = new Originator();
		Stack<Memento> mementos = new Stack<>();
		
		originator.setState("A");
		mementos.push(originator.createMemento());
		originator.setState(originator.getState() + "B");
		mementos.push(originator.createMemento());
		originator.setState(originator.getState() + "C");
		mementos.push(originator.createMemento());
		originator.setState(originator.getState() + "D");
		mementos.push(originator.createMemento());
		
		while (!mementos.isEmpty()) {
			originator.restoreMemento(mementos.pop());
			System.out.println(originator.getState());
		}
	}
}
```

![메멘토 패턴1](https://user-images.githubusercontent.com/53072057/117246855-490f9a80-ae78-11eb-98ae-30b0e30c36ce.JPG)  

마치 Ctrl + Z 기능을 사용한 것처럼 이전 상태로 복구되는 것을 확인할 수 있다.  
