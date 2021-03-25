---
title:  "[Java] 향상된 for 문 : for-each"
excerpt: "[Java] 향상된 for 문 : for-each을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-03-25T18:35:00
---

Java5에서부터 향상된 for 문이라고 부르는 **for-each 문**이 도입되었다. 일반적인 for 문과 거의 비슷하지만 내부적으로 조금 다르게 순회하는 방법이다.  

for-each 문의 형태는 다음과 같다.  

```java
for (type var: iterate) {
    body-of-loop
}
```

**type**은 int, String 등 변수 타입을 의미하며 **iterate**는 순환할 자료구조를 의미하며 Array나 Collection 등의 자료구조를 사용할 수 있다.

예를 들어 String 형태의 변수를 가지는 리스트가 있을 경우 이를 순회할 때 다음과 같이 사용할 수 있다.  

```java
List<String> list = Arrays.asList("A","B","C","D","E");

---------- 기존의 for 문 --------------
for (int i = 0; i < list.size(); ++i) {
	String var = list.get(i);
}

---------- 향상된 for 문 --------------
for (String var : list) {
	//var를 사용해주면 됨, var은 list.get(i)와 같은 역
}
```
  
좀 더 자세히 말하면 **iterate는 iterable이 가능한 자료구조들을 사용할 수 있다**. Iterable 인터페이스를 살펴보면 for-each 문에 활용할 수 있다고 적힌 것을 볼 수 있다.  

