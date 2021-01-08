---
title:  "스프링(Spring) MVC 패턴"
excerpt: "스프링(Spring) MVC 패턴을 알아보자!"

categories:
  - Spring
  
last_modified_at: 2021-01-08T18:35:00
---

이전에 MVC 패턴에 대해서 공부하였다. MVC 패턴은 **Model, View, Controller로 구성되어 역할들을 분리하여 개발이 용이하도록 하는 디자인 패턴**이라고 설명하였었다. 스프링(Spring) 또한 웹 애플리케이션을 개발할 때 기본적으로 MVC 패턴을 바탕으로 개발한다. 어떻게 보면 Model, View, Controller로 비슷하게 볼 수 있지만 약간 다르기 때문에 한번 알아보도록 하자.  

스프링 MVC 패턴은 다음과 같은 구조를 가진다.  

![스프링MVC1](https://user-images.githubusercontent.com/53072057/103963166-f6f58b80-519b-11eb-8d97-878c003a1b57.JPG)  

MVC 패턴에서 사용자는 Controller, View가 소통하였다면 스프링 MVC 패턴에서 **사용자는 DispatcherServler라는 친구와만 소통**한다.  

스프링 MVC 패턴은 다음과 같이 동작한다.  

1. 사용자가 서비스를 요청하면 DispatcherServlet에게 전달된다.

2. DispatcherServler은 HandlerMapping을 통해서 요청에 해당하는 Controller를 알아낸다.

3. 알아낸 Controller를 HandlerAdapter에게 전달하면 HandlerAdapter에서 Controller에게 알맞은 타입으로 변환해서 Controller에게 전달한다.

4. Controller는 Model에게 Data를 요청하고 Model은 연동된 Database를 통해 알맞은 Data를 Controller에 응답한다.

5. Controller는 전달받은 Data를 알맞은 타입으로 변환하여 HandlerAdapter에게 전달하고 HandlerAdapter는 이를 DispatcherServlet에게 전달된다.

6. DispatcherServlet은 처리 결과를 ViewResolver에게 전달하여 알맞은 View를 알아내고 DispatcherServlet에게 전달한다.

7. DispatcherServlet은 알맞은 View에게 Data를 전달하고 View는 처리한 View를 DispatcherServlet에게 전달한다.

8. DispatcherServlet은 사용자의 요청을 처리한 View를 사용자에게 전달한다.  

기존의 MVC 패턴보다 조금 복잡한 구조를 가지고 있는 것을 확인할 수 있다. Model, Controller, View는 이전 시간에 공부하였으니 새롭게 나온 것들에 대해서 어떤 역할을 수행하는지 간단하게 알아보자.  

* DispatcherServlet  

ㅁ 사용자의 요청을 받아들이는 친구로서 FrontController라고 불린다.  

ㅁ 스프링 프레임워크의 중심이 되는 서블릿으로 모든 사용자의 요청을 받아 사용자에게 응답하기까지의 모든 과정을 제어한다.  

ㅁ web.xml에 설정한다.  

* HandlerMapping  

ㅁ 사용자가 요청한 서비스에 알맞은 Controller를 찾아서 매핑해주는 친구이다.  

ㅁ @Controller 어노테이션이 적용된 객체의 @RequestMapping 값을 이용해 요청을 처리할 Controller를 매핑한다.  

* HandlerAdapter  

ㅁ DispatcherServlet의 처리 요청을 변환에서 Controller에게 전달하는 친구이다.  

ㅁ Controller의 응답 결과인 Data를 DispatcherServlet이 요구하는 타입으로 변환해 준다.  
​
* ViewResolver  

ㅁ 사용자의 서비스 요청을 보여줄 View를 검색하여 찾아주는 친구이다.  
