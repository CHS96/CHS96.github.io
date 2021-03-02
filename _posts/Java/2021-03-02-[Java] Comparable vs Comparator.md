---
title:  "[Java] Comparable vs Comparator"
excerpt: "[Java] Comparable vs Comparator를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-03-02T18:35:00
---

Java에서 배열이나 리스트의 원소들을 특정 기준으로 정렬하기 위해서 사용하는 방법은 **Comparable 인터페이스나 Comparator 인터페이스**를 사용하는 방법이 존재한다.  
​
이들은 비슷하면서도 다른 점이 존재하는 친구들이다. 한번 공부 및 정리해보도록 하자.  

우선 Comparable과 Comparator 인터페이스는 다음과 같이 구성되어 있다.  

```java
package java.lang;

public interface Comparable<T> {
    public int compareTo(T o);
}

--------------------------------------
package java.util;

@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
    boolean equals(Object obj);
}
```

두 인터페이스는 비슷하면서도 조금 다른 것을 확인할 수 있다. 둘 다 비교에 대한 int 값을 반환하는 공통점이 있지만 Comparable의 compareTo 메서드는 1개의 인자를 받는 반면 Comparator의 compare 메서드는 2개의 인자를 받는 차이가 있다.  

또한, Comparator 인터페이스는 @FunctionalInterface라는 어노테이션을 가지고 있다. 이는 함수형 인터페이스라는 의미로 Java 8에 추가된 람다식을 활용할 수 있다는 특징이 있다.  

그렇다면 이들은 각각 어떤 상황에서 사용되는 것일까?  

결론부터 말하면 다음과 같다.  

