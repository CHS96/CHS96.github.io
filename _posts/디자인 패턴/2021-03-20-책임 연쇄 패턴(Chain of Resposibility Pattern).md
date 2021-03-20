---
title:  "책임 연쇄 패턴(Chain of Resposibility Pattern)"
excerpt: "책임 연쇄 패턴(Chain of Resposibility Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-03-20T18:35:00
---

책임 연쇄 패턴(Chain of Resposibility Pattern)은 **행위(Behavioral) 패턴 중 하나로써 명령 객체와 일련의 처리 객체를 포함하는 디자인 패턴**이다.  

요청을 처리할 수 있는 객체가 둘 이상 존재하여 한 객체가 처리하지 못하면 다음 객체로 넘어가는 형태의 패턴이다.  

즉, 요청을 처리할 수 있는 객체들이 **사슬(Chain)**로 묶여져있어서 어떤 요청이 들어왔을때 첫 번째 객체가 처리할 수 없다면 그 요청을 처리해 줄 수 있을 때까지 연쇄작용으로 다음 객체에게 처리를 양도하는 것이다.  

책임 연쇄 패턴의 구조는 다음과 같다. 연결 리스트, 재귀 함수를 생각하면 쉽다.  

![책임연쇄패턴](https://user-images.githubusercontent.com/53072057/111866312-efc6d700-89af-11eb-87c7-4165dce27950.JPG)  

*****

간단한 예시를 통해 책임 연쇄 패턴을 이해해보자.  

고객이 자판기에서 커피를 주문하는 상황을 생각해보자. 자판기에서 커피를 제공하기 위해서는 총 3가지를 수행해야 한다. 단, 객체들은 한 가지 기능밖에 수행 못한다고 가정하자.  

![책임연쇄패턴1](https://user-images.githubusercontent.com/53072057/111866315-f0f80400-89af-11eb-90c1-0add62fb33fe.JPG)  

우리는 이 문제를 책임 연쇄 패턴으로 처리해 줄 수 있다. 첫 번째 객체는 종이컵을 준비하는 기능을 수행하고 처리하지 못한 남은 요청을 두 번째 객체에게 넘겨주는 방식으로 처리해 줄 수 있기 때문이다.  

먼저 **Handler** 인터페이스 부분이다. 어떤 요청이 들어오면 해당 기능을 수행하는 메서드 하나를 가진다.  

```java
public interface Handler {
	public void requestHandler(Request request);
}
```

커피를 주문하는 요청 **Request** 클래스를 하나 구현해 준다. 요청은 3단계의 과정을 처리해 줘야 한다는 것을 표시하기 위해 level이라는 변수 하나를 사용한다.  

```java
public class Request {
	
	private int level;
	
	public Request() {
		this.level = 1;
	}

	public int getLevel() {
		return level;
	}

	public void setLevel(int count) {
		this.level = count;
	}
}
```

각 요청을 처리하는 기능을 가지는 3개의 클래스를 만들어 준다.  

<h2>[ 1. 종이컵을 준비하는 기능을 수행하는 클래스 ]</h2>  

```java
public class PaperCup implements Handler {
	
	private Handler nextHandler;
	
	public Handler getNextHandler() {
		return nextHandler;
	}
	
	public void setNextHandler(Handler nextHandler) {
		this.nextHandler = nextHandler;
	}

	@Override
	public void requestHandler(Request request) {
		int level = request.getLevel();
		System.out.println("Level " + level + " : 종이컵 준비");

		request.setLevel(level + 1);

        //아직 처리해야할 단계가 남아있고 넘겨줄 Hanlder가 존재하면
        //다음 객체에게 남은 처리를 넘겨줌
		if (level < 3 && this.nextHandler != null) {
			this.nextHandler.requestHandler(request);
		}
	}
}
```

<h2>[ 2. 커피 재료를 투하하는 기능을 수행하는 클래스 ]</h2>  

```java
public class Coffee implements Handler {

	private Handler nextHandler;
	
	public Handler getNextHandler() {
		return nextHandler;
	}
	
	public void setNextHandler(Handler nextHandler) {
		this.nextHandler = nextHandler;
	}
	
	@Override
	public void requestHandler(Request request) {
		int level = request.getLevel();
		System.out.println("Level " + level + " : 커피 재료를 투하");

		request.setLevel(level + 1);

        //아직 처리해야할 단계가 남아있고 넘겨줄 Hanlder가 존재하면
        //다음 객체에게 남은 처리를 넘겨줌
		if (level < 3 && this.nextHandler != null) {
			this.nextHandler.requestHandler(request);
		}
	}
}
```

<h2>[ 3. 뜨거운 물을 투하하는 기능을 수행하는 클래스 ]</h2>  

```java
public class HotWater implements Handler {

	private Handler nextHandler;

	public Handler getNextHandler() {
		return nextHandler;
	}

	public void setNextHandler(Handler nextHandler) {
		this.nextHandler = nextHandler;
	}

	@Override
	public void requestHandler(Request request) {
		int level = request.getLevel();
		System.out.println("Level " + level + " : 뜨거운 물 투하");

		request.setLevel(level + 1);

        //아직 처리해야할 단계가 남아있고 넘겨줄 Hanlder가 존재하면
        //다음 객체에게 남은 처리를 넘겨줌
		if (level < 3 && this.nextHandler != null) {
			this.nextHandler.requestHandler(request);
		}
	}
}
```

커피를 제조하는 모든 기능을 구현해 줬으니 Client는 커피를 주문하면 된다.  

```java
public class Client {

	public static void main(String[] args) {
		orderCoffee();
	}
	
	public static void orderCoffee() {
		PaperCup first = new PaperCup();
		Coffee second = new Coffee();
		HotWater third = new HotWater();
		
		first.setNextHandler(second);
		second.setNextHandler(third);
		
		first.requestHandler(new Request());
	}
}
```

![책임연쇄패턴2](https://user-images.githubusercontent.com/53072057/111866316-f0f80400-89af-11eb-9f7b-5f1df7b1b97c.JPG)  

![책임연쇄패턴3](https://user-images.githubusercontent.com/53072057/111866317-f1909a80-89af-11eb-9773-0e3e9552b07d.JPG)  


