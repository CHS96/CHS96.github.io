---
title:  "[Java] 정규 표현식(Pattern, Matcher)"
excerpt: "[Java] 정규 표현식(Pattern, Matcher)을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-03-23T18:35:00
---

정규 표현식(Reqular Expression)은 **문자열을 처리하는 방법 중의 하나로 특정한 조건의 문자를 검색하거나 치환하는 과정을 매우 간편하게 처리할 수 있도록 하는 수단**이다.  

이메일이나 전화번호 등 특정 문자열을 확인할 때 if 문과 for 문을 통해 지저분하게 처리하던 것을 정규 표현식을 활용하면 훨씬 간단하고 깔끔하게 처리할 수 있다.  

Java에서는 util.regex 패키지에서 제공하는 **Pattern**과 **Matcher** 라이브러리를 사용하여 정규 표현식을 활용할 수 있다.  

실제로 문자열 클래스인 String 클래스는 특정한 조건의 문자를 검색하거나 치환하는 메서드를 정규 표현식을 활용하여 구현하였다.  

대표적으로 matches, replaceAll, replace, split 메서드들이 있다.  

<h3>[ 주어진 문자열에 매칭되는 값이 있는지 확인 ]</h3>  

![정규표현식](https://user-images.githubusercontent.com/53072057/112096339-731b3f00-8be1-11eb-89b2-ee570937c0fe.JPG)  

<h3>[ 주어진 문자열이 포함되는 모든 부분을 치환 ]</h3>  

![정규표현식1](https://user-images.githubusercontent.com/53072057/112096344-744c6c00-8be1-11eb-82af-9ae90151e4e3.JPG)  

replace와 replaceAll 메서드는 둘 다 주어진 문자열이 포함되는 모든 부분을 치환하는 메서드이지만 replace는 정규식을 인자 값으로 받고 replaceAll은 그냥 문자열을 인자 값으로 받는 차이가 존재한다.  

이는 실제로 문자열을 치환할 때 정규식을 적용시켜주냐의 차이가 발생한다.  

<h3>[ 주어진 문자열과 매치되는 문자열을 구분자로 자름 ]</h3>  

![정규표현식2](https://user-images.githubusercontent.com/53072057/112096346-744c6c00-8be1-11eb-8361-7a4e327403c3.JPG)  

다만, 정규 표현식은 문법을 모른다면 이해하기도 어렵고 활용하기도 힘들다. 그렇다면 정규 표현식의 문법과 Pattern, Matcher 클래스를 활용하여 사용하는 방법에 대해서 알아보자.  


먼저 정규 표현식 문법이다. 자주 사용하는 것들만 나타냈고 이외에도 다양한 문법이 존재한다.  

![정규표현식3](https://user-images.githubusercontent.com/53072057/112096348-74e50280-8be1-11eb-806f-483ef0f1e4bb.JPG)  

위의 문법을 활용하면 다음과 같이 특정 문자열을 표현할 수 있다.  

```java
﻿^[0-9]*$ : ﻿숫자로만 구성된 문자열을 표현
^[0-9a-zA-Z]*$ : 숫자, 영소(대)문자로만 구성된 문자열을 표현
^[0-9a-zA-Z]+@[a-zA-Z]+\\.[a-zA-Z]+$ : Email을 표현
```

Pattern과 Matcher를 활용하는 방법에 대해서 알아보자.  

먼저 Pattern 클래스이다. 클래스명에서 알 수 있듯이 **문자열을 검사할 패턴을 만들어내는 친구**이다.  

Pattern의 생성자는 private으로 선언되어 있기 때문에 new Pattern() 형식의 생성자로 생성할 수 없다.  

![정규표현식4](https://user-images.githubusercontent.com/53072057/112096353-757d9900-8be1-11eb-9036-8cd15a1af84c.JPG)  

Pattern 객체는 complie() 메서드를 통해 생성할 수 있다. compile() 메서드는 패턴으로 사용할 문자열만 인자로 생성하는 방법과 flags라는 인자를 하나 더 사용해서 만드는 2가지 방법이 존재한다.  

![정규표현식5](https://user-images.githubusercontent.com/53072057/112096355-757d9900-8be1-11eb-8dff-06b8c4507a0e.JPG)  

```java
String regex = "^[0-9]*$"; //숫자로만 이루어져 있는 패턴
Pattern pattern = new Pattern(regex) (x)
Pattern pattern = Pattern.complie(regex) (o)
```

flags는 패턴이 나중에 문자열을 처리하는 방법에 영향을 주는 친구로 다음과 같은 종류들이 존재한다.  

![정규표현식6](https://user-images.githubusercontent.com/53072057/112096357-76162f80-8be1-11eb-8346-37ee67b4bf09.JPG)  

```java
String str = "abcABdCD"; //대소문자로 구성된 문자열
String regex = "^[a-z]*$"; //소문자로만 이루어진 패턴
Pattern pattern = Pattern.compile(regex);
Matcher matcher = pattern.matcher(str);
System.out.println(matcher.matches()); => 대문자도 있기 때문에 false를 출력
---------------------------------------------------------------------
Pattern pattern = Pattern.compile(regex, Pattern.CASE_INSENSITIVE); //int로 0x02(2)
Matcher matcher = pattern.matcher(str);
System.out.println(matcher.matches()); => 대소문자를 무시하기 때문에 true를 출력
```

위의 방법을 보면 Matcher 객체를 사용하여 matches() 메서드로 검사하였지만 Matcher 객체 없이 Pattern 클래스에서 바로 matches() 메서드를 사용하여도 검사가 가능하다.  

![정규표현식7](https://user-images.githubusercontent.com/53072057/112096359-76aec600-8be1-11eb-8f8c-b84a7f940bee.JPG)  

```java
System.out.println(Pattern.matches(regex, str));
```

하지만 매번 검사할 때마다 Pattern, Matcher 객체를 생성해야 하기 때문에 한번 사용할 것이 아니라면 Pattern의 matches() 메서드보단 Matcher 객체를 생성하여 Matcher의 matches() 메서드를 사용하는 것이 낫다.  


다음으로 Matcher 클래스이다. Matcher 클래스명에서 알 수 있듯이 **패턴을 활용하여 문자열을 처리하는 기능을 수행하는 친구**이다.  

Matcher 클래스의 입력값으로 CharSequenece라는 인터페이스가 사용되는데 이를 통해 다양한 형태의 입력 데이터로부터 문자 단위의 매칭 기능을 사용할 수 있다.  

기본적으로 제공되는 CharSequence 객체들은 CharBuffer, String, StringBuffer 클래스가 존재한다.  

Matcher 클래스의 주요 메서드들은 다음과 같다.  

![정규표현식8](https://user-images.githubusercontent.com/53072057/112096360-76aec600-8be1-11eb-885d-81f2ffc4173c.JPG)  

*****

정규 표현식, Pattern, Matcher 활용하여 email을 검사하는 메서드를 구현하였다. if 문과 for 문을 사용하여 복잡하게 구현했던 기능을 매우 간단하게 구현할 수 있다는 장점이 있다.  

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {
	public static void main(String[] args) {
		String email1 = "abcde483@naver.com";
		String email2 = "abcde483@navercom";
		System.out.println("\"" + email1 + "\" is " + isEmail(email1));
		System.out.println("\"" + email2 + "\" is " + isEmail(email2));
	}
	
	public static boolean isEmail(String email) {
        //email을 표현하는 정규 표현식
        //간단하게 영문자, 숫자 + @ + 영문자 + . + 영문자의 조합으로 구현
		String regex = "^[0-9a-zA-Z]+@[a-zA-Z]+\\.[a-zA-Z]+$";
		Pattern p = Pattern.compile(regex);
		Matcher m = p.matcher(email);
		return m.matches();
	}
}
```

![정규표현식9](https://user-images.githubusercontent.com/53072057/112096361-77475c80-8be1-11eb-827a-1cafcc5c4c7d.JPG)  




<h2>[ Reference ]</h2>  
* <https://coding-factory.tistory.com/529>  
* <https://highcode.tistory.com/6>
* <https://offbyone.tistory.com/400>
  
