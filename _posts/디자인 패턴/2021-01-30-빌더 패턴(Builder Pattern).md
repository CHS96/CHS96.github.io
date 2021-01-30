---
title:  "빌더 패턴(Builder Pattern)"
excerpt: "빌더 패턴(Builder Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-01-30T18:35:00
---

빌더 패턴은 **생성(Creational) 패턴 중 하나로써 복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현 결과를 만들 수 있게 하는 패턴**이다.  

간단하게 얘기하면 복잡한 객체를 생성하는 과정을 추상화하여 서브 클래스에게 넘겨 서브 클래스에서 객체를 생성하도록 하는 디자인 패턴이다.  

빌더 패턴의 구조는 다음과 같다.  

![빌더1](https://user-images.githubusercontent.com/53072057/106345503-0461e980-62f4-11eb-80cd-d09eb67561e6.JPG)  

* Director : 내부적으로 Builder 인터페이스 객체를 통해 Product를 생성하는 클래스  
* Builder : Product에 필요한 기능을 가지는 인터페이스  
* ConcreteBuilder : Builder 인터페이스를 상속받아 구체적으로 기능을 구현하고 Product를 생성하는 클래스  
* Product : 생성해야 할 복합 객체  
 
간단한 예시를 통해 빌더 패턴을 이해해 보자.  

스마트폰 객체를 생성해야 한다고 가정하자. 스마트폰 기본적으로 CPU, RAM, OS를 변수로 가지고 있다고 하자. 그렇다면 다음과 같이 클래스를 하나 생성하여 생성자를 통해 간단하게 객체를 만들 수 있다.  

```java
public class SmartPhone {
	
	private String CPU;
	private String RAM;
	private String OS;
	
    public SmartPhone(String CPU, String RAM, String OS) {
		this.CPU = CPU;
		this.RAM = RAM;
		this.OS = OS;
	}
}

public class Main {
	public static void main(String[] args) {
		SmartPhone smartPhone = new SmartPhone("CPU", "RAM", "OS");
	}
}
```

지금은 스마트폰이 변수 3개만 가지지만 훨씬 많은 종류의 변수들을 가진다면 어떻게 될까? Main 클래스에 코드가 더욱 복잡해질 것이다. 또한 스마트폰은 다양한 종류가 존재하며 각 스마트폰의 제원(CPU, RAM, OS)들은 한 가지 종류가 있는 것이 아니다. 즉, 이렇게 객체를 생성하는 방법은 상당히 비효율적인 방법이다.  

그렇다면 빌더 패턴을 적용시켜 개선해 보자.  

먼저 Product인 **SmartPhone** 클래스를 생성할 것이다. 간단하게 CPU, RAM, OS 3개의 변수만 가진다고 하자.  

```java
public class SmartPhone {
	
	private String CPU;
	private String RAM;
	private String OS;
	
	public String getCPU() {
		return CPU;
	}
	
	public void setCPU(String CPU) {
		this.CPU = CPU;
	}
	
	public String RAM() {
		return RAM;
	}
	
	public void setRAM(String RAM) {
		this.RAM = RAM;
	}
	
	public String getOS() {
		return OS;
	}
	
	public void setOS(String OS) {
		this.OS = OS;
	}

	@Override
	public String toString() {
		return "SmartPhone [CPU=" + CPU + ", RAM=" + RAM + ", OS=" + OS + "]";
	}
}
```

또한 다양한 스마트폰을 생성하기 위한 **SmartPhoneBuilder** 인터페이스를 구현하자. **이후에 스마트폰의 종류에 따라 각각 필요한 기능을 재정의하여 구현하도록 해주기 위해서이다.** SmartPhoneBuilder 인터페이스는 각 제원을 설정하는 메서드와 생성한 Product를 반환하는 메서드를 가진다.  

```java
public interface SmartPhoneBuilder {
	
	public void set_CPU();
	public void set_RAM();
	public void set_OS();
	
	public SmartPhone getSmartPhone();
}
```

다음으로 스마트폰 종류 중 하나인 Galaxy S20을 생성하는 **GalaxyS20Builder** 클래스를 구현해준다. SmartPhoneBuilder 인터페이스를 상속받아 GalaxyS20Builder에 적합한 제원들을 설정해준다.  

```java
public class GalaxyS20Builder implements SmartPhoneBuilder {

	private SmartPhone smartPhone;
	
	public GalaxyS20Builder() {
		smartPhone = new SmartPhone();
	}
	
	@Override
	public void set_CPU() {
		smartPhone.setCPU("스냅드래곤 865");
	}

	@Override
	public void set_RAM() {
		smartPhone.setRAM("12GB");
	}

	@Override
	public void set_OS() {
		smartPhone.setOS("Android");
	}

	@Override
	public SmartPhone getSmartPhone() {
		return smartPhone;
	}
}
```

마지막으로 스마트폰을 생성해주는 **Director** 클래스이다. 원하는 스마트폰의 객체를 통해 스마트폰 객체를 생성한다.  

```java
public class Director {
	
	private SmartPhoneBuilder builder;
	
    //원하는 Builder 객체를 설정
	public Director(SmartPhoneBuilder builder) {
		this.builder = builder;
	}
	
    //설정한 Builder 객체를 통해 Product를 생성
	public SmartPhone build() {
		builder.set_CPU();
		builder.set_RAM();
		builder.set_OS();
		
		return builder.getSmartPhone();
	}
}
```

**Main** 클래스에선 직접 스마트폰을 생성하지 않고 Director 클래스를 통해 스마트폰을 생성한다.  

```java
public class Main {
	public static void main(String[] args) {
		Director director = new Director(new GalaxyS20Builder());
		SmartPhone smartPhone = director.build();
		System.out.println(smartPhone.toString());
	}
}
```

![빌더2](https://user-images.githubusercontent.com/53072057/106345504-04fa8000-62f4-11eb-9c96-05099bcd6a44.JPG)  

Main 클래스를 보면 이전에 비해 상당히 깔끔해진 것을 확인할 수 있다. 또한 다양한 스마트폰을 생성한다 하여도 Main 클래스에 따로 추가하지 않아도 되는 장점이 있다.  

*****

사실 위의 패턴은 요즘에 언급하는 빌더 패턴과는 조금 다른 개념이다. 위의 패턴의 목적은 객체의 생성 과정과 표현 방법을 분리하는 것이라면 **현대의 빌더 패턴의 목적은 변수가 많은 객체를 생성할 때 가독성, 유지 보수하기 편리하도록 구현하는 방법을 의미한다.**  

간단하게 이전의 스마트폰의 객체를 생성하는 예시를 통해 이해해 보자. SmartPhone 클래스는 이전과 동일하다.

스마트폰 객체를 생성할 때 우리는 생성자를 통해 생성한다.  

```java
SmartPhone smartPhone = new SmartPhone("CPU", "RAM", "OS");
```

위의 3개 변수는 필수적인 요소라고 가정하고 SmartPhone에 필수적이지 않은 변수 A가 새로 생겼다고 하자. 스마트폰 객체를 생성한다면 어떻게 될까? A라는 변수는 꼭 없어도 되는 변수이기 때문에 null 값이 될 수 있다.  

```java
SmartPhone smartPhone1 = new SmartPhone("CPU", "RAM", "OS", "A");
SmartPhone smartPhone2 = new SmartPhone("CPU", "RAM", "OS", null);
```

하지만 매번 null 값을 넣어줘야 하는게 귀찮을 수도 있다. 이를 제거할 수 있는 방법은 없을까? 간단하다 필수적인 3개의 변수만 가지는 생성자를 추가해주면 된다.  

```java
public SmartPhone(String CPU, String RAM, String OS) {
	this.CPU = CPU;
	this.RAM = RAM;
	this.OS = OS;
}
	
public SmartPhone(String CPU, String RAM, String OS, String A) {
	this.CPU = CPU;
	this.RAM = RAM;
	this.OS = OS;
	this.A = A;
}
```

그런데 이게 과연 좋은 방법일까? 변수가 많고 조건이 다양할수록 구현해야 할 생성자는 훨씬 많아질 것이다. 결국 코드도 길어지고 복잡해지면서 가독성이 떨어질 것이다.  

두 번째 단점은 생성자의 각 변수들의 의미를 파악하기가 힘들고 잘못된 값을 타이핑할 수가 있다. 예를 들어 다음과 같이 생성자를 선언했다고 하자.  

```java
SmartPhone smartPhone = new SmartPhone("asdf", "dfasdf", "dsfasd");
```

1, 2, 3번째 변수가 무엇을 나타내는지 한 번에 파악하기가 힘들다. 또한 3개의 변수가 모두 String이기 때문에 실수로 잘못된 값을 값을 바꿔서 타이핑하는 경우도 발생할 수 있을 것이다.  

이러한 문제점들을 개선하기 위해 현대의 빌더 패턴을 적용시켜보자.  

SmartPhone 클래스 내부에 Builder 클래스를 생성한다. Main 클래스에서 사용할 수 있도록 **static** 클래스로 선언해준다.  

```java
public class SmartPhone {
	
	private String CPU;
	private String RAM;
	private String OS;
	private String A;

	//Main 클래스에서 사용하기 위해 static 클래스로 생성
	public static class Builder {
		//필수적인 변수
		private String CPU;
		private String RAM;
		private String OS;
		
		//필수적이지 않은 변수
		private String A;

		//필수적인 값들은 생성자 생성시 초기화
		public Builder(String CPU, String RAM , String OS) {
			this.CPU = CPU;
			this.RAM = RAM;
			this.OS = OS;
		}

		//이런식으로 메서드를 구성하면 파이프라이닝처럼 메서드를 사용 가능
		public Builder set_A(String A) {
			this.A = A;
			return this;
		}
		
		public SmartPhone build() {
			return new SmartPhone(this);
		}
	}
	
	private SmartPhone(Builder builder) {
		this.CPU = builder.CPU;
		this.RAM = builder.RAM;
		this.OS = builder.OS;
		this.A = builder.A;
	}
}
```

Main 클래스에서는 다음과 같이 파이프라이닝 기법을 사용하여 스마트폰 객체를 생성할 수 있다.  

```java
public class Main {
	public static void main(String[] args) {
		SmartPhone smartPhone = new SmartPhone.Builder("CPU", "RAM", "OS").set_A("A").build();
	}
}
```

빌드 패턴을 통해 객체를 생성하는 장점은 다음과 같다.  

![빌더3](https://user-images.githubusercontent.com/53072057/106345505-05931680-62f4-11eb-8d4a-1d89726c0f97.JPG)
