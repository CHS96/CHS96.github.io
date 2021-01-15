---
title:  "템플릿 메서드 패턴(Template Method Pattern)"
excerpt: "템플릿 메서드 패턴(Template Method Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-01-15T18:35:00
---

템플릿 메서드 패턴은 **행위(Behavioral) 패턴 중 하나로써 알고리즘의 구조를 메서드에 정의하고 하위 클래스에서 알고리즘 구조의 변경 없이 알고리즘을 재정의하는 패턴**이다.  

간단하게 얘기하면 비슷한 알고리즘을 수행하는 친구들을 각각 별도의 클래스로 구현하지 말고 공통적인 기능을 템플릿이라는 틀(추상 클래스)로 만들어 사용하고 추가적인 기능은 각자 재정의하여 구현하는 방법이다.  

템플릿 메서드 패턴의 구조는 다음과 같다.  

![템플릿1](https://user-images.githubusercontent.com/53072057/104675412-d17ef980-5728-11eb-829b-69ac09030776.JPG)  

* AbstractClass : 템플릿 메서드를 정의하는 추상 클래스, templateMethod()는 operation1(), operation2()를 순서대로 실행하는 메서드  

* ConcreteClass : 상속받은 AbstractClass의 operation1(), operation2()를 구현하는 클래스  

간단한 예시를 통해 템플릿 메서드 패턴을 이해해 보자. 템플릿 메서드 패턴을 이해하는데 자주 사용되는 커피와 차를 끓이는 방법을 예시로 한 번 알아보자.  

일반적인 커피와 차를 끓이는 방법은 다음과 같다.  

![템플릿2](https://user-images.githubusercontent.com/53072057/104675414-d2b02680-5728-11eb-8c03-9188e32e9a78.JPG)  

이를 각각 별도의 클래스로 구현한다면 다음과 같을 것이다.  

```java
class Coffee {
	public void make_Coffee() {
		boil_Water();
		add_Coffee();
		stir();
		pour_Into_Cup();
	}
	
	public void boil_Water() {
		System.out.println("물을 끓인다.");
	}
	
	public void add_Coffee() {
		System.out.println("커피를 넣는다.");
	}
	
	public void stir() {
		System.out.println("잘 섞이도록 젓는다.");
	}
	
	public void pour_Into_Cup() {
		System.out.println("컵에 따른다.");
	}
}

class Tea {
	public void make_Tea() {
		boil_Water();
		add_Tea();
		stir();
		pour_Into_Cup();
	}
	
	public void boil_Water() {
		System.out.println("물을 끓인다.");
	}
	
	public void add_Tea() {
		System.out.println("차를 넣는다.");
	}
	
	public void stir() {
		System.out.println("잘 섞이도록 젓는다.");
	}
	
	public void pour_Into_Cup() {
		System.out.println("컵에 따른다.");
	}
}
```

두 클래스를 살펴보면 기능들이 상당히 비슷해 보이지 않는가? 비슷한 기능임에도 불구하고 코드가 중복 작성되어 있기 때문에 상당히 비효율적인 방법인 것을 알 수 있다. 그렇다면 템플릿 메서드 패턴을 적용시켜 개선해 보자.  

중복되는 코드를 우리는 객체지향 프로그래밍의 특징인 **추상화 및 상속**을 통해 간단한 형태로 개선이 가능하다. 커피와 차를 끓이는 방법 중 공통된 부분들을 추상화시켜 보자.  

"물을 끓인다.", "잘 섞이도록 젓는다", "컵에 따른다." 이 3개의 알고리즘은 두 클래스에 공통된 기능이다. 하지만 잘 생각해 보면 "커피/차를 넣는다."라는 기능 또한 "A를 넣는다."라는 기능으로 추상화가 가능하다. 따라서 우리는 추상 클래스를 다음과 같이 구현할 수 있다.  

```java
abstract class Make_beverage {
	public void make_beverage() {
		boil_Water();
		add_Something();
		stir();
		pour_Into_Cup();
	}
	
	private void boil_Water() {
		System.out.println("물을 끓인다.");
	}
	
	protected abstract void add_Something();
	
	private void stir() {
		System.out.println("잘섞이도록 젓는다.");
	}
	
	private void pour_Into_Cup() {
		System.out.println("컵에 따른다.");
	}
}
```

여기서 중요한 점은 각 기능들을 수행하는 templateMethod()는 모든 클래스에서 접근할 수 있도록 **public**으로 선언하지만 templateMethod()에 들어가는 각 기능들은 **private**로 선언해야 한다. 그 이유는 각 기능들은 외부에 노출돼서는 안되기 때문이다. 오직 templateMethod()를 통해서만 접근해야 한다. 다만 추상 메서드는 상속받는 하위 클래스에서 재정의해야 하기 때문에 다른 패키지에서 접근할 수 있도록 **protected**로 선언한다.  

이제 남은 건 커피와 차 클래스를 선언하여 Make_beverage 추상 클래스를 상속받고 add_Something() 메서드만 자신의 입맛대로 재정의해주면 된다.  

```java
abstract class Make_beverage {
    //템플릿 메서드 : 각 기능들을 순차적으로 실행하는 메서드
	public void make_beverage() {
		boil_Water();
		add_Something();
		stir();
		pour_Into_Cup();
	}
	
	private void boil_Water() {
		System.out.println("물을 끓인다.");
	}
	
	protected abstract void add_Something();
	
	private void stir() {
		System.out.println("잘 섞이도록 젓는다.");
	}
	
	private void pour_Into_Cup() {
		System.out.println("컵에 따른다.");
	}
}

class Coffee extends Make_beverage {
	@Override
	protected void add_Something() {
		System.out.println("커피를 넣는다.");
	}
}

class Tea extends Make_beverage {
	@Override
	protected void add_Something() {
		System.out.println("차를 넣는다.");
	}
}

class Client {
	private Make_beverage make_beverage;
	
	public Client(Make_beverage make_beverage) {
		this.make_beverage = make_beverage;
	}

	public void setMake_beverage(Make_beverage make_beverage) {
		this.make_beverage = make_beverage;
	}
	
	public void make_Beverage() {
		make_beverage.make_beverage();
	}
}

class Blog {
    public static void main(String[] args) {
    	Client client = new Client(new Coffee());
    	client.make_Beverage();
    	System.out.println("===================");
    	client.setMake_beverage(new Tea());
    	client.make_Beverage();
    }
}
```

![템플릿3](https://user-images.githubusercontent.com/53072057/104675416-d2b02680-5728-11eb-9c06-f8dbb6d4d296.JPG)  

템플릿 메서드 패턴을 적용시켜 중복된 부분을 줄여 코드 가독성을 향상시켰다. 또한 추상화 및 상속을 통해 확장성도 증가하였다.  
