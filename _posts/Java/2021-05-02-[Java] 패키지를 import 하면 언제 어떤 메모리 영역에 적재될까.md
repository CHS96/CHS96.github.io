---
title:  "[Java] 패키지를 import 하면 언제 어떤 메모리 영역에 적재될까?"
excerpt: "[Java] 패키지를 import 하면 언제 어떤 메모리 영역에 적재되는지 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-05-02T18:35:00
---

어떤 언어든지 간에 프로그래밍을 하다 보면 언어 내에서 이미 작성해놓은 패키지(라이브러리)나 외부 패키지(라이브러리)를 활용하는 경우가 상당히 많다.  

당장 알고리즘 문제를 풀 때만 하여도 입출력, 정렬 부분은 직접 구현하지 않고 이미 구현해놓은 기능들을 가져다 쓴다.  

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
	
	public static void main(String[] args) {
		try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
				BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));) 
		{
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```


위 상황은 io 패키지를 import 하여 사용한 예시이며, Java는 기본적으로 아무런 패키지를 import 하지 않아도 String, StringBuffer, StringBuilder, Math, Exception 등 다양한 패키지를 기본적으로 사용할 수 있다.  

```java
public class Main {
	
	public static void main(String[] args) {
		String str = "Hello World!";
		int big = Math.max(10, 30);
		Exception exception = new Exception();
	}
}
```

이는 Java에서 java.lang 패키지를 default로 항상 import 하기 때문에 lang 패키지 내의 라이브러리들을 사용할 수 있는 것이다.  

여기서 의문이 하나 생겼다. JVM의 Class Loader가 .class 파일을 Runtime Data Area에 적재해 줄 것인데 import 한 패키지나 java.lang 패키지 같은 경우는 언제 어떤 메모리에 적재되는 것인지가 궁금해졌다.  



이를 알기 위해선 **자바 프로그램의 실행 흐름**을 알아야 한다. 그전에 프로그램의 메모리 영역을 간단하게 알고 넘어가자.  

일반적으로 프로그램은 메모리 영역을 크게 **코드 실행 영역**과 **데이터 저장 영역**으로 나눈다. 또한 데이터 저장 영역은 **스태틱(Static) 영역, 스택(Stack) 영역, 힙(Heap) 영역**으로 나뉜다.  

여기서 중요한 영역은 Runtime Data Area에 속하는 데이터 저장 영역이다. 이들은 각각 클래스, 메서드, 객체들을 관리하는 영역이다.  

![import](https://user-images.githubusercontent.com/53072057/116805017-5f5ae500-ab5e-11eb-88af-a1749b71c560.JPG)  


다음으로 자바 프로그램의 실행 흐름을 살펴보자.  

자바 프로그램은 기본적으로 main() 메서드의 실행으로 시작하여 main() 메서드의 종료로 끝난다. 자바 프로그램의 실행 과정은 다음과 같다.  

1. JRE(Java Runtime Enviroment)가 자바 프로그램을 실행시킬 때 main() 메서드를 찾는다.  

2. main() 메서드가 존재하면 Class Loader가 목적 파일(.class)을 실행시킨다.  

3. Runtime Data Area의 Static 영역에 java.lang 패키지, import 한 패키지들을 적재시킨다.  

      - 프로그램에 있는 모든 클래스와 필드, 메서드가 올라간다.

4. Stack 영역에 main() 메서드의 Stack Frame을 적재시키고 변수 영역에 인자들을 위치시킨다.  

      - 지역변수의 경우 선언이 아닌 초기화될 때 위치된다.  

      - 클래스 선언 {}을 제외하고 메서드의 {}. if의 {}가 생길 때마다 Stack Frame이 생긴다.  

5. main() 메서드를 실행한다.  

6. 메서드는 }를 만나게 되면 Stack Frame에서 사라지게 된다. 즉, main() 메서드의 }를 만나게 될 경우 main() 메서드는 Stack Frame에서 사리 지게 되며 JRE는 JVM을 종료시키고 모든 메모리들을 제거한다.  


![import1](https://user-images.githubusercontent.com/53072057/116805019-5ff37b80-ab5e-11eb-81a8-3fdf6b67f16f.JPG)  




<h2>[ Reference ]</h2>  
* <https://sehun-kim.github.io/sehun/JVM/>  
