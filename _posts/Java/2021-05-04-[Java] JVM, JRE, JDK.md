---
title:  "[Java] JVM, JRE, JDK"
excerpt: "[Java] JVM, JRE, JDK를 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-05-04T18:35:00
---

Java 프로그래밍 문법을 배우기 전에 JVM, JRE, JDK 등에 대해서 먼저 배우게 된다.  

이들의 차이점에 대해서 한 번 알아보도록 하자.  


<h1>JVM(Java Virtual Machine)</h1>  

JVM은 자바 가상 머신(Java Virtual Machine)의 약자로써 Java의 특징인 동일한 프로그램이 다양한 컴퓨터에서 실행이 가능하도록 해주는 친구이다.  

JVM은 자바 소스코드(.java)가 컴파일러에 의해 만들어지는 바이트 코드(.class)를 해석하여 실행할 수 있다.  

또한, JVM은 플랫폼에 의존적이다. 즉, 리눅스의 JVM과 윈도우즈의 JVM은 서로 다르다.  

단, 컴파일된 바이트 코드(.class)는 어떤 JVM에서도 동작할 수 있다.  
  
  
JVM은 다음과 같은 역할은 수행한다.  
	* 바이트 코드를 읽는다.  
	* 바이트 코드를 검증한다.  
	* 바이트 코드를 실행한다.  
	* 실행 환경(Runtime Environment)의 규격을 제공한다. (필요한 라이브러리 및 기타 파일)  

​

  
<h1>JRE(Java Runtime Environment)<h1>  

JRE는 자바 실행 환경(Java Runtime Environment)의 약자로써 컴파일된 자바 프로그램을 실행시킬 수 있는 자바 환경을 의미한다.  

JRE는 JVM이 자바 프로그램을 동작시킬 때 필요한 라이브러리 파일들과 기타 파일들을 가지고 있다.  

JRE는 JVM의 실행 환경을 구현했다고 표현할 수 있다.  

JRE는 자바 프로그램을 실행하기 위한 필수적인 요소이다.  

하지만 JRE만 존재한다면 자바 프로그램을 실행할 수는 있지만 자바 프로그램을 개발할 수는 없다.  

이를 가능하게 해주는 친구가 바로 JDK(Java Development Kit)이다.  
​
![Java  JVM, JRE, JDK](https://user-images.githubusercontent.com/53072057/116960631-f4371d00-acdb-11eb-941b-9636744aeb70.JPG)  

​

  
<h1>JDK(Java Development Kit)<h1>  

JDK는 JRE + 자바 프로그램을 개발하기 위해 필요한 도구(javac, java 등)을 포함한다.  

즉, JDK를 설치하면 자바 프로그램을 실행시킬 수 있는 JRE도 함께 설치된다.  

![Java  JVM, JRE, JDK1](https://user-images.githubusercontent.com/53072057/116960634-f5684a00-acdb-11eb-91e9-3eca2d0fd28e.JPG)  




<h2>[ Reference ]</h2>  
* <https://wikidocs.net/257>  
