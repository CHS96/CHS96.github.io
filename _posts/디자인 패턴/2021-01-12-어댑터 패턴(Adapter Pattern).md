---
title:  "어댑터 패턴(Adapter Pattern)"
excerpt: "어댑터 패턴(Adapter Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-01-12T18:35:00
---

어댑터 패턴(Adapter Pattern)은 **구조(Structural) 패턴 중 하나로써 클래스의 인터페이스를 사용자가 기대하는 다른 인터페이스로 변환하는 패턴으로, 호환성이 없는 인터페이스 때문에 함께 동작할 수 없는 클래스들이 함께 작동하도록 해주는 방법**이다.  

간단하게 얘기하면 해외여행 갈 때 자주 들고 가는 돼지코 어댑터를 생각하면 쉽다. 우리나라는 220V를 사용하지만 해외여행을 가다 보면 110V를 사용하는 곳도 있을 것이다. 하지만 돼지코 어댑터를 사용한다면 110V를 사용하는 곳에서도 220V를 사용할 수 있다. 이처럼 서로 호환성이 없지만 함께 작동하도록 해주는 것이 어댑터 패턴이다.  

어댑터 패턴의 구조는 다음과 같다.  

![어댑터1](https://user-images.githubusercontent.com/53072057/104268147-91c0d380-54d6-11eb-84e7-bce09f70d639.JPG)  

* Target : Adapter가 구현하는 인터페이스이며 Client는 Target을 통해 Adaptee 기능을 사용

* Adapter : Adaptee가 Target에 호환될 수 있도록 연결해 주는 역할

* Adaptee : Adapter를 통해 Target에 호환시킬 기능  

한 번 간단한 코드를 통해 어댑터 패턴을 이해해 보자.  

위에서 언급한 것처럼 Client라는 객체가 여행을 다니면서 상황에 따라 다양한 전기를 사용한다고 생각해 보자. 220V 전기를 사용하려면 다음과 같이 220V 클래스를 하나 구현할 것이다.  

```java
class Volt_220 {
	public void volt_Connect() {
		System.out.println("220V 전용입니다.");
	}
}
```

그렇다면 만약 110V을 추가하려면 어떻게 해야 할까? 220V와는 전혀 다른 전압이기 때문에 새로운 110V 클래스를 구현해야 할 것이다.  

```java
class Volt_110 {
	public void volt_Connect() {
		System.out.println("110V 전용입니다.");
	}
}
```

매번 새로운 전압에 따른 클래스를 구현하는 것은 상당히 귀찮을 거다. 현재는 연결하는 메서드 한 개뿐이라 간단해 보일지라도 클래스 내부가 상당히 복잡하다면 더욱 번거롭다. 또한 Client에서도 상황에 따라 매번 전압에 따른 객체를 상속하고 사용해야 하기 때문에 이는 굉장히 비효율적이다.  

```java
----------- 220V 사용하는 Client -------------
class Client extends Volt_220 {}

Client client = new Client();
client.volt_Connect();

----------- 110V 사용하는 Client -------------
class Client extends Volt_110 {}

Client client = new Client();
client.volt_Connect();
```

그렇다면 한 번 어댑터 패턴을 적용시켜 코드를 개선해보자. 전기를 연결하는 connectApdater라는 인터페이스를 통해 연결하는 메서드를 구현한다면 Client는 단지 connectAdapter 객체를 통해 원하는 전기를 사용할 수 있다.  

```java
interface ConnectAdapter {
	public void volt_Connect();
}

class Volt_220 implements ConnectAdapter {
	@Override
	public void volt_Connect() {
		System.out.println("220V 전용입니다.");
	}
}

class Volt_110 implements ConnectAdapter {
	@Override
	public void volt_Connect() {
		System.out.println("110V 전용입니다.");
	}
}

class Client {
	ConnectAdapter connectAdapter;
	
	public Client(ConnectAdapter connectAdapter) {
		this.connectAdapter = connectAdapter;
	}
	
	public void setConnectAdapter(ConnectAdapter connectAdapter) {
		this.connectAdapter = connectAdapter;
	}
	
	public void volt_Connect() {
		connectAdapter.volt_Connect();
	}
}

class Blog {
    public static void main(String[] args) {
    	Client client = new Client(new Volt_220());
    	client.volt_Connect();

    	client.setConnectAdapter(new Volt_110());
    	client.volt_Connect();
    }
}
```

Client는 어댑터 인터페이스로만 의존하기 때문에 다양한 전기를 사용한다 하여도 Clinet의 코드는 변경할 필요가 없어진다. 또한 서로 호환되는 인터페이스가 별로 없는 클래스들이 함께 작동되게 해주는 장점이 있다.  