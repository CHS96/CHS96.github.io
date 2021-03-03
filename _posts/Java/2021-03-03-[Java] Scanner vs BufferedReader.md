---
title:  "[Java] Scanner vs BufferedReader"
excerpt: "[Java] Scanner vs BufferedReader를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-03-03T18:35:00
---

Java를 처음 공부할 때는 Scanner 클래스를 사용하여 데이터들을 입력받았을 것이다.  

```java
import java.util.Scanner;

public class Main {
	public static void main(String args[] ) {
		Scanner sc = new Scanner(System.in);
		
		int num = sc.nextInt(); //정수형 데이터를 입력
		System.out.println(num);
		
		sc.close();
	}
}
```

하지만 Problem Sovling을 하다 보면 입력이 굉장히 많은 문제가 있는데 이때는 Scanner를 사용하면 시간 초과가 발생하게 된다. 이를 해결하기 위해서는 BufferedReader 클래스를 사용해야 한다.  

그렇다면 Scanner와 BufferedReader는 왜 데이터를 입력받을 때 시간 차이가 발생하는 것일까?  

이는 **입력받은 데이터를 언제 사용자에게 전송하는지에 대한 차이** 때문에 발생한다.  

먼저 Scanner이다. Scanner는 **데이터를 입력받을 경우 즉시 사용자에게 전송**된다. 입력받을 때마다 전송되어야 하기 때문에 이 부분에서 많은 시간이 소요된다. 또한, Scanner는 내부에서 입력 데이터를 처리할 때 정규식을 많이 사용하기 때문에 이 또한 상당한 시간이 소요된다.  

![Java  Scanner vs BufferedReader](https://user-images.githubusercontent.com/53072057/109741371-442c3180-7c10-11eb-8537-c94c788323e4.JPG)  

반면, BufferedReader는 데이터를 입력받을 경우 사용자에게 바로 전송되는 것이 아니라 **버퍼라는 저장 공간에 하나씩 채우다가 버퍼가 가득 차거나 개행 문자를 만날 경우 버퍼의 내용을 사용자에게 전송하는 방식**이다.  

![Java  Scanner vs BufferedReader1](https://user-images.githubusercontent.com/53072057/109741374-455d5e80-7c10-11eb-8460-533f70ffd37b.JPG)  

매번 보내는 것이 아니라 한 번에 모았다가 보내기 때문에 전송하는데 걸리는 시간을 절약할 수 있는 것이다.  

그렇다면 속도가 훨씬 빠른 BufferedReader만 사용하면 되는데 왜 Scanner라는 기능이 존재하는 걸까?  

그 이유는 BufferedReader는 속도가 빠른 대신 몇 가지 단점이 존재한다. Scanner는 BufferedReader보다 나중에 나온 기능으로 BufferedReader의 장점인 속도를 낮추는 대신 단점인 기능들을 보완해 준 친구라고 생각하면 쉽다.  

그렇다면 둘의 장단점을 비교해보자.  

첫 번째로 BufferedReader는 입력 데이터를 처리하는 기능이 Scanner보다 별로 없다. BufferedReader는 입력을 한 줄 단위로 받기 때문에 모든 입력 데이터를 String으로 인식한다. 따라서 문자열이 아닌 다른 데이터 타입으로 사용하기 위해서는 별도로 형변환을 해줘야 한다.  

```java
int numi = Integer.parseInt(br.readLine()); //문자열을 정수(int)로 변환
long numl = Long.parseLong(br.readLine()); //문자열을 정수(long)로 변환
```

하지만 Scanner는 입력받는 시점에 데이터의 타입이 결정되므로 별도의 형변환이 필요 없다.  

```java
int numi = sc.nextInt();
long numl = sc.nextLong();
```

이뿐만 아니라 Scanner는 훨씬 많은 기능을 제공해주기 때문에 BufferedReader 보다 문자열을 분석하고 파싱하는 등 다양하게 처리할 수 있는 장점이 있다.  

두 번째로 BufferedReader는 무조건 try-catch 문이나 throws 문을 통해 사용자가 직접 예외 처리를 해줘야 한다.  

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

//throws
public class Main {
	public static void main(String args[] ) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String s = br.readLine();
		br.close();
	}
}

//try-catch
public class Main {
	public static void main(String args[] ) {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		try {
			String s = br.readLine();
			br.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```

반면 Scanner는 사용자가 직접 예외 처리를 구현할 필요가 없다.  

```java
import java.util.Scanner;

public class Main {
	public static void main(String args[] ) {
		Scanner sc =new Scanner(System.in);
		int num = sc.nextInt();
		sc.close();
	}
}
```

이는 BufferedReader는 **Checked Exception**으로 반드시 예외 처리를 해줘야 하고 Scanner는 **UnChecked(Runtime) Exception**으로 명시적인 예외 처리를 하지 않아도 되기 때문이다.  

![Java  Scanner vs BufferedReader2](https://user-images.githubusercontent.com/53072057/109741376-455d5e80-7c10-11eb-9702-65114b591f78.JPG)  

이에 관해선 나중에 따로 공부할 것이라 궁금하다면 Checked Exception vs UnChecked Exception 키워드로 따로 검색해보길 바란다.  

마지막으로 BufferedReader는 동기화를 지원해주지만 Scanner는 동기화를 지원해주지 않는다. 따라서 멀티 스레드 환경에서는 BufferedReader를 사용하는 것이 안전하다.  

![Java  Scanner vs BufferedReader3](https://user-images.githubusercontent.com/53072057/109741378-45f5f500-7c10-11eb-9065-ae8fea2f655e.JPG)  


<h2>[ Reference ]</h2>  
* <https://carpediem0212.tistory.com/11>  
* <https://eatnows.tistory.com/89>  
