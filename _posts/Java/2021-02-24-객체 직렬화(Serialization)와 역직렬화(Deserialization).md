---
title:  "객체 직렬화(Serialization)와 역직렬화(Deserialization)"
excerpt: "객체 직렬화(Serialization)와 역직렬화(Deserialization)를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-02-24T18:35:00
---

객체지향 언어인 Java는 프로그램의 모든 데이터들이 **객체**로 이루어져 있다고 봐도 무방하다.  

그렇다면 Java로 만든 프로그램의 데이터(객체)를 외부로 전송하려면 어떻게 해야 할까? 네트워크를 공부했다면 기본적으로 데이터인 객체 그 자체를 네트워크 상으로 전송할 수 없다는 것을 알 것이다. 이를 전송하기 위해선 객체 그 자체보단 조금 더 단순한 형태로 변환해야 할 것이다.  

Java의 I/O 처리는 정수, 문자열, 바이트 단위의 처리만 지원하기 때문에 복잡한 객체의 내용을 저장/복원하거나 네트워크 상으로 전송하기 위해서는 객체의 내용을 I/O가 처리할 수 있는 형태로 변환해 줘야 한다.  

Java에서 말하는 **객체 직렬화는 이처럼 Java의 객체를 외부로 저장/복원하거나 네트워크 상으로 전송할 수 있도록 바이트 형태로 변환하는 기술**을 의미한다.  

즉, 객체가 아무리 복잡하여도 직렬화를 통해 객체를 바이트 형태로 변환하여 외부로 전송할 수 있는 것이다.  

반대로 역직렬화는 **직렬화를 통해 변환된 바이트 형태를 다시 원상태인 객체로 변환시키는 기술**을 의미한다.  

직렬화의 특징은 다음과 같다.  

