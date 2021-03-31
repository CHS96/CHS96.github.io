---
title:  "[Java] try-with-resources : 자원을 할당하는 방법"
excerpt: "[Java] try-with-resources : 자원을 할당하는 방법을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-03-31T18:35:00
---

Java에선 Stream이나 DB Connection처럼 다양한 자원을 외부에서 사용할 수 있다.  

하지만 **어떤 언어든지 간에 자원을 사용한 후 더 이상 사용할 필요가 없다면 자원의 효율성을 위해 반환해 줘야 한다**.  

예를 들어 다음과 같은 stream 하나를 사용했다고 하자. stream은 객체이기 때문에 다양한 예외 처리를 위해 보통 try-catch 문으로 감싸서 사용한다.  

```java
BufferedReader br = null;
try {
	br = new BufferedReader(new InputStreamReader(System.in));	
	//br 객체를 사용
	br.close();
} catch (Exception e) {
	//Exception 처리
}
```

하지만 위 코드는 문제가 존재한다. try-catch 문은 try 문을 실행하다가 Exception이 발생하면 즉시 catch 문으로 넘어가기 때문에 br.close()가 무조건 실행된다는 것을 보장해 줄 수 없다.  

다음과 같이 br 객체를 할당하지 않은 상태에서 사용하게 되면 br.close()가 실행되지 않는 것을 확인할 수 있다.  

```java
BufferedReader br = null;
try {
	br.readLine(); //null인 상태에서 사용 => NullPointerException
	System.out.println("br 객체 자원 반환");	
	br.close();
} catch (Exception e) {
	//Exception 처리
    System.out.println("Exception 발생");
}
```

