---
title:  "퍼사드 패턴(Facade Pattern)"
excerpt: "퍼사드 패턴(Facade Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-03-27T18:35:00
---

퍼사드 패턴(Facade Pattern)은 **구조(Structural) 패턴 중 하나로써 복잡한 서브 클래스들을 피해 더 상위에 인터페이스를 구성함으로써 서브 클래스들의 기능을 간편하게 사용할 수 있도록 하는 패턴**이다.  

건물의 외관을 뜻하는 퍼사드(Facade)처럼 퍼사드 패턴은 건물의 내부(서브 클래스)들을 건물의 외관(상위의 인터페이스)로 감싸서 사용할 수 있으며 건물의 내부는 들여다볼 수 없다는 것을 의미한다.  

즉, **복잡한 내부 동작은 알 필요 없고 기능 자체만 사용하면 되는 경우 사용하면 되는 디자인 패턴**이다.  

서브 클래스들 사이의 통합 인터페이스를 제공하는 **Wrapper 객체**가 필요하다.  

퍼사드 패턴의 구조는 다음과 같다.  

![퍼사드패턴1](https://user-images.githubusercontent.com/53072057/112709401-a1a85b00-8efc-11eb-9076-41e9b1c21f60.JPG)  

* Facade : Package의 기능들을 감싸는 상위 인터페이스, 하위의 기능들을 수행하는 단순한 메서드 하나를 가짐  

* package : 각 기능을 담당하는 클래스  

*****

얼마 전에 공부한 "웹 브라우저에 URL을 입력했을 때 발생하는 일"을 예시로 퍼사드 패턴을 알아보자.  

실제로 웹 브라우저를 사용하는 User는 URL을 주소창에 입력하여 Enter만 누르면 되지 실제로 내부에서 어떻게 동작하는지는 알 필요가 없다.  

즉, 내부 동작을 감싸서 전체 기능만 수행할 수 있으면 되기 때문에 퍼사드 패턴을 활용할 수 있다.  

URL을 웹 브라우저에 입력하여 Enter를 누르면 내부에서는 총 8단계의 과정을 통해 우리의 화면에 주소창의 정보들이 출력된다. 따라서 각 기능들을 단계별로 클래스로 나타낸다.  

여기서 각 기능들은 별도의 패키지를 하나 선언하여 그 안에 구현해준다. 이유는 패키지 내에 선언한 클래스들은 public 접근 제한자를 붙이지 않으면 **default**로 지정되기 때문에 **오직 같은 패키지 내에서만 접근할 수 있게 된다**.  

외부의 Client에서는 내부의 기능을 알 필요가 없기 때문에 접근할 수 없도록 해주는 것이다.  

```java
package UrlSystem;

class Level1 {
	public Level1(String URL) {
		System.out.println(URL + " Enter!");
		System.out.println("===============================================");
		System.out.println("Level 1 : 브라우저의 "+ URL + " 파싱");
	}
}
```

```java
package UrlSystem;

class Level2 {
	public Level2(String URL) {
		System.out.println("Level 2 : HSTS 목록 조회");
	}
}
```

. . .  

```java
package UrlSystem;

class Level8 {
	public Level8(String URL) {
		System.out.println("Level 8 : 브라우저에서 응답을 해석");
		System.out.println("===============================================");
		System.out.println(URL + " 화면 출력!");
	}
}
​```

이렇게 총 8단계의 기능을 가지는 클래스들을 각각 구현해준다. 외부에서는 해당 기능들을 접근할 수 없기 때문에 8단계의 기능을 수행해주는 상위 인터페이스 하나를 구현해준다.  

```java
package UrlSystem;

public class UrlFacade {
	Level1 level1; Level2 level2;
	Level3 level3; Level4 level4;
	Level5 level5; Level6 level6;
	Level7 level7; Level8 level8;
	
	public UrlFacade(String URL) {
		level1 = new Level1(URL); level2 = new Level2(URL);
		level3 = new Level3(URL); level4 = new Level4(URL);
		level5 = new Level5(URL); level6 = new Level6(URL);
		level7 = new Level7(URL); level8 = new Level8(URL);
	}
}
```

UrlFacade 클래스는 8단계의 기능을 수행해 줄 수 있다. 또한, 같은 UrlSystem Package이지만 **public 접근 제한자**를 사용하기 때문에 외부의 Client에서도 접근할 수 있다.  

Client는 UrlFacade 클래스를 통해 URL의 내부 동작을 알 필요 없이 원하는 기능을 수행할 수 있다.  

```java
package facade;

import UrlSystem.UrlFacade;

public class Client {
	public static void main(String[] args) {
		UrlFacade urlFacade = new UrlFacade("https://www.naver.com/");
	}
}
```

![퍼사드패턴2](https://user-images.githubusercontent.com/53072057/112709402-a2d98800-8efc-11eb-8458-dfa6cdf90be3.JPG)  

각 클래스들의 구조는 다음과 같다.  

![퍼사드패턴3](https://user-images.githubusercontent.com/53072057/112709403-a3721e80-8efc-11eb-86be-b475122782ee.JPG)  

퍼사드 패턴을 활용하면 하위 클래스 기능들을 하나의 상위 인터페이스로 단순화시켜 사용할 수 있는 장점이 있다.  