![객체 직렬화(Serialization)와 역직렬화(Deserialization)](https://user-images.githubusercontent.com/53072057/108942110-b7c2c180-7699-11eb-94c7-5c836c11a9b5.JPG)  

직렬화와 역직렬화의 의미는 대충 이해했을 것이다. 그렇다면 Java에서 직렬화 및 역직렬화하는 방법에 대해서 알아보자.  

기본적으로 클래스의 객체를 직렬화시키기 위해선 **Serializable 인터페이스**를 implements 해줘야 한다.  

```java
class Person implements Serializable {
	String name;
	int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
}
```

다만, Serializable 인터페이스를 implements 하여도 따로 구현해야할 내용은 없다. Serializable 인터페이스를 확인해보면 아무것도 없는 것을 확인할 수 있다.  

![객체 직렬화(Serialization)와 역직렬화(Deserialization)1](https://user-images.githubusercontent.com/53072057/108942112-b8f3ee80-7699-11eb-8bdd-ed5d6013b936.JPG)  

Seializable 인터페이스는 **현재 클래스의 객체가 직렬화가 제공되어야 함을 JVM에게 알려주는 역할만 수행**한다.  

그렇다면 Person 클래스의 객체를 직렬화를 통해 파일로 변환하여 저장해보자.  

객체 직렬화는 직렬화하고자 하는 객체에 직렬화를 수행해주는 **ObjectOutputStream**과 직렬화한 내용을 저장할  .txt 파일을 생성해주는 **FileOutputStream**을 통해 수행된다.  

다음과 같이 Serialization.txt 파일을 열어서 person 객체의 정보들을 직렬화시켜서 저장해준다.  

```java
public class Main {
	public static void main(String[] args) throws IOException {
		Person person = new Person("Libi", 26);
		
		try(ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("﻿Serialization.txt"))){
			out.writeObject(person);
		} catch (Exception e) {
		}
	}
}
```

수행하면 Serialization.txt에는 다음과 같이 변환된 바이트 형태의 내용이 저장되어 있다.  

![객체 직렬화(Serialization)와 역직렬화(Deserialization)2](https://user-images.githubusercontent.com/53072057/108942113-b98c8500-7699-11eb-8ee5-5c20d9bd5113.JPG)  

그렇다면 직렬화로 저장된 파일을 역직렬화를 통해 불러오도록 하자. 역직렬화도 직렬화와 비슷하게 수행된다.  

객체 역직렬화는 불러온 파일에 역직렬화를 수행하여 객체로 복수시켜주는 **ObjectInputStream**과 .txt 파일을 불러오는 **FileInputStream**을 통해 수행된다.  

```java
public class Main {
	public static void main(String[] args) throws IOException {
		Person person = null;
		
		try(ObjectInputStream in = new ObjectInputStream(new FileInputStream("﻿Serialization.txt"))){
			person = (Person) in.readObject();
		} catch (Exception e) {
		}
		
		System.out.println(person.toString());
	}
}
```

![객체 직렬화(Serialization)와 역직렬화(Deserialization)3](https://user-images.githubusercontent.com/53072057/108942116-b98c8500-7699-11eb-99f7-a1eb1ed282c5.JPG)  

이전에 직렬화를 수행하여 변환한 객체의 정보가 원래대로 복구된 것을 확인할 수 있다.  

현재 Person 클래스의 모든 정보를 직렬화를 통해 저장하였다. 만약 특정 정보를 저장하고 싶지 않다면 어떻게 해야 할까?  

이는 저장하고 싶지 않은 멤버에 **Transient** or **Static** 키워드를 붙이면 제외할 수 있다.  

```java
class Person implements Serializable {
	static String name;
	transient int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
}
```

![객체 직렬화(Serialization)와 역직렬화(Deserialization)4](https://user-images.githubusercontent.com/53072057/108942117-ba251b80-7699-11eb-92d3-2043fc05ae60.JPG)  

name과 age에 대한 정보가 저장되어 있지 않은 것을 확인할 수 있다. 역직렬화를 통해 해당 파일을 읽어보자.  

![객체 직렬화(Serialization)와 역직렬화(Deserialization)5](https://user-images.githubusercontent.com/53072057/108942119-ba251b80-7699-11eb-8931-d20b72f37f7f.JPG)  

아무런 정보가 없기 때문에 기본값으로 초기화되어 출력되는 것을 확인할 수 있다.  

역직렬화를 수행할 때 주의해야 할 점들이 몇 가지 존재한다.  

먼저 역직렬화를 수행하기 위해선 **직렬화환 클래스의 속성과 역직렬화를 수행하는 클래스 속성이 일치**​해야 한다고 하였다. 그렇다면 Java가 어떻게 두 클래스의 속성이 일치하는지 판단할 수 있을까?  

이는 **serialVersionUID 필드**를 통해 판단한다. 이는 Serializable 인터페이스에 구현된 것으로, 직렬화를 선언한 클래스를 컴파일할때 컴파일러가 클래스 구조 정보를 해시값으로 변환한 값을 자동으로 넣어준다.  

Default 값으로 클래스의 기본 해시값을 가진다.  

![객체 직렬화(Serialization)와 역직렬화(Deserialization)6](https://user-images.githubusercontent.com/53072057/108942121-babdb200-7699-11eb-96dd-8a155c08e807.JPG)  

즉, 이 값을 비교하여 두 클래스의 속성이 같은지를 판단해 준다.  

```java
class Person implements Serializable {
	private static final long serialVersionUID = 1L;
	
	String name;
	int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
}
```

또한, String → StringBuilder, int → long 등 잘못된 타입으로 받아들이면 Exception이 발생한다. 즉, 객체 직렬화는 **타입에 대해 상당히 엄격하다는 것**을 알 수 있다.  

마지막으로 간단한 객체의 내용도 직렬화를 수행하면 데이터뿐만 아니라 다른 구조 데이터 등의 정보도 포함되기 때문에 **데이터의 크기가 많이 커지는 단점**이 있다.  

따라서 웬만하면 내용 변경이 없을만한 클래스에 사용하는 것이 좋으며, 사용하더라도 장기 보관용으로는 적합하지 않다. 또한, 데이터 크기가 많이 차이나기 때문에 긴 만료 시간을 가지는 데이터는 JSON 등 다른 포맷을 사용하여 저장하는 것이 낫다.  

이러한 객체 직렬화는 Java에서 서블릿 세션(Servlet Session), 캐시(Cache), 자바 RMI(Remote Method Invocation) 등에서 사용된다.  


<h2>[ Reference ]</h2>  
* <https://codevang.tistory.com/164>  
* <https://nesoy.github.io/articles/2018-04/Java-Serialize>  