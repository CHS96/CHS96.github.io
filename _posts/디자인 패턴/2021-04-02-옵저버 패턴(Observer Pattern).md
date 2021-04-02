---
title:  "옵저버 패턴(Observer Pattern)"
excerpt: "옵저버 패턴(Observer Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-04-02T18:35:00
---

옵저버 패턴(Observer Pattern)은 **행위(Behavioral) 패턴 중 하나로써 한 객체의 상태가 변화하면 객체에 상속되어 있는 다른 객체들에게 변화된 상태를 전달해주는 패턴**이다.  

옵저버 패턴은 **일대다 관계**를 가진다.  

주로 분산된 시스템 간에 이벤트를 생성·발행(Publish)하고, 이를 수신(Subscribe)해야 할 때 이용한다.  

Observer 영어 그대로 **관찰자**라고 생각하면 이해하기 쉽다. 한 객체를 Observer를 통해 관찰하다가 어떤 이벤트가 발생하면 이를 Observer를 통해 다른 객체들에게 알려주는 것이다.  

옵저버 패턴의 구조는 다음과 같다.  

![옵저버](https://user-images.githubusercontent.com/53072057/113374226-95fadf80-93a7-11eb-891a-cfc30a8c600c.JPG)  

* Subject : Observable이라고도 부르며 이벤트가 발생하는 객체, Observer의 리스트를 가지고 있음, Observer의 등록/해제 메서드를 이용하여 Subject을 등록/해제할 수 있음  

* Observer : Subject의 이벤트를 관찰하는 대상, 이벤트가 발생하면 연결된 하위 대상들에게 알려줌  

* ConcreteObserver : 각자의 역할을 수행하면서 Observer를 통해 Subject의 이벤트를 감지  

*****

유튜브의 구독 알람을 예시로 옵저버 패턴을 이해해보자.  

A라는 유튜버 채널이 있다고 하자. 이 채널은 다수의 사람들이 구독하고 있을 것이다.  

A라는 유튜버가 새로운 영상을 업로드하였다고 하자. 아무런 기능이 없다면 구독자는 매번 유튜버의 채널을 들어가서 확인해야 하는 번거로운 상황이 발생한다.  

하지만 유튜브는 구독과 알림이라는 기능을 통해 구독자에게 새로운 영상이 업로드되었다는 것을 알려준다. 이 기능을 통해 구독자는 일일이 확인할 필요가 없어진다.  

이때 구독자에게 알려주는 역할을 하는 친구가 바로 **옵저버(Observer)**이다.  

그렇다면 이러한 동작 과정을 옵저버 패턴을 이용해서 구현해보자.  

먼저 **Subject** 인터페이스이다. Observer를 등록/해제하는 기능과 어떤 이벤트가 발생했을 때 Observers들에게 알려주는 기능을 가진다.  

```java
public interface Subject {
	public void registerObserver(Observer observer);
	public void unregisterObserver(Observer observer);
	public void notifyObservers(String msg);
}
```

다음으로 **Youtuber** 클래스이다. Observer들을 관리해 줄 List를 하나 가지고 있으며 Subject 인터페이스의 기능들을 구현해준다.  

```java
import java.util.ArrayList;
import java.util.List;

public class YouTuber implements Subject {
	private List<Observer> subscriberList;
	
	public YouTuber() {
		subscriberList = new ArrayList<>();
	}
	
	@Override
	public void registerObserver(Observer observer) {
		subscriberList.add(observer);
	}

	@Override
	public void unregisterObserver(Observer observer) {
		subscriberList.remove(observer);
	}

	@Override
	public void notifyObservers(String msg) {
		for (Observer observer : subscriberList) {
			observer.notifySubscriber(msg);
		}
	}
}
```

다음으로 **Observer** 인터페이스이다. 자신을 구현한 하위 클래스들에게 Subject의 이벤트를 알려주는 기능을 가진다.  

```java
public interface Observer {
	public void notifySubscriber(String msg);
}
```

마지막으로 **Subscriber** 클래스이다. Observer 인터페이스를 구현하여 이벤트를 감지한다.  

```java
public class SubscriberA implements Observer {

	@Override
	public void notifySubscriber(String msg) {
		System.out.println(this.getClass() + " : " + msg);
	}
}

public class SubscriberB implements Observer {

	@Override
	public void notifySubscriber(String msg) {
		System.out.println(this.getClass() + " : " + msg);
	}
}
​```

**Main** 클래스에서 youtuber에게 두 명의 구독자를 등록한 후 새로운 영상이 업로드되었다고 알려주면 Observer가 구독자들에게 이를 알려준다.  

```java
public class Main {

	public static void main(String[] args) {
		YouTuber youTuber = new YouTuber();
		Observer subscriberA = new SubscriberA();
		Observer subscriberB = new SubscriberB();
		
		System.out.println("subscriberA 구독 등록");
		youTuber.registerObserver(subscriberA);
		System.out.println("subscriberB 구독 등록");
		youTuber.registerObserver(subscriberB);
		
		youTuber.notifyObservers("New Video Upload!");
	}
}
```

![옵저버1](https://user-images.githubusercontent.com/53072057/113374228-972c0c80-93a7-11eb-88e5-484176ced0cd.JPG)  

*****

사실 Java에는 옵저버 패턴의 기능을 수행해주는 Observer 인터페이스와 Observable 클래스가 구현되어 있다.  

![옵저버2](https://user-images.githubusercontent.com/53072057/113374230-972c0c80-93a7-11eb-8c86-6ba0463e3253.JPG)  

![옵저버3](https://user-images.githubusercontent.com/53072057/113374231-97c4a300-93a7-11eb-82f1-248b5f769863.JPG)  

즉, 직접 구현하지 않고 이들을 통해 다음과 같이 옵저버 패턴을 사용할 수 있다.  

먼저 Observable 클래스를 상속받는 **Youtuber** 클래스이다.  

```java
import java.util.Observable;

public class YouTuber extends Observable {
	
	public void notifyEvent(String msg) {
		setChanged(); //동기화를 위해 setChanged()를 실행해야지만 notifyObservers()가 실행됨
		notifyObservers(msg);
	}
}
```

여기서 주의해야 할 점은 notifyObservers() 메서드를 직접 호출하는 것이 아닌 별도의 메서드를 하나 구현하여 내부에서 호출하도록 한 것이다.  

이처럼 구현하는 이유는 **setChanged()** 메서드 때문이다. Observable 클래스는 동기화를 위해 changed 변수가 true 일 때만 notifyObservers() 메서드가 호출된다.  

![옵저버4](https://user-images.githubusercontent.com/53072057/113374232-97c4a300-93a7-11eb-9c43-af4969fd7311.JPG)  

다음으로 Observer 인터페이스를 구현하는 **Subscriber** 클래스이다. Observer 인터페이스의 update() 메서드를 재정의하여 사용한다.  

```java
import java.util.Observable;
import java.util.Observer;

public class SubscriberA implements Observer {

	@Override
	public void update(Observable o, Object arg) {
		System.out.println(this.getClass() + " : " + arg);
	}
}

public class SubscriberB implements Observer {

	@Override
	public void update(Observable o, Object arg) {
		System.out.println(this.getClass() + " : " + arg);
	}
}
```

**Main**에서 실행한 결과를 보면 이전의 방식과 동일한 결과를 얻는 것을 확인할 수 있다.  

```java
import java.util.Observer;

public class Main {

	public static void main(String[] args) {
		YouTuber youTuber = new YouTuber();
		Observer subscriberA = new SubscriberA();
		Observer subscriberB = new SubscriberB();
		
		System.out.println("subscriberA 구독 등록");
		youTuber.addObserver(subscriberA);
		System.out.println("subscriberB 구독 등록");
		youTuber.addObserver(subscriberB);
		
		youTuber.notifyEvent("New Video Upload!");
	}
}
```

![옵저버5](https://user-images.githubusercontent.com/53072057/113374234-985d3980-93a7-11eb-990d-fa0ca0582528.JPG)  

이처럼 옵저버 패턴을 직접 구현할 수도 있지만 Java에서 제공하는 기능으로 옵저버 패턴을 사용할 수도 있다.  

두 방식을 설명한 이유는 **각 방식이 장단점이 존재**하기 때문이다.  

Java에서는 다중 상속을 금지한다. 후자의 방식은 인터페이스가 아닌 클래스로 구현되어 있기 때문에 다중 상속이 허용되지 않는다. 반면 전자의 방식은 입맛에 따라 인터페이스로 구현할 수 있기 때문에 다중 상속이 허용된다.  

하지만 전자의 방식은 아무래도 직접 구현해야 한다는 까다로운 단점이 존재한다. 직접 클래스와 인터페이스를 만들어줘야 하며 동기화 같은 문제들도 처리해 줘야 한다.  

따라서 상황에 따라 두 방식을 적절히 사용해야 할 것이다.  
