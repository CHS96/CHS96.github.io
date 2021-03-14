---
title:  "[Java] Optional : Null을 처리하는 방법"
excerpt: "[Java] Optional : Null을 처리하는 방법을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-03-14T18:35:00
---

Java에서는 null을 사용하여 존재하지 않는 객체를 표현한다.  

null 값을 사용함으로써 존재하지 않는 값을 표현할 수 있는 장점이 있지만 **NPE(NullPointerException)**이 발생할 수 있는 단점이 존재한다.  

NPE는 프로그램을 실행하는 **런타임** 시 발생하기 때문에 개발자들은 NPE를 사전에 발견하고 처리하기가 상당히 까다롭다.  

또한, Java는 모든 것이 객체로 이루어져 있다고 해도 무방할 정도로 무수히 많은 객체가 존재하기 때문에 NPE는 상당히 많은 곳에서 발생할 수 있으며 이를 모두 고려해주는 것은 힘들다.  

예를 들어 다음과 같이 Person과 University 클래스가 있다고 하자.  

```java
class Person {
	String name;
	int age;
	University university; //University 객체를 받음
    //getters & setters
}

class University {
	String universityName;
	int grade;
	String studentId;
    //getters & setters
}
```

Person 객체의 대학교 학번을 알아내기 위한 메서드를 하나 구현하였다.  

```java
public String getStudentID(Person person) {
    return person.getUniversity().getStudentId();
}
```

위의 코드는 다음과 같이 NPE에 상당히 많이 노출되어 있는 위험한 코드이다.  

```java
1. 메서드의 인자로 받는 person 객체가 null일 경우
2. person의 university 객체가 null일 경우
3. getStudentID 메서드의 반환 값이 null일 경우
//﻿3번의 경우 NPE는 아니지만 null 값을 반환함으로써 호출부에 NPE 위험을 전파시킬 수 있음
```

이러한 NPE를 해결해 주기 위해서 과거에는 if 문으로 null을 확인하는 방식으로 해결하였다.  

```java
public String getStudentID(Person person) {
    if (person == null) return "person is null"; //1번 null 처리
    University university = person.university;
    	
    if (university == null) return "university is null"; //2번 null 처리
    String studentId = university.getStudentId();
    	
    if (studentId == null) return "studentId is null"; //3번 null 처리
    return university.getStudentId();
}
```

하지만 이런 방법은 처리해야 할 값이 많아질수록 코드가 길어지고 가독성이 떨어지는 단점이 존재한다. 이를 개선하기 위해 Java 8부터 **Optional**이라는 개념이 등장하였다.  

