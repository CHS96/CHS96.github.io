---
title:  "플라이웨이트 패턴(Flyweight Pattern)"
excerpt: "플라이웨이트 패턴(Flyweight Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-05-13T18:35:00
---

플라이웨이트 패턴(Flyweight Pattern)은 **구조(Structural) 패턴 중 하나로써 인스턴스가 필요할 때마다 매번 생성하는 것이 아니고 가능한 한 공유해서 사용함으로써 메모리를 절약하는 패턴**이다.  

간단히 얘기하면 사용하려고 하는 인스턴스가 이전에 생성하여 존재한다면 인스턴스를 생성하지 말고 가져다 써서 메모리를 절약하겠다는 의미이다.  

플라이웨이트 패턴을 사용하면 다수의 유사 객체를 생성하거나 조작할 때 유용하게 사용할 수 있다.  

String 객체를 생성하는 리터럴 방식의 **String Constant Pool**이 대표적인 플라이웨이트 패턴을 적용한 예시이다.  

![플라이웨이트](https://user-images.githubusercontent.com/53072057/118122948-8783f700-b42e-11eb-9cde-6d77aefa4c4f.JPG)  

플라이웨이트 패턴의 구조는 다음과 같다.  

![플라이웨이트1](https://user-images.githubusercontent.com/53072057/118122956-88b52400-b42e-11eb-9ed2-a53b2d1037a8.JPG)  

* FlyweigtFactory : Flyweight 객체를 생성 및 공유해 주는 역할을 수행하는 클래스  

* Flyweight : 공유에 사용될 객체들을 나타내는 인터페이스  

* ConcreteFlyweight : Flyweight 인터페이스를 구현한 실제로 공유에 사용될 객체들을 나타내는 클래스  

*****

Java의 String Constant Pool이 플라이웨이트 패턴을 적용한 예시라고 설명하였다. 이를 간단하게 플라이웨이트 패턴을 적용하여 구현해보도록 하자.  

먼저 **Flyweight** 인터페이스이다. 다양한 ConcreteFlyweight가 필요 없기 때문에 따로 구현해 줄 것은 없고 형식상 그냥 구현만 하였다.  

```java
public interface Flyweight {
       //do Something
}
```

다음으로 **ConcreteFlyweight** 클래스이다. 사실상 StringConstantPool은 String Data만 표현할 것이기 때문에 굳이 인터페이스를 거치지 않고 하나의 ConcreteFlyweight 클래스를 구현하면 된다.  

리터럴 방식으로 표현하는 String 객체를 하나 가지는 StringLiteral 클래스를 구현해 준다.  

```java
public class StringLiteral implements Flyweight {

	private String data;
	
	public StringLiteral(String data) {
		this.data = data;
	}

	public String getData() {
		return data;
	}

	public void setData(String data) {
		this.data = data;
	}
}
```

마지막으로 StringLiteral 객체를 관리해 주는 **FlyweightFactory** 클래스이다. 해당 객체가 존재하는지 빠르게 탐색하기 위해 Map 자료구조로 Flyweight 객체를 관리해 준다.  

key 값으로 Map에서 해당 객체가 존재하면 그 객체를 반환해 주고 존재하지 않는다면 새로운 StringLiteral 객체를 생성하여 반환해 준다.  

이처럼 **객체가 필요할 때만 생성하고, 존재하는 객체를 공유함으로써 메모리를 절약할 수 있게 된다.**  

```java
import java.util.HashMap;
import java.util.Map;

public class StringConstantPool {
	
	Map<String, Flyweight> pool;
	
	public StringConstantPool() {
		pool = new HashMap<>();
	}
	
	public Flyweight getFlyweight(String key) {
		Flyweight flyweight = pool.get(key);
		
		if (flyweight == null) {
			flyweight = new StringLiteral(key);
			pool.put(key, flyweight);
		}
		
		return flyweight;
	}
}
```

**Client** 클래스에서 다음과 같이 여러 개의 "Hello", "Hello World", "Hello world" String 객체를 생성하여 객체의 주소를 출력해보면 문자열이 같은 객체들은 똑같은 주소를 가지는 것을 확인할 수 있다.  

```java
public class Client {

	public static void main(String[] args) {
		StringConstantPool stringConstantPool = new StringConstantPool();
		System.out.println(stringConstantPool.getFlyweight("Hello"));
		System.out.println(stringConstantPool.getFlyweight("Hello World"));
		System.out.println(stringConstantPool.getFlyweight("Hello"));
		System.out.println(stringConstantPool.getFlyweight("Hello"));
		System.out.println(stringConstantPool.getFlyweight("Hello world"));
		System.out.println(stringConstantPool.getFlyweight("Hello World"));	
	}
}
```

![플라이웨이트2](https://user-images.githubusercontent.com/53072057/118122958-88b52400-b42e-11eb-9f4a-b56598e5c707.JPG)  
