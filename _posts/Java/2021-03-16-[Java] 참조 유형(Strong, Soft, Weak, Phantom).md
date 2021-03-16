---
title:  "[Java] 참조 유형(Strong, Soft, Weak, Phantom)"
excerpt: "[Java] 참조 유형(Strong, Soft, Weak, Phantom)을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-03-16T18:35:00
---

Java는 모든 것들이 객체로 이루어져 있다고 봐도 무방하다. 또한 객체들은 new 연산을 통해 Heap 영역에 생성되기 때문에 이들은 GC(Garbage Collection) 대상이 될 수 있다.  

Java에선 메모리가 부족하면 JVM의 Garbage Collector가 스택 영역과 힙 영역을 탐색하면서 스택 영역의 변수가 더 이상 참조하지 않는 힙 영역의 객체들에 대해 GC를 수행해 줌으로써 메모리를 관리한다.  

하지만 개발자가 객체의 참조 강도를 약하게 만들어 GC 대상이 될 수 있도록 만들어 JVM의 메모리를 어느 정도 관리할 수도 있다.  

객체의 참조 강도를 조절할 수 있는데 Java의 참조 유형은 크게 Strong, Soft, Weak, Phantom 4가지가 존재한다.  

![Java  참조 유형(Strong, Soft, Weak, Phantom)](https://user-images.githubusercontent.com/53072057/111254554-af3a2700-8658-11eb-961b-eaf9afab6a83.JPG)  

그렇다면 각 유형들에 대해 하나씩 살펴보자.  


<h2>[ Strong Reference ]</h2>  

Java에서 가장 기본적인 참조 유형으로 new 연산을 통해 객체를 생성하는 방법이다.  

스택 영역의 strongRef 변수가 힙 영역의 객체에 대한 참조를 가지고 있는 한, strongRef 객체는 절대로 GC의 대상이 될 수 없다.  

```java
public class Main {

	static class StrongReference {
		/*
		 * StringReference 코드
		 */
	}

	public static void main(String[] args) {
		StrongReference strongRef = new StrongReference();
		System.out.println("GC 실행 전 : " + strongRef);
		System.gc();
		System.out.println("GC 실행 후 : " + strongRef);
	}
}
```

![Java  참조 유형(Strong, Soft, Weak, Phantom)1](https://user-images.githubusercontent.com/53072057/111254557-b06b5400-8658-11eb-994f-c7659a2fa0d0.JPG)  

GC를 실행하였지만 strongRef는 Strong Reference이기 때문에 제거되지 않은 것을 확인할 수 있다.  
​
만약 Strong 참조 유형의 객체를 GC의 대상이 되도록 하려면 strongRef 변수에게 Null을 할당하여 기존의 객체에 접근할 수 없는 Unreachable Object로 변경해야 한다.  

```java
strongRef = null; //strongRef의 기존 객체에 접근할 수 없도록 Unreachable Object로 변경
                  //이제 기존 객체는 GC의 대상이 될 수 있음
```


<h2>[ Soft Reference ]</h2>  

대상 객체를 참조하는 경우가 SoftReference 객체만 존재하는 경우 GC 대상이 될 수 있다. 무조건 제거되는 것은 아니며 JVM의 메모리가 부족한 경우(Out of Memory)에만 제거된다.  

```java
import java.lang.ref.SoftReference;

public class Main {

	static class StrongReference {
		/*
		 * StringReference 코드
		 */
	}
	
	public static void main(String[] args) {
		StrongReference strongRef = new StrongReference();
		SoftReference<StrongReference> softRef = new SoftReference<>(strongRef);//strongRef 객체를 Soft 참조 유형으로 변경

		strongRef = null; //strongRef의 기존 객체에 접근할 수 없도록 Unreachable Object로 변경
		
		System.out.println("GC 실행 전 : " + softRef.get());
		System.gc();
		System.out.println("GC 실행 후 : " + softRef.get());
	}
}
```

![Java  참조 유형(Strong, Soft, Weak, Phantom)2](https://user-images.githubusercontent.com/53072057/111254559-b103ea80-8658-11eb-914b-c9c89c8acf69.JPG)  

GC를 수행하였지만 JVM 메모리가 넉넉하기 때문에 Soft Reference인 softRef는 제거되지 않은 것을 확인할 수 있다.  

get() 메서드는 자신이 참조하고 있는 객체를 반환하는 메서드로 여기선 StrongReference 타입인 strongRef를 반환한다. 만약 GC에 의해 제거되었다면 null을 반환한다.  

![Java  참조 유형(Strong, Soft, Weak, Phantom)3](https://user-images.githubusercontent.com/53072057/111254560-b103ea80-8658-11eb-9d2a-72000d6f0c31.JPG)  


<h2>[ Weak Reference ]</h2>  

대상 객체를 참조하는 경우가 WeakReference 객체만 존재하는 경우 GC 대상이 되며 다음 GC 실행 시 무조건 힙 영역에서 제거된다.  

```java
import java.lang.ref.WeakReference;

public class Main {

	static class StrongReference {
		/*
		 * StringReference 코드
		 */
	}
	
	public static void main(String[] args) {
		StrongReference strongRef = new StrongReference();
		WeakReference<StrongReference> weakRef = new WeakReference<>(strongRef);//strongRef 객체를 Weak 참조 유형으로 변경
		
		strongRef = null; //strongRef의 기존 객체에 접근할 수 없도록 Unreachable Object로 변경
		
		System.out.println("GC 실행 전 : " + weakRef.get());
		System.gc();
		System.out.println("GC 실행 후 : " + weakRef.get());
	}
}
```

![Java  참조 유형(Strong, Soft, Weak, Phantom)4](https://user-images.githubusercontent.com/53072057/111254561-b19c8100-8658-11eb-9e32-808ff7b64f84.JPG)  

GC가 수행되면 JVM의 메모리에 상관없이 GC에 의해 제거된 것을 확인할 수 있다.  


<h2>[ Phantom Reference ]</h2>  

Phantom Reference는 가장 GC 대상이 될 확률이 높은 유형이다. Phantom Reference는 다른 Reference와 다르게 생성자에서 무조건 ReferenceQueue를 받는다.   

```java
import java.lang.ref.PhantomReference;
import java.lang.ref.ReferenceQueue;

public class Main {

	static class StrongReference {
		/*
		 * StringReference 코드
		 */
	}

	public static void main(String[] args) throws InterruptedException {
		StrongReference strongRef = new StrongReference();
		ReferenceQueue<StrongReference> refQueue = new ReferenceQueue<>();
		PhantomReference<StrongReference> phantomRef = new PhantomReference<>(strongRef, refQueue);

		System.out.println("GC 실행 전 : " + phantomRef.get());
		System.out.println(phantomRef.enqueue());
		System.gc();
		System.out.println("GC 실행 후 : " + phantomRef.get());
		System.out.println(phantomRef.enqueue());
	}
}
```

![Java  참조 유형(Strong, Soft, Weak, Phantom)5](https://user-images.githubusercontent.com/53072057/111254564-b19c8100-8658-11eb-9fdd-fff1ac1569f8.JPG)  

GC가 수행되기 전 finalize() 호출 후 메모리에서 정리된다고 한다. (내부적으로 ReferenceQueue에서 유지하고 있지만 GC가 수행되면 ReferenceQueue에서도 제거되는 것 같다.)  

또한, Phantom은 다른 참조 유형과 다르게 get() 메서드를 통해 접근하게 되면 항상 null이 반환된다. 즉, 접근을 할 수 없다는 것을 의미한다.  

![Java  참조 유형(Strong, Soft, Weak, Phantom)6](https://user-images.githubusercontent.com/53072057/111254566-b2351780-8658-11eb-8629-1bd0eb30d6ed.JPG)  

Phantom Reference는 잘 사용되지 않고, 어려운 개념이라 그냥 GC 대상이 될 확률이 가장 높은 참조 유형이라고만 알고 넘어가야겠다.  



<h2>[ Reference ]</h2>  
* <https://lion-king.tistory.com/entry/Java-%EC%B0%B8%EC%A1%B0-%EC%9C%A0%ED%98%95-Strong-Reference-Soft-Reference-Weak-Reference-Phantom-References>  
* <https://dev-ahn.tistory.com/117>  
