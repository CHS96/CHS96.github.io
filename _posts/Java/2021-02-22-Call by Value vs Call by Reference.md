---
title:  "Call by Value vs Call by Reference"
excerpt: "Java - Call by Value vs Call by Reference를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-02-22T18:35:00
---

함수의 호출 방식에서 대표적인 방식은 Call by Value(값에 의한 호출)와 Call by Reference(참조에 의한 호출), 두 가지 방식이 존재한다.  

아마 바로 Java를 접하였으면 잘 모를 수도 있지만 C 계열의 언어를 접해봤다면 Call by Value와 Call by Reference를 한 번쯤은 들어봤을 것이다.  

우선 간단한 swap 함수(교환하는 함수)를 통해 Call by Value와 Call by Reference에 대해서 알아보자.  

```C++
#include <iostream>
using namespace std;

void swap_CallByValue(int A, int B) {
    int temp = A;
    A = B;
    B = temp;
}

void swap_CallByReference(int* A, int* B) {
    int temp = *A;
    *A = *B;
    *B = temp;
}

int main() {
    int a = 10, b = 20;
    swap_CallByValue(a, b);
    cout << a << " " << b << endl;
    swap_CallByReference(&a, &b);
    cout << a << " " << b << endl;
    
    return 0;
}
```

이를 실행한 결과는 다음과 같다.  

