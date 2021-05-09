---
title:  "[Java] final, finally, finalize"
excerpt: "[Java] final, finally, finalize의 차이를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-05-09T18:35:00
---

Java에는 final 접두사가 들어가는 final , finally, finalize, 3가지 용어가 존재한다.  

비슷한 이름 때문에 헷갈릴 수도 있는 이들에 대해서 한번 정리해보도록 하자.  


<br>
<h1>final</h1>  

final은 **변경 불가능하도록 해주는 기능**으로 변수, 메서드, 클래스에 적용할 수 있다.  

![Java  final, finally, finalize](https://user-images.githubusercontent.com/53072057/117568888-d9124600-b0fd-11eb-8119-9103855e55e8.JPG)  

<br>
<br>
<h1>finally</h1>  

finally는 try-catch 블록에 사용되는 기능으로 **무조건 실행되는 블록을 정의해 주는 기능**이다.  

try-catch 블록은 try 문을 실행하다가 Exception이 발생하게 되면 즉시 catch 문이 실행되기 때문에 남은 try 문은 실행될 수 없다.  

```java
BufferedReader br = null;
try {
	br = new BufferedReader(new InputStreamReader(System.in));	
	//br 객체를 사용
    //만약 Exception이 발생하면 뒤의 내용들을 실행될 수 없음
    //do Something
	br.close();
} catch (Exception e) {
	//Exception 처리
}
​```

하지만 finally 문을 사용하면 finally 구문에 있는 내용은 Exception이 발생 유무에 상관없이 무조건 실행된다.(단, try-catch 블록이 실행되는 도중에 JVM이 종료되는 경우에는 finally 문이 실행되지 않는다)  

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

<br>
<br>
<h1>finalize</h1>  

finalize는 Object 클래스의 메서드로 **메모리를 관리하기 위해 더 이상 참조되지 않는 객체를 GC(Garbage Collector)에 의해 정리될 때 호출되는 종료자 메서드**이다.  

![Java  final, finally, finalize1](https://user-images.githubusercontent.com/53072057/117568890-da437300-b0fd-11eb-9db7-e1aad832b396.JPG)  

Object 클래스의 메서드이기 때문에 커스텀 한 클래스에 오버라이딩하여 해당 클래스의 객체가 GC에 의해 정리될 때 특정 동작을 수행하도록 할 수 있다.  

간단하게 10개의 Test 객체를 생성하여 참조될 수 없도록 하여 GC를 수행해보자.  

```java
class Test {
	
	int idx;
	
	public Test(int idx) {
		this.idx = idx;
	}
	
	@Override
	protected void finalize() throws Throwable {
		System.out.println(getClass() + " "  + idx + " finalize() 메서드 실행");
	}
}

public class Main {
	
	public static void main(String[] args) {
		Test test;
		for (int i = 1; i <= 10; ++i) {
			test = new Test(i);
		}
		test = null;

		System.gc(); //GC 수행
	}
}
```

![Java  final, finally, finalize2](https://user-images.githubusercontent.com/53072057/117568891-da437300-b0fd-11eb-9ef1-3b9f35b22890.JPG)  

결과를 보면 참조되지 않는 Test 객체들이 GC에 의해 메모리에서 정리당하면서 finalize() 메서드를 호출하는 것을 확인할 수 있다.  

참고로 **제거되는 객체의 순서는 무작위이며 메모리의 상황에 따라 일부 객체만 GC에 의해 수행될 수도 있다**.  
