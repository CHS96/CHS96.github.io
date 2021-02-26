---
title:  "equals vs '==' & hashCode"
excerpt: "equals vs '==' & hashCode를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-02-26T18:35:00
---

Java에서는 두 개의 데이터가 같은지 비교할 때 equals() 메서드나 '==' 비교연산자를 사용한다. 이들의 차이에 대해서 한번 정리해보자.  

Java에는 크게 두 가지 데이터 타입이 존재한다.  

![equals vs '=='   hashCode](https://user-images.githubusercontent.com/53072057/109254755-02794080-7836-11eb-9b4f-4e8c616a5af3.JPG)  

원시 타입의 데이터는 '==' 비교연산자로만 두 데이터가 같은지 비교할 수 있으며 equals() 메서드는 사용할 수 없다. 왜냐하면 equals() 메서드는 Object 클래스의 메서드이기 때문에 원시 타입의 데이터는 사용할 수 없다.  

반면, 참조 타입의 데이터는 객체를 나타내기 때문에 '==' 비교연산자와 equals() 메서드 모두 사용이 가능하다.  

그렇다면 두 방법은 뭐가 다른 것일까?  

'==' 비교연산자는 **두 객체의 주소를 비교하여 동일한지 판단**한다.​​ 즉, 두 객체가 같은 주소를 가리키는 똑같은 객체일 경우에만 true를 반환한다.  

```java
public class Main {
	public static void main(String[] args) {
		String a = new String("A");
		String b = a;
		String c = new String("A");
		
		System.out.println(a == b);
		System.out.println(a == c);
	}
}
```

![equals vs '=='   hashCode1](https://user-images.githubusercontent.com/53072057/109254757-03aa6d80-7836-11eb-8a13-74c63fe1f62d.JPG)  

a와 b는 같은 주소를 가리키는 동일한 객체이기 때문에 true를 반환하였고 a와 c는 다른 객체이기 때문에 false를 반환한 것을 확인할 수 있다.  

그렇다면 equals() 메서드는 어떨까? 결론부터 말하면 equals() 메서드도 마찬가지로 **두 객체가 같은 주소를 가리키는 동일한 객체일 경우에만 true를 반환한다.**  

그 이유는 Object 클래스의 equals() 메서드가 다음과 같이 '==' 비교연산자를 통해 두 객체가 동일한지 확인하기 때문이다.  

![equals vs '=='   hashCode2](https://user-images.githubusercontent.com/53072057/109254759-03aa6d80-7836-11eb-945c-8d8d17b99572.JPG)  

그럼 두 개가 동일한 것이 아닌가?라고 의문이 들 수도 있다. 그렇다면 다음 코드의 실행 결과를 한번 보도록 하자.  

```java
public class Main {
	public static void main(String[] args) {
		String a = new String("A");
		String b = a;
		String c = new String("A");
		
		System.out.println(a == b);
		System.out.println(a.equals(b));
		
		System.out.println(a == c);
		System.out.println(a.equals(c));
	}
}
```

![equals vs '=='   hashCode3](https://user-images.githubusercontent.com/53072057/109254760-04430400-7836-11eb-9204-46aa18ae8c11.JPG)  

결과를 보면 이상한 점을 발견할 수 있다. 분명히 equals 메서드나 '==' 비교연산자 모두 두 객체가 동일한 주소를 가리키는지를 판단한다고 하였는데 a.equals(c)의 결과를 보면 a와 c는 서로 다른 객체임에도 불구하고 true를 반환한 것을 확인할 수 있다.  

그 이유는 String 클래스에서 Object 클래스의 equals() 메서드를 오버라이딩하여 재정의하였기 때문이다.  

![equals vs '=='   hashCode4](https://user-images.githubusercontent.com/53072057/109254761-04db9a80-7836-11eb-875d-c86c06c3b417.JPG)  

1번 구간은 기존의 equals() 메서드처럼 두 객체가 같은 주소를 가리키는지를 판단하는 부분이다. 하지만 2번 구간을 보면 두 객체의 문자열 구성이 동일하다면 true를 반환하도록 구현해 놓은 것을 확인할 수 있다.  

즉, String 클래스의 equals 함수는 **문자열이 같은지를 비교하는 역할을 수행하도록 두 객체가 다른 주소를 가리키더라도 같은 문자열이라면 동일하다고 판단하도록 구현한 것**이다.  

이처럼 equals() 메서드는 클래스에서 오버라이딩을 통해 자신의 입맛대로 재정의할 수 있다는 부분에서 '==' 비교연산자와 차이가 발생한다. '==' 비교연산자의 상위 호환이라고 생각하면 쉽다.  

하지만 '==' 비교연산자는 equals() 메서드에 포함될 수도 있고 포함되지 않을 수도 있다. 이건 equals() 메서드를 재정의하는 개발자에게 달렸다.  

마지막으로 **hashCode()** 메서드에 대해서 알아보도록 하자.  

hashCode() 메서드도 equals() 메서드와 마찬가지로 Object 클래스의 메서드이다. Object의 hashCode() 메서드는 객체의 메모리 주소를 이용해서 hashCode를 생성해주기 때문에 객체마다 다른 hashCode를 가진다.  

hashCode는 간단히 말해서 **객체를 식별하는 정수값을 의미**한다.  

그렇다면 객체마다 hashCode가 다르니 equals() 메서드처럼 hashCode() 메서드를 통해 두 객체가 동일한지를 판단할 수 있지 않을까? 이것도 equals() 메서드처럼 클래스에서 어떻게 구현하느냐에 따라 다르다.  

한번 String 클래스의 hashCode()를 살펴보자.  

![equals vs '=='   hashCode5](https://user-images.githubusercontent.com/53072057/109254764-04db9a80-7836-11eb-9ec5-2d51697c88e5.JPG)  

해싱은 충돌을 피하기 위해 해시 함수로 소수를 사용한다고 하였다. 여기서 31은 소수이면서 홀수이기 때문에 특별한 숫자로 해시 함수로 사용되었다. 더 자세하게 알고 싶으면 따로 찾아보길 바란다.  

즉, hashCode() 메서드는 "ABC"라는 String 객체가 있으면 'A', 'B, 'C'를 각각 해시 함수를 적용하여 "ABC"의 해시 값을 정해주는 것이다. 이는 다른 객체더라도 같은 문자열이면 동일한 해시값을 가진다.  

```java
public class Main {
	public static void main(String[] args) {
		String a = new String("ABC");
		String b = new String("ABC");
		System.out.println(a.hashCode() + " == " + b.hashCode());
	}
}
```

![equals vs '=='   hashCode6](https://user-images.githubusercontent.com/53072057/109254765-05743100-7836-11eb-825c-a4db248b4a96.JPG)  

a와 b는 다른 객체임에도 불구하고 같은 해시값을 가지는 것을 확인할 수 있다. 즉, String 클래스에서는 hashCode() 메서드로 두 객체가 동일한지는 판단할 수 없다.  

그렇다면 HashSet, HashMap, HashTable 등 Hash를 사용하는 컬렉션 프레임워크는 어떨까?  

결론부터 말하면 Hash를 사용하는 자료구조는 hashCode() 메서드로 두 객체가 동일한지 판단할 수는 없지만 판단하기 위해 활용된다.  

Hash를 사용하는 자료구조는 **두 객체가 동일한지 판단하기 위해 먼저 hashCode() 메서드를 통해 두 객체의 해시값이 같은지를 판단하고, 만약 같다면 equals() 메서드를 통해 동일한지 판단한다.**  

Hash를 사용하는 자료구조는 이론상 Key의 값을 찾을 때 **O(1)**의 시간 복잡도가 걸린다. 그 이유는 Key에 HashCode()를 적용시킨 해시값을 통해 Key에 해당하는 Value가 저장된 인덱스를 단번에 찾을 수 있기 때문이다.  

그렇다면 Hash를 이용한 자료구조는 hashCode() 메서드를 통해 두 객체가 같은지 판단할 수 있을 것 같은데 왜 hashCode() 메서드로 두 객체가 동일한지를 판단할 수 없는 걸까?  

그 이유는 메모리 한계상 Hash를 활용한 자료구조의 크기는 한정적이기 때문에 해시 충돌로 인해 다른 Key를 가지는 객체여도 같은 해시 값을 가질 수 있는 경우가 발생할 수 있기 때문이다.  

이러한 상황을 해결하기 위해 hashCode() 메서드로 확인한 후 equals() 메서드로 한 번 더 확인해 주는 것이다.  

그렇다면 어차피 결국 equals() 메서드를 통해 확인하는데 equals() 메서드만 재정의하여 사용하면 되는데 왜 굳이 hashCode()도 재정의해서 사용하는 것일까?  

이는 논리적 결함을 없애기 위해서이다.  

hashCode() 메서드를 재정의하지 않았다면 같은 객체라도 해시값이 다를 수 있다. 즉, HashTable에서 해당 객체가 저장된 버킷을 찾을 수 없는 경우가 발생하게 된다. 이는 Hash를 사용하는 이유에 모순된다.  

반대로 equals() 메서드를 재정의하지 않는다면 객체가 저장된 버킷은 찾을 수 있지만 해당 객체가 자신과 같은 객체인지 비교할 수 없기 때문에 null을 리턴하게 된다.  

즉, 둘 다 재정의하지 않는다면 Hash 자료구조를 사용할 수 없게 된다. 그렇기 때문에 두 메서드를 모두 재정의하여 사용하는 것이다.  

![equals vs '=='   hashCode7](https://user-images.githubusercontent.com/53072057/109254767-05743100-7836-11eb-8306-cbaf01a03865.JPG)  
