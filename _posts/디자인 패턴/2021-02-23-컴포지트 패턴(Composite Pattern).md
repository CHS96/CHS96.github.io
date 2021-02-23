---
title:  "컴포지트 패턴(Composite Pattern)"
excerpt: "컴포지트 패턴(Composite Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-02-23T18:35:00
---

컴포지트 패턴(Composite Pattern)은 **구조(Structural) 패턴 중 하나로써 여러 객체를 가진 복합 객체와 단일 객체를 구분 없이 다루고자 할 때 사용하는 패턴**이다.  

객체들의 관계를 트리 구조로 구성하여 부분-전체 계층을 표현하는 패턴으로, 사용자가 단일 객체와 복합 객체 모두 동일하게 다루도록 해준다.  

주로 객체들을 계층 구조로 표현할 수 있으며, 클라이언트가 단일 객체와 복합 객체를 구분하지 않고 사용하고자 할 경우 사용하는 디자인 패턴이다.  

간단하게 PC의 파일과 디렉터리의 계층 구조를 생각하면 이해하기 쉽다. 파일은 자신의 밑에 다른 파일이나 디렉터리를 담을 수 없기 때문에 단일 객체이며, 디렉터리는 자신의 밑에 파일이나 디렉터리를 담을 수 있기 때문에 복합 객체로 표현한다.  

이들을 구분하여 각각 구현하지 말고 추상화를 통해 파일이나 디렉터리를 하나의 공통된 인터페이스로 구현하는 것을 의미한다.  

컴포지트 패턴의 구조는 다음과 같다.  

![컴포지트1](https://user-images.githubusercontent.com/53072057/108803988-7372eb00-75df-11eb-91ab-7686aae17ef9.JPG)  

* Component : 단일 객체(Leaf)와 복합 객체(Composite)를 추상화한 공통 인터페이스, operation()은 Leaf와 Composite가 공통적으로 가져야 할 기능  

* Leaf :  단일 객체를 표현하는 클래스, 트리의 단말 노드  

* Composite : 복합 객체를 표현하는 클래스, 추가적인 기능들을 통해 Component 객체를 관리(Leaf를 담는 그릇 역할)  

그렇다면 파일과 디렉터리의 구조를 구현하는 예시를 통해 컴포지트 패턴을 이해해보자.  

먼저 컴포지트 패턴을 사용하지 않고 구현해보자. 파일 이름을 가지는 파일 클래스는 간단하게 다음과 같이 구현할 수 있다.  

```java
public class File {
	String name;
	
	public File (String name) {
		this.name = name;
	}
}
```

디렉터리 클래스는 파일과 디렉터리를 자식으로 가질 수 있기 때문에 다음과 같이 구현할 수 있다.  

```java
import java.util.ArrayList;
import java.util.List;

public class Directory {
	String name;
	List<File> fileList;
	List<Directory> directoryList;
	
	public Directory(String name) {
		this.name = name;
		fileList = new ArrayList<>();
		directoryList = new ArrayList<>();
	}
	
	public void addFile(File file) {
		fileList.add(file);
	}
	
	public void addDirectory(Directory directory) {
		directoryList.add(directory);
	}
}
```

Main 클래스는 다음과 같다.  

```java
public class Main {
	public static void main(String[] args) {
		Directory root = new Directory("root");
		Directory users = new Directory("users");
		Directory administrator = new Directory("administrator");
		Directory download = new Directory("download");
		File A_img = new File("A.img");
		File B_img = new File("B.img");
		
		root.addDirectory(users);
			users.addDirectory(administrator);
				administrator.addDirectory(download);
					download.addFile(A_img);
					download.addFile(B_img);		
	}
}
```

이 코드가 우리가 원하는 파일과 디렉터리 구조와 다른 것은 아니다. 하지만 상당히 비효율적인 코드이다.   
​
첫 번째로 기능을 추가할 때마다 2개의 메서드를 구현해줘야 한다는 단점이 있다. 현재 파일과 디렉터리를 추가하는 메서드를 보면 각각 별도의 메서드로 구현된 것을 확인할 수 있다.  

또한, 메서드가 다양해졌기 때문에 클라이언트는 매번 객체에 적합한 메서드를 찾아서 사용해야 하는 불편함이 존재한다.  

그렇다면 컴포지트 패턴을 적용시켜 개선시켜보자.  

파일과 디렉터리는 상당히 공통적인 부분이 많다. 공통적인 부분이 많다는 것은 무엇을 의미할까? 객체지향의 특징 중 하나인 **추상화**를 통해 간편화시킬 수 있다는 것이다.  

파일과 디렉터리를 추상화 시킨 Component 추상 클래스를 구현해준다. 공통부분인 자신의 이름을 가지는 변수를 가지고 있다.  

```java
abstract class Component {
	String name;
	
	public Component(String name) {
		this.name = name;
	}
}
```

다음으로 이를 상속받는 파일(Leaf)과 디렉터리(Composite) 클래스를 구현해준다.  

```java
public class File extends Component {
	public File(String name) {
		super(name);
	}
}
import java.util.ArrayList;
import java.util.List;

public class Directory extends Component {
	List<Component> list;
	
	public Directory(String name) {
		super(name);
		list = new ArrayList<>();
	}
	
	public void addComponent(Component component) {
		list.add(component);
	}
}
```

이전과 달리 Component라는 타입의 리스트만 있으며 추가하는 메서드도 한 개만 있으면 된다.  

Main 클래스는 다음과 같다.  

```java
public class Main {
	public static void main(String[] args) {
		Directory root = new Directory("root");
		Directory users = new Directory("users");
		Directory administrator = new Directory("administrator");
		Directory download = new Directory("download");
		File A_img = new File("A.img");
		File B_img = new File("B.img");
		
		root.addComponent(users);
			users.addComponent(administrator);
				administrator.addComponent(download);
					download.addComponent(A_img);
					download.addComponent(B_img);		
	}
}
```

Component 타입으로 추상화를 통해 한 가지 메서드로도 원하는 대로 잘 동작하는 것을 확인할 수 있다.  

컴포지트 패턴은 단일 객체와 복합 객체를 각각 별도로 구현하지 않아도 되는 장점이 있다. 또한, 객체가 모두 같은 타입으로 취급되기 때문에 새로운 클래스를 추가하는데 용이하다.  

하지만 추상화를 통해 구현이 간단해진 만큼 객체들을 구분하기가 어렵고 구현할 수 있는 기능들이 제한적이기  구분되기 힘들고 아무래도 구현하는데 제한되는 것들이 많은 단점이 존재한다.  
