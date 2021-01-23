---
title:  "REST, REST API, RESTful"
excerpt: "REST, REST API, RESTful을 알아보자!"

categories:
  - Web
  
last_modified_at: 2021-01-23T18:35:00
---

최근 대부분의 API는 REST API를 제공하는 추세이다. 하지만 REST, REST API, RESTful에 대해 아직까지 잘 모르기 때문에 막상 질문이 들어오면 대답하기가 어렵다. 따라서 이번 기회에 한번 알아보려고 한다.  

아래의 글을 참고하여 공부 및 정리해보려고 한다.  

[![REST, REST API, RESTful1](https://user-images.githubusercontent.com/53072057/105569600-2e605c80-5d86-11eb-94b5-e2e825dc14cf.JPG)](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)  

먼저 공통된 단어인 REST에 대해서 알아보자.  

REST는 Representational State Transfer의 약자로써 **자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고받는 모든 것**을 의미한다. 자원은 문서, 그림, 데이터 등 해당 소프트웨어가 관리하는 모든 것을 뜻한다. 일반적으로 JSON or XML 형식을 통해 자원의 상태를 전달한다.  

월드 와이드 웹(www)과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 개발 아키텍처의 한 형식으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 **웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일**이다.  

따라서 웹의 관점에서 REST의 정의는 **HTTP URI를 통해 자원(Resouce)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것**을 의미한다.  

즉, REST는 **자원 기반의 구조(ROA, Resource Oriented Architecture) 설계의 중심에 Resource가 있고 HTTP Method를 통해 Resource를 처리 하도록 설계된 아키텍처**이다.  

CRUD는 Create, Read, Update, Delete의 줄임말로 각각 HTTP Method와 매칭된다.  

![REST, REST API, RESTful2](https://user-images.githubusercontent.com/53072057/105569601-2f918980-5d86-11eb-8f07-b8a9e6f1b8e2.JPG)  

REST의 장단점은 다음과 같다.  

* 장점  
	- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요 없음  
	- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있음  
	- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용 가능  
	- Hypermedia API의 기본을 충실히 지키면서 범용성을 보장  
	- REST API 메시지가 의도하는 바를 명확하게 나타냄  
	- 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화  
	- 서버와 클라이언트의 역할을 명확하게 분리  
	
* 단점
	- 표준이 존재하지 않음  
	- 사용할 수 있는 메서드가 제한적  
	- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 값이 더 어려움  
	- 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재  
	
REST는 자원, 행위, 표현 3가지로 구성된다.  

* 자원(Resource) : URI  
	- 모든 자원은 고유한 ID가 존재하고, 이 자원은 Server에 존재  
	- 자원을 구별하는 ID는 HTTP URI 형태이다.  
	- Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태에 대한 조작을 Server에 요청  
	
* 행위(Verb) : HTTP Method  
	- HTTP 프로토콜의 POST, GET, PUT, DELETE를 사용  
	
* 표현(Representation of Resource)  
	- Server는 JSON, XML 형식의 데이터를 통해 Client의 요청에 대한 적절한 응답을 보냄  
	
REST의 특징은 다음과 같다.  

* Server-Client 구조  
	- Server(자원), Client(자원 요청) 형태의 구조, 덕분에 서로 간의 의존성이 줄어듬  
	
* Stateless(무상태)  
	- HTTP 프로토콜의 특징인 무상태성을 가짐  
	- 서버와 클라이언트 간의 데이터의 정보가 남지 않음 → 구현이 단순해짐  
	
* Cacheable(캐시 처리 기능)  
	- HTTP 프로토콜의 인프라를 활용하기 때문에 캐싱 기능을 적용 가능  
	- Last-Modified Tag or E-Tag를 이용하여 캐싱 구현  
	- 캐시를 통해 전체 응답시간, 성능, 서버의 자원 이용률을 향상  
	
* Layered System(계층화)  
	- Client는 REST API Server만 호출  
	- REST Server는 다중 계층으로 구성될 수 있음  
	- API Server는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성, 확장성, 보안성을 향상시킴  
	- PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용 가능  
	
* Code-On-Demand(optional)  
	- Server로부터 스크립트를 받아서 Client에서 실행, 반드시 충족할 필요는 없음  
	
* Uniform Interface(인터페이스 일관성)  
	- URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행  
	- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용 가능 → 특정 언어나 기술에 종속 x  
	
이번에는 REST API에 대해서 알아보자. REST는 앞에서 공부하였으니 API가 뭔지에 대해서 먼저 알고 넘어가자.  

API는 Application Programming Interface의 약자로써 **데이터와 기능의 집합을 제공하여 컴퓨터 프로그램 간 상호작용을 촉진하며, 서로 정보를 교환 가능하도록 하는 것**을 의미한다.  

REST API는 말 그대로 **REST 기반으로 구현한 API**를 의미한다. REST API는 확장성과 재사용성을 높여 유지 보수 및 운용을 편리하게 할 수 있으며, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다.  

누구나 쉽게 REST API를 이해하기 위해서 REST API를 설계할 때 따르는 기본, 세부 규칙들이 존재한다. 먼저 REST API 설계 기본 규칙이다.  

* URI는 정보의 자원을 표현해야 한다.  
	- 동사보다는 명사, 대문자보다는 소문자를 사용  
	- 도큐먼트 이름으로는 단수 명사를 사용, 컬렉션, 스토어 이름으로는 복수 명사를 사용  

![REST, REST API, RESTful3](https://user-images.githubusercontent.com/53072057/105569602-302a2000-5d86-11eb-9475-d755fcdb8d63.JPG)  

* 자원에 대한 행위는 HTTP Method로 표현한다.  
	- URI에 HTTP Method가 들어가면 안 됨  
	- URI에 행위에 대한 동사 표현이 들어가면 안 됨  
	- 경로 부분 중 변하는 부분은 유일한 값으로 대체  
	
다음으로 REST API 설계 세부 규칙이다.  

* 슬래시 구분자(/)는 계층 관계를 나타내는데 사용한다.  
* URI 마지막 문자로 슬래시(/)를 포함하지 않는다.  
* 하이픈(-)은 URI 가독성을 높이는데 사용한다.  
* 밑줄(_)은 URI에 사용하지 않는다.  
* URI 경로에는 소문자를 사용하도록 한다.  
* 파일 확장자는 URI에 포함하지 않는다.  
* 리소스 간에 연관 관계가 있는 경우 리소스들을 URI에 사용한다.  

마지막으로 RESTful이다.  

RESTful은 **일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어**이다. 간단하게 REST API를 제공하는 웹 서비스를 RESTful 하다고 표현할 수 있다. RESTful의 목적은 이해하기 쉽고 사용되기 쉬운 REST API를 만드는 것이다.  