![Call by Value vs Call by Reference](https://user-images.githubusercontent.com/53072057/108662345-77d0d280-7511-11eb-85be-588f1b2a19fd.JPG)  

첫 번째 swap_CallByValue() 함수를 실행하였지만 a와 b가 교환되지 않았다. 그 이유는 **Call by Value**로 동작했기 때문이다.  

Call  by Value는 값에 의한 호출 말 그대로 함수의 인자에게 값을 넘겨준다. 이게 무슨 뜻이냐면 넘겨준 변수(a, b)와 함수에서 실행되는 매개변수(A, B)는 똑같은 변수가 아니라는 뜻이다.  

즉, swap_CallByValue() 함수 내에서 A와 B를 출력하면 둘의 값이 교환된 것을 확인할 수 있지만 함수가 종료되면 아무것도 남게 되지 않기 때문에 기존의 a와 b는 교환된 값이 아닌 원래 자신의 값을 가진다.  

이와 반대로 swap_CallByReference() 함수를 실행한 두 번째 출력은 원하는 대로 a와 b의 값이 교환된 것을 확인할 수 있다. 이는 **Call by Reference**로 동작했기 때문이다.  

Call by Value는 값을 넘겨줬다면 Call by Reference는 값이 아닌 변수의 주소를 넘겨준다. 즉, a와 A, b와 B가 가리키는 메모리의 주소가 같다는 말이다.  

A가 가리키는 값을 변경한 후 그 주소를 가리키고 있는 a를 출력하게 되면 변경된 값이 출력되는 것이다.  

사실 Call by Value와 Call by Reference가 무엇인지는 알고 있다. 이번 글을 작성하는 목적은 Java는 과연 이들 중 어떤 호출 방식을 사용하는 것인가?에 대한 의문이 들었기 때문이다.  

학부시절 때 Java는 Call by Value 방식으로만 동작한다고 배웠다.  

왜냐하면 Java에는 흔히 C 계열에서 말하는 포인터(Pointer)라는 개념이 없다.(비슷한 개념으로 참조라는 개념이 존재한다. 하지만 이는 포인터와는 다른 개념이다.) 즉, 주소를 관리할 수 없다는 뜻이다. 주소를 관리할 수 없다는 말은 Call by Reference로 동작할 수 없다는 의미이다.  

하지만 Java로 프로그래밍을 하다 보면 위의 예시처럼 Call by Reference로 동작하는 상황을 접할 수 있다. 분명히 동작할 수 없다고 배웠는데 이는 모순이 아닌가?에 대한 의문점이 생겼다.  

결론적으로 말하면 **Java는 Call by Value로만 동작**한다. 의문점을 해결하기 전에 우선 Java에서 메서드에 넘겨줄 수 있는 데이터 타입에 대해서 알아보자.  

Java의 데이터 타입은 크게 **원시 타입(Primitive Type)과 참조 타입(Reference Type)**으로 나뉜다.  

![Call by Value vs Call by Reference2](https://user-images.githubusercontent.com/53072057/108662349-7901ff80-7511-11eb-8c12-230535bf9138.JPG)  

Java에서 원시 타입의 데이터를 매개변수로 넘겨주면 Call by Value로 동작하게 된다.  

```java
class Main {
	public static void main(String[] args) {
		int a = 10, b = 20;
		swap(a, b);
		System.out.println("a : " + a + "\n" + "b : " + b);
	}
	
	public static void swap(int a, int b) {
		int temp = a;
		a = b;
		b = temp;
	}
}
```

![Call by Value vs Call by Reference3](https://user-images.githubusercontent.com/53072057/108662350-799a9600-7511-11eb-88d2-4efde0bc80b9.JPG)  

그렇다면 참조 타입인 Integer 데이터를 매개변수로 넘겨줘보자.  

```java
class Main {
	public static void main(String[] args) {
		Integer a = new Integer(10);
		Integer b = new Integer(20);
		swap(a, b);
		System.out.println("a : " + a + "\n" + "b : " + b);
	}
	
	public static void swap(Integer a, Integer b) {
		Integer temp = a;
		a = b;
		b = temp;
	}
}
```

![Call by Value vs Call by Reference4](https://user-images.githubusercontent.com/53072057/108662351-799a9600-7511-11eb-9a9f-4be9805e23fa.JPG)  

교환되지 않는 것을 보니 참조 타입도 Call by Value로 동작하는구나라고 생각할 수 있다. 그렇다면 이번에는 정수형 변수 하나를 가지는 Test 객체를 생성하여 두 객체의 변수 값을 변경해보자.  

```java
class Test {
	int data;
	
	public Test(int data) {
		this.data = data;
	}
}

class Main {
	public static void main(String[] args) {
		Test a = new Test(10);
		Test b = new Test(20);
		swap(a, b);
		System.out.println("a : " + a.data + "\n" + "b : " + b.data);
	}
	
	public static void swap(Test A, Test B) {
		int temp = A.data;
		A.data = B.data;
		B.data = temp;
	}
}
```

![Call by Value vs Call by Reference5](https://user-images.githubusercontent.com/53072057/108662352-7a332c80-7511-11eb-8ecf-f893d67df628.JPG)  

결과를 보면 a와 b의 값이 변경된 것을 확인할 수 있다. 변경되었다는 것은 Call by Reference로 동작한 것이 아닌가?라고 생각할 수 있지만 아쉽게도 Call by Value 방식으로 동작한 것이다.  

우선 위 2개의 코드의 차이점이 무엇일까? 첫 번째 코드는 직접 해당 객체의 값을 직접 교환하였다면 두 번째 코드는 객체의 참조 값을 통해 해당하는 변수를 교환하였다는 점이다.  

객체의 값을 직접 교환한 경우는 교환되지 않았지만 객체의 참조 값을 통해 교환된 이유는 다음과 같다.  

이전에 Java의 메모리 구조를 공부할 때 new 연산을 통해 객체를 생성하면 지역변수는 스택 영역, 객체는 힙 영역에 할당된다고 하였다.  

Java에서 객체를 메서드의 인자로 넘길 경우 **Call by Reference처럼 객체의 실제 주소를 넘기는 것이 아니라 지역변수가 가리키고 있는 힙 영역의 객체를 가리키는 새로운 지역변수를 생성하여 같은 객체를 가리키도록 한다.**  

즉, a와 b가 가리키는 객체를 가리키는 A와 B라는 객체를 생성하여 해당 객체의 값을 변경해주는 것이다.  

![Call by Value vs Call by Reference6](https://user-images.githubusercontent.com/53072057/108662354-7a332c80-7511-11eb-8567-86128d3f1551.JPG)  

![Call by Value vs Call by Reference7](https://user-images.githubusercontent.com/53072057/108662355-7acbc300-7511-11eb-8391-8984a6352971.JPG)  


<h2>[ Reference ]</h2>  
* <https://sleepyeyes.tistory.com/11>  
* <http://wonwoo.ml/index.php/post/1679>  