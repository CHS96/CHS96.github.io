---
title:  "[Java] 초기화 블록(Initialization Block)"
excerpt: "[Java] 초기화 블록(Initialization Block)을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-04-06T18:35:00
---

Java에선 클래스 변수, 인스턴스의 변수의 초기화가 복잡하거나 공통된 부분이 많을 경우 **초기화 블록**을 사용하면 코드를 간결하게 할 수 있다.  

초기화 블록의 종류로는 **Static Block**과 **Instance Block**이 존재한다.  

![초기화블록](https://user-images.githubusercontent.com/53072057/113646571-08c7cb80-96c4-11eb-888f-d62f6683e6be.JPG)  

초기화 블록은 다음과 같이 클래스 내에 선언하여 사용하면 된다.  

```java
static {
	//Static Block
}
	
{
	//Instance Block
}
```

​

너무 간단한 내용이라서 다음 코드의 실행 결과를 보면 초기화 블록이 어떤 것인지 이해할 수 있을 것이다.  

```java
class I﻿nitializationBlock {
	
	static String staticBlock;
	String instanceBlock;
	
	static {
		//Static Block
		staticBlock = "클래스 로딩 시 한번만 실행";
		System.out.println(staticBlock);
	}

	{
		//Instance Block
		instanceBlock = "인스턴스 생성마다 실행";
		System.out.println(instanceBlock);
	}
}

public class Main {

	public static void main(String[] args) {
		I﻿nitializationBlock i﻿nitializationBlock1 = new I﻿nitializationBlock();
		I﻿nitializationBlock i﻿nitializationBlock2 = new I﻿nitializationBlock();
	}
}
```

![초기화블록1](https://user-images.githubusercontent.com/53072057/113646575-09606200-96c4-11eb-98b2-d2ec3e10cd06.JPG)  


앞서 Static Block은 클래스가 로딩 시 한 번만 실행되는 블록이고 Instance Block은 인스턴스가 생성될 때마다 실행되는 블록이라고 언급하였다.  

실행 결과를 보면 Main 클래스에서 InitializationBlock 인스턴스를 2개 생성하였지만 Static Block은 한 번만 실행되었고 Instance Block은 두 번 실행된 것을 확인할 수 있다.  




Static Block은 어떤 부분에 사용하는지 잘 모르겠지만 Instance Block은 인스턴스 생성 시마다 호출되는 것이기 때문에 인스턴스들에게 공통되는 부분을 중복 처리하지 말고 단순화시켜 줄 수 있다.  

예를 들어 Text라는 클래스에서 생성자마다 문자열을 출력하는 기능을 가진다고 하자.  

```java
class Text {
	String text;
	
	Text () { 
		System.out.println("Text 생성자 생성");
	}
	
	Text (String text) {
		System.out.println("Text 생성자 생성");
		this.text = text;
	}
	
}

public class Main {

	public static void main(String[] args) {
		Text text1 = new Text();
		Text text2 = new Text("Text");
	}
}
```

![초기화블록2](https://user-images.githubusercontent.com/53072057/113646576-09f8f880-96c4-11eb-8e6e-cad52bba55c8.JPG)  

똑같은 기능이 중복되기 때문에 생성자가 많아진다면 코드가 복잡해질 수 있다. 하지만 Instance Block을 사용하면 생성자가 아무리 많더라고 코드를 간결하게 작성할 수 있다.  

```java
class Text {
	String text;
	
	Text () {}
	
	Text (String text) {
		this.text = text;
	}
	
	{
		//Instance Block
		System.out.println("Text 생성자 생성");
	}
}

public class Main {

	public static void main(String[] args) {
		Text text1 = new Text();
		Text text2 = new Text("Text");
	}
}
```

![초기화블록3](https://user-images.githubusercontent.com/53072057/113646578-09f8f880-96c4-11eb-9471-c0d99cfabc46.JPG)  

두 코드의 실행 결과가 같은 것을 확인할 수 있다.  

![초기화블록4](https://user-images.githubusercontent.com/53072057/113646579-0a918f00-96c4-11eb-9274-0f85331437b3.JPG)  
