---
title:  "방문자 패턴(Visitor Pattern)"
excerpt: "방문자 패턴(Visitor Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-03-10T18:35:00
---

방문자 패턴(Visitor Pattern)은 **행위(Behavioral) 패턴 중 하나로써 알고리즘을 객체 구조에서 분리시키는 패턴**이다.  

보통 클래스에서 객체를 다루는 기능을 객체를 나타내는 클래스 내부에 메서드로 구현하여 함께 구현하지만 방문자 패턴은 각 클래스들의 데이터 구조에서 처리 기능을 분리하여 별도의 클래스로 구현하는 디자인 패턴이다.  

분리된 처리 기능은 **방문자(Visitor)**를 통해 각 클래스들을 방문하면서 수행한다.  

객체와 알고리즘을 분리하면 구조를 수정하지 않고도 실질적으로 새로운 동작을 기존의 객체 구조에 추가할 수 있는 장점이 있다.  

객체지향 5원칙(SOLID) 중 하나인 **개방-폐쇄 원칙(Open-Closed Principle, OCP)**을 적용하는 방법이다.  

방문자 패턴의 구조는 다음과 같다.  

![방문자패턴](https://user-images.githubusercontent.com/53072057/110579253-bf11c100-81a9-11eb-91a0-b4adae48d2fe.JPG)  

* Visitor : 방문자를 표현하는 인터페이스, visit 메서드는 Element 객체를 주입받아 해당 공간을 방문  

* ConcreteVisitor : Visitor 인터페이스를 실제로 구현하는 클래스  

* Element : 방문자가 방문할 공간을 표현하는 인터페이스, accept 메서드는 방문할 Visitor를 주입받음  

* ConcreteElement : Element 인터페이스를 실제로 구현하는 클래스  

*****

간단하게 여러 가지 Element 객체가 존재할 때 Visitor 객체를 통해 접근하도록 해보자.  

우선 기본적으로 Visitor 인터페이스와 Element 인터페이스는 다음과 같이 구성된다.  

```java
public interface Visitor {
	public void visit(Element element);
}

public interface Element {
	public void accept(Visitor visitor);
}
```

다음과 같이 두 종류의 Element 객체가 존재한다고 하자. 이들은 Visitor 객체를 인자로 받아서 visit() 메서드를 수행한다.  

```java
public class ElementA implements Element {

	@Override
	public void accept(Visitor visitor) {
		visitor.visit(this);
	}
}

public class ElementB implements Element {

	@Override
	public void accept(Visitor visitor) {
		visitor.visit(this);
	}
}
```

ElementA를 방문하는 VisitorA, ElementB를 방문하는 VisitorB를 구현해준다. 각 visit 메서드는 해당 객체일 경우에만 원하는 기능을 수행하도록 해준다.  

```java
public class VisitorA implements Visitor {

	@Override
	public void visit(Element element) {
		if (element instanceof ElementA) {
            //원하는 기능
			System.out.println("VisitorA : " + element.getClass());
		} else {
			System.out.println("ElementA 객체가 아닙니다.");
		}
	}
}

public class VisitorB implements Visitor {

	@Override
	public void visit(Element element) {
		if (element instanceof ElementB) {
            //원하는 기능
			System.out.println("VisitorB : " + element.getClass());
		} else {
			System.out.println("ElementB 객체가 아닙니다.");
		}
	}
}
```

Client 클래스에서 Element 객체들이 어디에 속하는지 Visitor 객체를 통해 수행해준다.  

```java
public class Client {
	public static void main(String[] args) {
		Element elementA = new ElementA();
		elementA.accept(new VisitorA());
		
		Element elementB = new ElementB();
		elementB.accept(new VisitorB());
		
		elementA.accept(new VisitorB());
		elementB.accept(new VisitorA());
	}
}
```

![방문자패턴1](https://user-images.githubusercontent.com/53072057/110579254-c042ee00-81a9-11eb-9bbc-7c3def9f67f6.JPG)  

일반적인 방식은 객체의 입장에서 메서드를 통해 어떤 기능을 수행하는 것이었다면 방문자 패턴은 기능을 하는 클래스의 입장에서 객체를 넘겨받아 기능을 수행하도록 하였다.  
​
사실 방문자 패턴을 어떤 부분에서 사용해야 하는지 아직 감이 잘 잡히지는 않는다. 기능을 따로 클래스로 구현하는 것이다 보니 데이터 구조는 변경이 없고 기능이 자주 변경되는 경우에 사용하는 것 같은데 결국 Element와 Visitor 간의 결합도가 상승하기 때문에 안 좋은 것 같기도 하고 잘 모르겠다.  
