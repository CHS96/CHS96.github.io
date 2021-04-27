---
title:  "상태 패턴(State Pattern)"
excerpt: "상태 패턴(State Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-04-27T18:35:00
---

상태 패턴(State Pattern)은 **행위(Behavioral) 패턴 중 하나로써 객체의 상태에 따라 동일한 동작을 다르게 처리해야 할 때 사용하는 패턴**이다.  

객체 상태를 클래스로 나타내어 캡슐화하고 어떤 동작을 수행할 때 객체의 상태를 참조하는 방식으로 처리한다.  

전략 패턴과 비슷한 패턴이지만 차이점이 존재한다.  

전략 패턴은 특정한 계열의 알고리즘들을 캡슐화하여 필요에 따라 외부에서 교체하는 패턴이었다면 상태 패턴은 내부에서 어떤 동작을 객체의 상태에 따라 수행하도록 구현하여 적합한 동작을 수행하도록 하는 패턴이다.  

즉, 전략 패턴은 외부(클라이언트)에서의 개입이 요구되지만 상태 패턴은 외부에서의 개입이 요구되지 않는다.  

전략 패턴은 **상속을 대체하려는 목적**, 상태 패턴은 **조건문들을 대체하려는 목적**으로 사용한다고 한다.  

![상태1](https://user-images.githubusercontent.com/53072057/116241784-3cb47f00-a7a0-11eb-87c4-29a2f9d4306f.JPG)  

* Context : State 객체를 가지는 클래스, State의 상태를 변경하는 기능을 가짐, 직접 동작을 실행하는 대신 request()로 State 객체에게 실행하도록 위임한다.  

* State : 상태를 나타내는 인터페이스, Context의 내부에서 StateA, StateB 등 다양한 상태를 변경하면서 그에 따른 동작을 수행하도록 해줌  

* StateA, StateB : State를 실제로 구현한 클래스, 여러 가지의 상태를 나타냄  

*****

형광등을 예시로 상태 패턴에 대해 알아보도록 하자.  

형광등은 전원을 On/Off 하는 버튼을 가진다고 하자. 현재 형광등의 전원이 On/Off 유무에 따라 버튼을 눌렸을 경우 다음과 같은 동작이 수행된다.  

```java
STATE = ON 
     if push_On then Nothing Happens 
     if push_Off then Power OFF

STATE = OFF
     if push_On then Power ON
     if push_Off then Nothing Happens 
```


형광등의 On/Off를 상태로 표현할 수 있기 때문에 상태 패턴을 적용하여 구현해보자.  

먼저 **State** 인터페이스이다. On/Off 버튼을 눌렸을 경우 어떤 action()이 취하도록 하는 메서드를 가진다.  

```java
interface State {
	public void ON(Light light);
	public void OFF(Light light);
}
```


다음으로 **On** 클래스이다. State를 On으로 변경해 주는 클래스로 현재 형광등의 상태인 On/Off에 따라 On 버튼을 눌렸을 경우 어떤 일이 발생하는지를 구현해 준다.  

```java
class On implements State {

	@Override
	public void ON(Light light) {
		System.out.println("Nothing Happens");
	}

	@Override
	public void OFF(Light light) {
		System.out.println("Power OFF");
		light.setState(new Off());
	}
}
```


다음으로 **Off** 클래스이다. State를 Off으로 변경해 주는 클래스로 현재 형광등의 상태인 On/Off에 따라 Off 버튼을 눌렸을 경우 어떤 일이 발생하는지를 구현해 준다.  

```java
class Off implements State {

	@Override
	public void ON(Light light) {
		System.out.println("Power ON");
		light.setState(new On());
	}

	@Override
	public void OFF(Light light) {
		System.out.println("Nothing Happens");
	}
}
```


마지막으로 형광등을 표현하는 **Light** 클래스이다. 내부에 상태를 나타내는 State 객체를 하나 가진다. 또한, 각 버튼의 기능을 Light에서 구현하지 않고 State 객체에게 위임하여 상태를 변경하도록 해준다.  

State에게 위임함으로써 Light 클래스는 아무리 기능이 추가되어도 코드가 복잡해지지 않는다.  

```java
public class Light {
	private State state;
	
	public Light() {
		state = new Off(); //초기 상태는 전원이 꺼져있음
	}
	
	public State getState() {
		return state;
	}
	
	public void setState(State state) {
		this.state = state;
	}

	public void On() {
		state.ON(this);
	}
	
	public void Off() {
		state.OFF(this);
	}
}
​```


**Main** 클래스에서 실행한 결과는 다음과 같다.  

```java
public class Main {

	public static void main(String[] args) {
		Light light = new Light();
		light.Off();
		light.On();
		light.On();
		light.Off();
	}
}
```

![상태2](https://user-images.githubusercontent.com/53072057/116241791-3de5ac00-a7a0-11eb-8c84-0dbbdc64a3d4.JPG)  

클라이언트 입장에서는 현재 형광등의 상태가 어떤지에 상관없이 On(), Off() 메서드만 사용하여 형광등의 상태를 쉽게 변경할 수 있음을 확인할 수 있다.  
