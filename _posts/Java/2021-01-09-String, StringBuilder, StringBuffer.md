---
title:  "String, StringBuilder, StringBuffer"
excerpt: "String, StringBuilder, StringBuffer의 차이점을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-01-09T18:35:00
---

String, StringBuilder, StringBuffer 모두 Java에서 **문자열을 관리하는 클래스**이다. 알고리즘 문제에서 문자열 연산이 자주 발생하는 문제인 경우 String을 사용한다면 시간 초과가 발생하는 경우가 상당히 많을 것이다. 이를 해결하기 위해선 String이 아닌 StringBuilder나 StringBuffer 클래스를 사용하여 문자열 연산을 처리해야 한다.  

각각 어떤 것인지 대충 알고 있었지만 이번 기회에 한 번 정리해보려고 한다.  

우선 문자열 연산시 String은 시간 초과가 나고 StringBuilder와 StringBuffer는 시간 초과가 나지 않는 차이점을 먼저 짚고 넘어가 보자.  

String으로 선언한 객체는 **불변(immutable)하는 객체**이다. 이게 무슨 뜻이냐면 new 연산을 통해 String 객체를 선언하면 힙 메모리에 할당되면서 이 객체의 메모리 공간은 절대로 변하지 않는다는 것이다. 만약 String 객체에 다른 String 객체를 합치는 연산을 하기 위해선 String 객체를 새롭게 선언한 후 이 객체에 두 String 객체를 합쳐야 한다.  

![s1](https://user-images.githubusercontent.com/53072057/104079922-9a13d700-5268-11eb-859a-6aa662fad84d.JPG)  

문자열을 합칠 때마다 새로운 String 객체를 생성해야 하고 새로운 String 객체에 현재의 문자열의 크기만큼 옮겨주는 연산을 해줘야 하기 때문에 매번 **O(문자열의 크기)**의 시간 복잡도를 가진다. 또한 사용되지 않는 String 객체는 가비지 컬렉터에 의해 제거돼야 하기 때문에 String을 이용해서 문자열을 합치는 방법은 상당히 비효율적이다.  

다만, String으로 선언한 객체는 불변하기 때문에 **단순 조회 연산에서는 상당히 효율적이며 다중 스레드 환경에서 동기화를 신경 쓸 필요가 없는 장점**이 있다.  

이와 달리 StringBuilder와 StringBuffer로 선언한 객체는 **가변(Mutable)하는 객체**이다. new 연산을 통해 객체를 한 번만 생성한 후 문자열 연산이 발생하게 되면 새로운 객체를 생성하는 것이 아니라 이미 선언한 객체의 메모리 공간을 변경하여 처리하는 방식이다. 정적 배열과 동적 배열을 생각하면 이해하기 쉬울 것이다.  

![s2](https://user-images.githubusercontent.com/53072057/104079923-9aac6d80-5268-11eb-8ce2-9b16439e27cb.JPG)  

그렇다면 StringBuilder와 StringBuffer 클래스의 차이점은 무엇일까? 둘의 차이점은 **다중 스레드 환경에서 동기화를 지원하는 여부**이다. StringBuilder 클래스는 동기화를 지원하지 않기 때문에 다중 스레드 환경에서 사용하기에는 부적합한 반면 StringBuffer 클래스는 동기화를 지원하기 때문에 다중 스레드 환경에서 사용하기에 적합하다.  

따라서 단일 스레드를 사용하는 일반적인 알고리즘 문제에서 문자열 연산이 많이 발생하는 경우 StringBuilder 클래스가 StringBuffer 클래스보단 빠르기 때문에 StringBuilder 클래스를 사용한다면 효율적으로 문자열 연산을 수행할 수 있을 것이다.  