![for-each](https://user-images.githubusercontent.com/53072057/112416840-d2a45680-8d69-11eb-9180-493e5747b27b.JPG)  

즉, 다음과 같이 Array나 Collection이 아니더라도 Iterable 객체로 for-each 문을 사용할 수 있다는 의미이다.  

```java
Iterable<String> iter = Arrays.asList("A","B","C","D","E");
		
for (String var : iter) {
	//var를 사용해주면 됨, var은 list.get(i)와 같은 역
}		
```

for-each 문이 가독성이 좋고 짧기 때문에 사용하기 간단해 보인다. 그렇다면 for-each 문만 사용하면 되지 않을까?라는 의문이 들 수 있지만 두 방식은 장단점이 존재한다.  

<br>

첫 번째는 **인덱스를 사용할 수 없다는 것이다**. 기존의 for 문 방식은 list에서 i라는 인덱스를 사용하여 접근하게 된다. 하지만 향상된 for 문 방식은 var 이외에는 사용할 수 있는 도구가 존재하지 않는다.  

물론 후자의 경우에도 별도의 인덱스를 하나 선언해 주면 전자의 방식과 동일하게 사용할 수 있다.  

```java
int i = 0;

for (String var : list) {
	//var를 사용해주면 됨, var은 list.get(i)와 같은 역할
    i++; //인덱스 증가
}
```

하지만 나는 전자의 방식이 더 가독성이 높은 것 같아서 인덱스를 사용해야 하는 경우는 전자를 선호한다.  

​<br>

두 번째는 **성능의 차이다**. 정확히는 순환할 자료구조에 따른 성능의 차이이다.  

성능의 차이를 알기 위해선 for-each 문이 내부에서 어떤 원리로 자료구조를 순환하는지를 알아야 한다.  

for-each 문은 Iterable이 가능한 자료구조를 순환한다고 하였다. Iterable이 인터페이스는 Iterator라는 인터페이스를 사용하여 내부를 순환한다.  

Iterator 인터페이스는 **반복자라고 불리며 자바의 컬렉션 프레임워크에 저장된 요소를 읽어오는 방법을 표준화하기 위한 역할로 사용된다​**.  

Collection 인터페이스는 Iterable 인터페이스를 상속받고 있기 때문에 Collection 인터페이스를 상속받는 하위 컬렉션들은 모두 Iterator를 사용할 수 있다.  

![for-each1](https://user-images.githubusercontent.com/53072057/112416842-d3d58380-8d69-11eb-992c-3506c2db6559.JPG)  

즉, 위의 for-each 문을 사용하여 String 타입을 가지는 List를 순환하는 방식은 실제로 다음과 같이 동작된다.  

```java
for (String var : list) {
	//var를 사용해주면 됨, var은 list.get(i)와 같은 역할
}
-------------------------------------------------------
Iterator<String> it = list.iterator();
		
while (it.hasNext()) //다음 객체가 존재하면
{
	String var = (String) it.next(); //다음 객체를 반환
}
```

내부 동작원리를 보면 어떤 자료구조에 따라 성능 차이가 발생할 수 있다는 것을 어느 정도 짐작할 수 있다. List를 예시로 한번 보도록 하자.  

List 인터페이스를 구현한 대표적인 Collection은 ArrayList와 LinkedList가 존재한다. 자료구조를 공부했다면 이들의 차이는 잘 알고 있을 거라고 생각한다.  

기존의 for 문 방식은 list의 원소를 접근할 때 get() 메서드를 사용하여 접근하였다. 그렇다면 ArrayList와 LinkedList의 get() 메서드 시간 복잡도는 어떻게 될까?  

ArrayList는 해당 인덱스로 바로 접근할 수 있기 때문에 **O(1)**의 시간 복잡도를 가지는 반면, LinkedList는 매번 첫 번째의 Head에서 해당 인덱스까지 이동하기 때문에 **O(N)**의 시간 복잡도를 가진다.  

즉, 다음과 같은 상황에서 List가 LinkedList라면 O(N)이 아닌 **O(N^2)**의 시간 복잡도를 가지게 된다.  

```java
for (int i = 0; i < list.size(); ++i) {
	String var = list.get(i);
}
```

하지만 for-each 문 방식은 내부에서 Iterator 인터페이스를 사용한다고 하였다. Iterator는 cursor라는 변수를 통해 자신이 순환하고 있는 위치를 기억할 수 있기 때문에 매번 처음부터 순환할 필요가 없다.  

즉, LinkedList에서는 for-each 문을 사용하면 모든 리스트의 원소를 순환하는데 **O(N)**의 시간 복잡도를 가진다.  

<br>

하지만 반대로 ArrayList인 경우는 평균적으로 기존의 for 문 방식이 더 빠르다. for-each 문 방식은 **Iterator라는 객체를 만들기 때문에 get()보다 next() 비용이 더 크기 때문이다**. 하지만 사실 큰 차이는 없다.  

실제로 10만 개의 데이터를 ArrayList와 LinkedList에 삽입하여 각 방식들을 테스트해보면 ArrayList는 별 차이가 없지만 LinkedList에서는 두 방식의 차이가 확연히 드러나는 것을 확인할 수 있다.  

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class Main {

	public static void main(String[] args) {
		List<Integer> arrayList = new ArrayList<>();
		List<Integer> linkedList = new LinkedList<>();
		for (int i = 1; i <= 100000; ++i) {
			arrayList.add(i);
			linkedList.add(i);
		}
		
		long start = System.currentTimeMillis();
		for (int i = 0, size = arrayList.size(); i < size; ++i) {
			arrayList.get(i);					
		}
		long end = System.currentTimeMillis();
		System.out.println("일반 for문 with ArrayList : " + (end - start) + "ms");
		
		start = System.currentTimeMillis();
		for (int v : arrayList) {}
		end = System.currentTimeMillis();
		System.out.println("향상된 for문 with ArrayList : " + (end - start) + "ms");
		
		System.out.println("=================================");
		
		start = System.currentTimeMillis();
		for (int i = 0,  size = linkedList.size(); i < size; ++i) {
			linkedList.get(i);
		}
		end = System.currentTimeMillis();
		System.out.println("일반 for문 with LinkedList : " + (end - start) + "ms");

		start = System.currentTimeMillis();
		for (int v : linkedList) {}
		end = System.currentTimeMillis();
		System.out.println("향상된 for문 with LinkedList : " + (end - start) + "ms");
	}
}
```

![for-each2](https://user-images.githubusercontent.com/53072057/112416843-d3d58380-8d69-11eb-9c3a-cfdb18f84111.JPG)  

이처럼 두 방식 중 어떤 방식이 더 빠르다고는 장담할 수 없다. 어떠한 자료구조를 순회할 것인지에 따라 다르기 때문이다.  

하지만 인덱스를 사용하지 않고 자료구조가 어떠한 방식이든 성능 차이가 없다면 for-each 문을 사용하는 것을 추천한다.  

인덱스를 사용할 필요가 없는 경우는 대부분 **단순히 자료구조를 순회하는 용도**일 것이다. 만약 기존의 for 문을 사용한다면 인덱스를 가지고 있기 때문에 실수로 자료구조의 데이터를 건드리는 경우가 발생할 수 있다.  

하지만 for-each 문은 인덱스가 없기 때문에 자료구조를 순회하는 도중 자료구조의 데이터를 건드릴 확률이 낮기 때문이다.  




<h2>[ Reference ]</h2>  
* <https://junghyungil.tistory.com/65>  
* <https://multifrontgarden.tistory.com/130>  
  
