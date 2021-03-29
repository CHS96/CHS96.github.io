---
title:  "[Java] BigInteger : 큰 수를 표현하는 방법"
excerpt: "[Java] BigInteger : 큰 수를 표현하는 방법을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-03-29T18:35:00
---

Java에서 정수를 표현하는 자료형은 byte, short, int, long이 존재한다.  

이들은 각각 다음과 같은 크기의 정수를 표현할 수 있다.  

![BigInteger](https://user-images.githubusercontent.com/53072057/112789756-c2a5b300-9098-11eb-8227-82f148499631.JPG)  

즉, 최대 범위인 long으로 표현할 수 있는 -9223372036854775808 ~ 9223372036854775807 내의 정수밖에 표현할 수 없다.  

그렇다면 long 범위를 벗어나는 수를 표현하고 싶다면 어떻게 해야 할까?  

예를 들어 10000000000000000000을 표현하고 싶다고 하자.  

우리는 일단 long으로는 이 수를 표현할 수 없다.  

![BigInteger1](https://user-images.githubusercontent.com/53072057/112789757-c3d6e000-9098-11eb-9e86-33d24efa535c.JPG)  

하지만 문자열로 표현한다면 이 수를 표현할 수 있다.  

![BigInteger2](https://user-images.githubusercontent.com/53072057/112789761-c3d6e000-9098-11eb-8934-fc151ac015fd.JPG)  

일단 수를 표현하는 데까지는 성공하였다. 하지만 이 수를 이용해서 다양한 연산을 할 수 있어야 한다.  

학창 시절 수학 문제를 풀 때 두 수를 덧셈하는 방식을 기억해보자. 일의 자리부터 더해가면서 더한 값이 자리수를 넘기면 다음 자리수로 넘겨줘서 더해주는 방식으로 계산을 하였다.  

![BigInteger3](https://user-images.githubusercontent.com/53072057/112789762-c3d6e000-9098-11eb-93ad-7d7397969b5a.JPG)  

이러한 방식처럼 문자열의 각 자리수를 배열로 관리하여 연산을 수행할 수 있다. 위의 방식으로 간단하게 두 문자열로 덧셈을 수행하는 메서드를 구현해봤다.  

```java
public class Blog {

	public static void main(String[] args) {
		String A = "185";
		String B = "17";
		System.out.println(A + " + " + B + " = " + plus(A, B));
		
		A = "10000000000000000000";
		B = "10000000000000000000";
		System.out.println(A + " + " + B + " = " + plus(A, B));
	}
	
	public static String plus(String A, String B) {
		if (A.length() < B.length()) {
			String temp = A;
			A = B;
			B = temp;
		}
		
		int[] arrA = new int[A.length() + 1];
		int[] arrB = new int[A.length() + 1];
		
		for (int i = A.length() - 1, j = 0; i >= 0; --i) {
			arrA[j++] = A.charAt(i) - '0';
		}
		
		for (int i = B.length() - 1, j = 0; i >= 0; --i) {
			arrB[j++] = B.charAt(i) - '0';
		}
		
		StringBuilder sb = new StringBuilder();
		for (int i = 0; i < A.length(); ++i) {
			int sum = arrA[i] + arrB[i];
			
			if (i < A.length() - 1 && sum >= 10) {
				sum -= 10;
				arrA[i + 1]++;
			}
			
			sb.insert(0, sum);
		}

		return sb.toString();
	}
}
```

![BigInteger4](https://user-images.githubusercontent.com/53072057/112789764-c46f7680-9098-11eb-8e4e-83f33a9855f8.JPG)  

long을 벗어나는 두 숫자의 덧셈 연산이 원하는 대로 잘 수행되는 것을 확인할 수 있다.  

이처럼 연산하는 메서드를 구현해도 되지만 Java에서는 이미 이러한 기능을 **BigInteger** 클래스로 구현해놨으며 우리는 이 기능을 가져다 쓰기만 하면 된다.  

그렇다면 BigInteger 클래스의 사용법에 대해서 간단하게 알아보도록 하자.  

BigInteger 객체를 생성하는 생성자는 종류가 다양하지만 가장 기본적인 방법은 **수를 표현한 문자열을 입력으로 받아 객체를 생성하는 방법**이다.  

```java
BigInteger num = new BigInteger("32123");
```

이는 기본적으로 해당 문자열을 10진수의 수로 표현해 준다. 만약 다른 진법으로 표현한 문자열을 생성하고 싶으면 다음과 같이 해주면 된다.  

```java
BigInteger num = new BigInteger("1A", 16); //16진수의 수를 생성, 1A == 26
```

다음으로 두 수를 연산하는 기능이다. BigInteger 객체는 위에서 동작원리를 봐서 알겠지만 일반적인 연산으로는 계산할 수 없다. **오직 BigInteger 클래스에서 지원하는 메서드로만 가능하다**.  

```java
BigInteger A = new BigInteger("25");
BigInteger B = new BigInteger("6");
		
//System.out.println(A+B); (x)
System.out.println(A.add(B)); // '+'
System.out.println(A.subtract(B)); // '-'
System.out.println(A.multiply(B)); // '*'
System.out.println(A.divide(B)); // '/'
System.out.println(A.mod(B)); // '%'
```

이뿐만 아니라 max, mod, gcd, abs 등 다양한 기능을 지원해 준다.  

또한, long, int 정수형을 **valueOf()** 메서드를 통해 BigInteger형으로 변환할 수 있으며, **intValue(), longValue()** 등의 메서드를 통해 반대로도 변환이 가능하다.  

```java
int a = 123;
BigInteger A = BigInteger.valueOf(a); //int -> BigInteger
		
BigInteger B = new BigInteger("123");
int b = B.intValue(); //BigInteger -> int
```

이처럼 BigInteger 클래스를 잘 활용하면 long 범위를 벗어나는 수들로도 다양한 연산이 가능하다. 하지만 당연히 일반적인 계산 방식이 아니기 때문에 기존의 연산 방식보다는 시간이 더 걸린다는 단점이 있다.  

![BigInteger5](https://user-images.githubusercontent.com/53072057/112789765-c46f7680-9098-11eb-926d-844739298779.JPG)  