![Comparable vs Comparator](https://user-images.githubusercontent.com/53072057/109599639-e1c72880-7b5e-11eb-9357-1ecf86cf00e4.JPG)  

이게 무슨 말이냐면 Comparable는 그 객체를 정렬할 때 default 값으로 compareTo 메서드를 기준으로 정렬하겠다는 의미이며, Comparator는 default 값 말고 다른 기준으로 정렬할 때 사용하겠다는 의미이다.  

예를 들어 String 배열을 정렬한다고 해보자. 일반적으로 배열을 정렬할 때는 Arrays.sort(), 리스트를 정렬할 때는 Collections.sort() 메서드를 사용해서 정렬한다.  

```java
public class Main {
	public static void main(String args[] ) throws Exception {
		String[] name = {"Tom", "Libi", "Kite", "John"};

		Arrays.sort(name);
		
		for (String s : name) {
			System.out.print(s + " ");
		}
	}
}
```

우리는 현재 String에 대해 아무런 정렬 기준을 정의해주지 않았다. 하지만 위의 코드를 실행하면 name의 원소들이 오름차순 기준으로 정렬되어 있는 것을 확인할 수 있다.  

![Comparable vs Comparator1](https://user-images.githubusercontent.com/53072057/109599644-e2f85580-7b5e-11eb-94b8-9180541c0965.JPG)  

왜 정렬 기준을 정해주지 않았는데 에러가 발생하지 않고 정렬이 되는 것일까? 이는 String 클래스에서 Comparable 인터페이스의 compareTo 메서드를 오름차순으로 정렬해라고 정의해놨기 때문이다.  

![Comparable vs Comparator2](https://user-images.githubusercontent.com/53072057/109599645-e2f85580-7b5e-11eb-91a9-630a82aa1af3.JPG)  

![Comparable vs Comparator3](https://user-images.githubusercontent.com/53072057/109599646-e390ec00-7b5e-11eb-9fc6-a0ab87cb0bdb.JPG)  

즉, String 객체들은 정렬 기준을 default 값으로 오름차순 정렬을 기준으로 잡은 것이다.  

그렇다면 내림차순으로 정렬할 수 있는 방법은 없을까? 이를 가능하게 해주는 방법이 바로 Comparator 인터페이스의 compareTo 메서드를 구현해서 정렬 기준으로 넘겨주는 것이다.  

사실 String 클래스에는 내림차순으로 정렬해주는 기능이 이미 구현되어 있다. 다음과 같이 실행하면 우리는 내림차순으로 정렬된 name의 값을 확인할 수 있다.  

```java
Arrays.sort(name, Collections.reverseOrder());
```

![Comparable vs Comparator4](https://user-images.githubusercontent.com/53072057/109599647-e390ec00-7b5e-11eb-9f9a-2925753701de.JPG)  

Arrays.sort() 메서드는 두 번째 인자 값으로 정렬 기준을 받을 수 있다. 이때 정렬 기준은 Comparator 인터페이스를 의미한다.  

![Comparable vs Comparator5](https://user-images.githubusercontent.com/53072057/109599648-e4298280-7b5e-11eb-89b5-fa3db2708ba7.JPG)  

그렇다면 Collectiions.reverseOrder() 메서드를 한번 확인해보자.  

![Comparable vs Comparator6](https://user-images.githubusercontent.com/53072057/109599649-e4c21900-7b5e-11eb-8423-76c78ccb7427.JPG)  

reverseOrder() 메서드는 보시다시피 ReverseComparator 클래스의 REVERSE_ORDER를 반환하도록 구현되어 있다. ReverseComparator는 다음과 같이 구현되어 있다.  

![Comparable vs Comparator7](https://user-images.githubusercontent.com/53072057/109599650-e4c21900-7b5e-11eb-8d90-5a7f26d28412.JPG)  

다음과 같이 compare 메서드가 구현되어 있는 것을 확인할 수 있다. 근데 반환하는 부분을 보면 뭔가 이상하다고 생각이 들 수도 있다.  

분명히 Comparator의 메서드는 compareTo()가 아니라 compare()였는데 여기는 compareTo() 메서드의 결괏값을 반환하도록 구현되어 있다.  

사실 별 큰 의미는 없다. 그냥 이미 구현되어 있는 compareTo() 메서드를 활용해서 내림차순으로 정렬하겠다는 의미이다.  

즉, String 클래스에서 오름차순으로 정렬되도록 재정의한 compareTo()를 활용한다는 의미이다. 오름차순은 c1.compareTo(c2) 형태였다면 내림차순은 c2.compareTo(c1)인 반대로 비교한다.  

이처럼 Comparable 인터페이스는 별다른 기준 정렬을 주지 않아도 항상 compareTo() 메서드를 기준으로 정렬되도록 default 기준 정렬을 정해주는 친구이며, Comparator 인터페이스는 가끔씩 default 기준 정렬 말고 다른 기준으로 정렬하고 싶을 때 사용하는 친구이다.  

즉, Comparator 인터페이스는 일회성이 강한 정렬 기준을 사용할 때 주로 사용한다. 아까 Comparator 인터페이스는 @FunctionalInterface 어노테이션이 붙어있다고 하였다.  

이전에 람다를 공부할 때 람다는 익명 클래스에서부터 발전된 개념이라고 하였다. 혹시나 궁금하면 아래의 글을 참고하길 바란다.  

* <https://blog.naver.com/djena5078/222185903248>  

[![Comparable vs Comparator8](https://user-images.githubusercontent.com/53072057/109599653-e55aaf80-7b5e-11eb-8434-9a8d9d6eb27d.JPG)](https://blog.naver.com/djena5078/222185903248)  

즉, Comparator 인터페이스는 일회성이 강한 친구이기 때문에 익명 클래스로 간단하게 구현할 수 있으며 여기서 람다식을 통해 더 간결하게 구현할 수 있는 장점이 있다.  

다음과 같이 id와 name을 가지는 Member 클래스가 있다고 하자. Comparable 인터페이스를 통해 default 값으로 id를 오름차순으로 정렬하도록 하였다.  

```java
class Member implements Comparable<Member> {
	int id;
	String name;
	
	public Member(int id, String name) {
		this.id = id;
		this.name = name;
	}

	@Override
	public int compareTo(Member o) {
		return this.id - o.id;
	}
	
	@Override
	public String toString() {
		return "Member [id=" + id + ", name=" + name + "]";
	}	
}
```

만약 name을 오름차순 기준으로 정렬하고 싶다면 어떻게 해야 할까? 이는 Comparator 인터페이스를 구현해주면 된다.  

```java
public static Comparator<Member> comp = new Comparator<Member>() {
	@Override
	public int compare(Member o1, Member o2) {
		return o1.name.compareTo(o2.name);
	}
};
```

구현한 comp를 기준으로 정렬하면 default 값이 아닌 원하는 대로 name을 기준으로 오름차순 정렬된 것을 확인할 수 있다.  

```java
public class Main {
	public static void main(String args[] ) throws Exception {
		Member member1 = new Member(1, "John"); Member member2 = new Member(3, "Libi");
		Member member3 = new Member(2, "Tom"); Member member4 = new Member(4, "Jerry");
		
		List<Member> list = new ArrayList<>();
		list.add(member1); list.add(member2);
		list.add(member3); list.add(member4);
		
		Collections.sort(list, comp); //구현한 comp(name 기준 오름차순)로 정렬
		
		System.out.println(list);
	}
}
```

![Comparable vs Comparator9](https://user-images.githubusercontent.com/53072057/109599654-e55aaf80-7b5e-11eb-930d-19cdfb9f0415.JPG)  

하지만 익명 클래스를 통해 구현이 가능하다고 하였다. 즉, 다음과 같이 별도의 메서드로 빼지 않고 sort하는 부분에 구현하는 방법이 있다.  

```java
Collections.sort(list, new Comparator<Member>() {
	@Override
	public int compare(Member o1, Member o2) {
		return o1.name.compareTo(o2.name);
	}
});
```

또한, 위의 코드는 람다식을 활용할 수 있는 조건이 충족되기 때문에 더욱 간단하게 코드를 변경할 수 있다.  

```java
//람다식을 활용해서 name을 기준으로 오름차순 정렬
Collections.sort(list, (o1, o2) -> o1.name.compareTo(o2.name));
```

단 한 줄로 name을 기준으로 오름차순 정렬이 가능하도록 만들었다.  

![Comparable vs Comparator10](https://user-images.githubusercontent.com/53072057/109599656-e5f34600-7b5e-11eb-82c3-434bbac24da9.JPG)  