![try-with-resources](https://user-images.githubusercontent.com/53072057/113092755-98cfc600-9229-11eb-8a77-dd020909cebb.JPG)  

이를 해결해 주기 위해 **try-catch-finally** 문이 존재한다. try-catch 문과 비슷하지만 어떠한 경우라도 finally 블록 내부의 코드는 무조건 실행된다.  

즉, 위의 코드에 try-catch-finally 문을 적용하면 항상 br.close()가 실행되도록 보장해 줄 수 있다.  

```java
BufferedReader br = null;
try {
	br = null;
	br.readLine();
} catch (Exception e) {
	//Exception 처리
    System.out.println("Exception 발생");
} finally {
	System.out.println("br 객체 자원 반환");	
	br.close();
}
```

하지만 이 코드를 실행해보면 동작하지 않는다.  

왜냐하면 close() 메서드가 **Checked Exception**인 IOException을 던지기 때문이다. Checked Exception은 컴파일 시 발생하는 예외이기 때문에 반드시 처리해 줘야 한다.  

![try-with-resources1](https://user-images.githubusercontent.com/53072057/113092758-99685c80-9229-11eb-9a5c-42effbb70f60.JPG)  

따라서 throws나 try-catch 문을 통해 예외 처리를 해줘야 한다.  

```java
BufferedReader br = null;
try {
    br = new BufferedReader(new InputStreamReader(System.in));	
    //br 객체를 사용
} catch (Exception e) {
	//Exception 처리
    System.out.println("Exception 발생");
} finally {
	try {
		System.out.println("br 객체 자원 반환");	
		br.close();
	} catch (IOException e) {
		e.printStackTrace();
	}
}
```

![try-with-resources2](https://user-images.githubusercontent.com/53072057/113092762-9a00f300-9229-11eb-8ed5-16ef08ebf2db.JPG)  

이제 Exception이 발생하여도 finally 구문을 통해 br 객체를 반환해 주는 것이 보장되었다. 하지만 여기서도 문제가 발생할 수 있는 요소가 하나 존재한다.  

바로 finally 구문의 try-catch 문이 실행되었는데 br 객체가 null인 경우이다. 즉, 또다시 NullPointerException이 발생할 수 있기 때문에 예외 처리를 한 번 더 해줘야 한다.  

```java
BufferedReader br = null;
try {
    br = new BufferedReader(new InputStreamReader(System.in));	
    //br 객체를 사용
} catch (Exception e) {
	//Exception 처리
    System.out.println("Exception 발생");
} finally {
	try {
	    if (br != null) { //﻿NullPointerException을 방지
			System.out.println("br 객체 자원 반환");	
			br.close();
		}
	} catch (IOException e) {
		e.printStackTrace();
	}
}
```


이제는 무조건 br 객체를 사용하고 난 후 안전하게 반환해 줄 수 있게 되었다.  

하지만 위의 코드는 굉장히 가독성이 떨어진다. try-catch 문이 2개나 중첩되었으며 내부에서 또 if 문을 사용해서 예외를 처리해 주기 때문에 굉장히 복잡하고 복잡한 만큼 실수하기가 쉽다는 단점이 존재한다.  

이를 위해 Java7부터는 자원을 쉽게 반환할 수 있도록 **try-with-resources**라는 문법을 지원해 준다.  

try-with-resources는 간단하게 try에 자원 객체를 전달한 후 try 구문이 끝나면 자동으로 자원을 반환해 주는 기능이다. 즉, finally 구문이나 따로 close()를 해줄 필요가 없다는 것을 의미한다.  

사용하는 방법은 try 옆의 () 안에 사용하는 자원을 전달해 주면 된다.  

```java
try (사용하는 자원) {
    //자원을 사용
} catch (Exception e) {
    e.printStackTrace();
}
```

즉, 위의 코드는 다음과 같이 굉장히 간단하게 구현할 수 있다.  

```java
try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in));){
    //br 객체를 사용
} catch (Exception e) {
	//Exception 처리
    System.out.println("Exception 발생");
}
```

() 내부를 보면 알겠지만 ';'이 사용되는 것을 볼 수 있다. 이는 내부에 여러 문장이 들어갈 수 있다는 것을 뜻한다. 즉, **여러 개의 자원을 () 안에 넣을 수 있다는 것을 의미한다**.  

```java
try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	 BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));){
     //br, bw 객체를 사용
} catch (Exception e) {
	//Exception 처리
    System.out.println("Exception 발생");
}
```

다만, () 내부에 들어갈 수 있는 객체는 **AutoCloseable** 인터페이스를 구현한 객체만 들어갈 수 있다.  

AutoCloseable 인터페이스는 이름 그대로 자동적으로 close를 해주는 인터페이스이다. close()라는 메서드 하나만을 가지는 인터페이스로 살펴보면 try-catch-resources 구문으로 관리되는 객체에서 자동적으로 close() 메서드가 호출된다고 설명하고 있다.  

![try-with-resources3](https://user-images.githubusercontent.com/53072057/113092763-9a998980-9229-11eb-81bc-e6fe17608460.JPG)  

BufferedReader 클래스도 타고 올라가면 AutoCloseable 인터페이스를 구현하였기 때문에 사용할 수 있다.  

![try-with-resources4](https://user-images.githubusercontent.com/53072057/113092764-9a998980-9229-11eb-95ba-a9025adff278.JPG)  

AutoCloseable 인터페이스를 구현만 한다면 내가 만든 클래스도 try-catch-resource 구문을 사용하여 자동으로 자원을 반환하도록 해줄 수 있다.  

```java
class Resource implements AutoCloseable {

	public Resource() {
		System.out.println("Resource class Object 생성");
	}
	
	@Override
	public void close() throws Exception {
		System.out.println("AutoCloseable close() 호출");
	}
}

public class Main {
	
	public static void main(String[] args) {
    	try (Resource resource = new Resource()){
    		//resource 객체를 사용
		} catch (Exception e) {
			//Exception 처리
    		System.out.println("Exception 발생");
		}
	}
}
```

![try-with-resources5](https://user-images.githubusercontent.com/53072057/113092766-9b322000-9229-11eb-8c13-1c081cee9ddb.JPG)  

try-catch-resources를 잘 활용하면 자원을 안전하게 반환해 주면서도 코드를 간결하게 구현하여 가독성을 높일 수 있는 장점이 있다.  




<h2>[ Reference ]</h2>  
* <https://multifrontgarden.tistory.com/192>  
* <https://ryan-han.com/post/java/try_with_resources/>  