![Java  Optional  Null을 처리하는 방법](https://user-images.githubusercontent.com/53072057/111058192-1e384400-84d0-11eb-83fc-fc5ffa68de65.JPG)  

즉, 객체를 관리하는 클래스마다 매번 if 문을 통해 처리해 주는 것을 Optioanl이라는 제네릭 클래스로 구현하여 모든 객체들을 감쌀 수 있도록 하여 NPE 처리해주는 방식이다.  

그렇다면 Optional 클래스의 기능들을 하나씩 살펴보자.  

먼저 Optional 객체를 생성하는 방법이다. Optional 클래스에서는 객체를 생성하는 방법으로 3가지 정적 팩토리 메서드를 제공해준다.  

먼저 Optional.empty() 메서드이다.  

![Java  Optional  Null을 처리하는 방법1](https://user-images.githubusercontent.com/53072057/111058193-1f697100-84d0-11eb-8585-a4d301a4801a.JPG)  

EMPTY라는 객체를 생성해 주는데 이는 말 그대로 텅 빈 Optional 객체를 의미한다.  

![Java  Optional  Null을 처리하는 방법2](https://user-images.githubusercontent.com/53072057/111058195-20020780-84d0-11eb-8d68-6ef0591e7857.JPG)  

두 번째는 Optional.of() 메서드이다.  

![Java  Optional  Null을 처리하는 방법3](https://user-images.githubusercontent.com/53072057/111058196-20020780-84d0-11eb-8bf6-3608be78bebf.JPG)  

value를 가지는 Optional 객체를 생성해준다. value를 필수로 가지는 Optioanl 생성자는 다음과 같이 requireNonNull() 메서드를 통해 value를 받는다.  

![Java  Optional  Null을 처리하는 방법4](https://user-images.githubusercontent.com/53072057/111058197-209a9e00-84d0-11eb-92ff-1b76b8fa246d.JPG)  

requireNonNull 메서드는 value로 null 값을 받지 않는다는 의미이다.  

![Java  Optional  Null을 처리하는 방법5](https://user-images.githubusercontent.com/53072057/111058198-209a9e00-84d0-11eb-9ed8-e7f70debab6e.JPG)  

즉, Optional.of()은 null이 아닌 객체를 생성해주는 방법이다.  

마지막으로 Optional.ofNullable() 메서드이다.  

![Java  Optional  Null을 처리하는 방법6](https://user-images.githubusercontent.com/53072057/111058199-21333480-84d0-11eb-9eb1-ba7edc380cf5.JPG)  

empty(), of() 메서드를 통해 value를 결정하여 Optional 객체를 생성해준다. 즉, 객체가 null 일 수도 있고 아닐 수도 있는 경우 사용하는 것으로 위의 두 방법을 합쳐놓은 방법이라고 생각하면 된다.  

```java
Optional<Person> optional1 = Optional.empty();
Optional<Person> optional2 = Optional.of(new Person("Libi", 26, new University()));
Optional<Person> optional3 = Optional.ofNullable(null);    	
```

다음으로 Optional 객체를 생성하였으니 해당 객체를 접근하는 방법이다.  

첫 번째로 get() 메서드이다. 해당 value가 존재하면 반환하고 null, 즉  비어있는 객체라면 Exception을 던진다.  

![Java  Optional  Null을 처리하는 방법7](https://user-images.githubusercontent.com/53072057/111058200-21333480-84d0-11eb-9d6b-7a21246cdc65.JPG)  

두 번째는 orElse() 메서드이다.  

![Java  Optional  Null을 처리하는 방법8](https://user-images.githubusercontent.com/53072057/111058201-21cbcb00-84d0-11eb-83aa-acdd9afe564a.JPG)  

get() 메서드는 null일 경우 Exception을 던졌지만 orElse() 메서드는 null일 경우 인자로 받은 other 객체를 반환해준다. other 객체는 null 값이어도 상관없다.  

세 번째로 orElseGet() 메서드이다.  

![Java  Optional  Null을 처리하는 방법9](https://user-images.githubusercontent.com/53072057/111058202-21cbcb00-84d0-11eb-8cb9-6b6938325962.JPG)  

orElse()가 어떤 객체를 반환했다면 orElseGet()은 인자로 supplie 메서드를 넣어서 해당 람다 표현식이 반환하는 결괏값(객체)을 받아온다.  

마지막으로 orElseThrow() 메서드이다.  

![Java  Optional  Null을 처리하는 방법10](https://user-images.githubusercontent.com/53072057/111058203-22646180-84d0-11eb-9297-1e6e424997aa.JPG)  

내부 객체를 반환하되 존재하지 않으면 파라미터에 있는 Exception을 던진다.  


다음으로 현재 value가 존재하는지를 판단하는 메서드이다.  

먼저 isPresent() 메서드이다. 단순하게 value가 null 인지를 판단해준다.  

![Java  Optional  Null을 처리하는 방법11](https://user-images.githubusercontent.com/53072057/111058204-22646180-84d0-11eb-9a4d-2767b3b0ed3c.JPG)  

다음으로 ifPresent() 메서드이다. 이는 만약 value가 null이 아닐 경우 실행될 로직을 함수형 인자로 넘겨줄 수 있다.  

![Java  Optional  Null을 처리하는 방법12](https://user-images.githubusercontent.com/53072057/111058205-22fcf800-84d0-11eb-9e5b-ab013e254d80.JPG)  

​

사실 Optional의 장점은 ifPresent() 메서드처럼 함수형 인자를 넘겨줌으로써 Stream처럼 사용할 수 있다는 점이다.  

Optional 역시 함수형 언어의 접근 방식에서 영감을 받아 생성된 친구이기 때문에 내부 메서드로 Stream 클래스가 가지고 있는 map, flatMap, filter와 같은 메서드를 가지고 있다.  

먼저 map() 메서드이다. 어떤 T의 값을 R로 변경시켜주는 메서드다. 여기서 R는 그냥 객체를 의미한다.  

![Java  Optional  Null을 처리하는 방법13](https://user-images.githubusercontent.com/53072057/111058206-22fcf800-84d0-11eb-8837-8813f74ad878.JPG)  

다음으로 flatMap() 메서드이다. map과 똑같이 T를 R로 변경시켜주는 메서드이지만 여기서 R는 그냥 객체가 아닌 Optional 객체를 의미한다.  

![Java  Optional  Null을 처리하는 방법14](https://user-images.githubusercontent.com/53072057/111058207-23958e80-84d0-11eb-86e8-6d6a4811e0cd.JPG)  

마지막으로 filter() 메서드이다. Stream에서는 여러 데이터를 특정 조건인 filter로 걸러내는 작업을 수행했다면 Optional의 filter는 특정 조건을 만족하지는 확인하는 용도로 사용된다.  

![Java  Optional  Null을 처리하는 방법15](https://user-images.githubusercontent.com/53072057/111058208-23958e80-84d0-11eb-8bdb-6184b8d7fa8c.JPG)  

*****

즉, Optional은 최대 1개의 원소를 가지고 있는 특별한 Stream으로 생각하면 쉽다. 따라서 Stream 방식처럼 코드를 간결하게 할 수 있다.  

위의 studentID를 반환하는 메서드를 Optional을 적용하면 다음과 같이 간결하고 가독성이 높도록 구현할 수 있다.  

```java
public String getStudentID(Person person) {
    if (person == null) return "person is null"; //1번 null 처리
    University university = person.university;
    	
    if (university == null) return "university is null"; //2번 null 처리
    String studentId = university.getStudentId();
    	
    if (studentId == null) return "studentId is null"; //3번 null 처리
    return university.getStudentId();
}

---------------------------------------------------------------

public String getStudentID(Person person) {
	return Optional.ofNullable(person)
			.map(Person::getUniversity)
			.map(University::getStudentId)
			.orElse("studentId is null");
}
```



<h2>[ Reference ]</h2>  
* <https://www.daleseo.com/java8-optional-before/>  
* <https://jeong-pro.tistory.com/169>  
