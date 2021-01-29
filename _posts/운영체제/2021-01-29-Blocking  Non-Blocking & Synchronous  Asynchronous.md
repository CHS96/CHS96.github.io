---
title:  "Blocking / Non-Blocking & Synchronous / Asynchronous"
excerpt: "Blocking / Non-Blocking & Synchronous / Asynchronous의 차이를 알아보자!"

categories:
  - 운영체제
  
last_modified_at: 2021-01-29T18:35:00
---

공부를 하다가 Blocking / Non-Blocking & Synchronous / Asynchronous 내용을 접하게 되었다. 각각 대충 어떤 의미인지 가볍게 알고 있었으며 Blocking-Synchronous,  Non-Blokcing-Asynchronous가 비슷한 개념?으로 알고 있었다. 하지만 당연하게도 다른 개념이니 용어를 구분하였을 것이다. 따라서 이번 기회에 한 번 공부 및 정리해보려고 한다.  

우선 각 용어들의 개념과 차이에 대해서 이해하고 넘어가자.  

Blokcing / Non-Blocking과 Synchronous / Asynchronous는 **관심사 관점**으로 구분할 수 있다.  

![BNSA1](https://user-images.githubusercontent.com/53072057/106227137-095b6600-622c-11eb-8c70-e9f7100c2d10.JPG)  

![BNSA2](https://user-images.githubusercontent.com/53072057/106227139-0a8c9300-622c-11eb-91c5-be741eee8ed0.JPG)  

관심사 관점뿐만 아니라 **입장**을 통해서도 구분할 수 있다.  

![BNSA3](https://user-images.githubusercontent.com/53072057/106227142-0a8c9300-622c-11eb-979c-01ce561ba015.JPG)  

그리고 Async와 NonBlocking은 **동작 관점**에서도 구분할 수 있다.  

![BNSA4](https://user-images.githubusercontent.com/53072057/106227144-0b252980-622c-11eb-80de-24ce2037a64d.JPG)  

각 개념들을 살펴보면 Synchronous & Blocking은 자기 마음대로 못하는 느낌이고 Asynchronous & Non-Blocking은 상대의 입장은 크게 상관 쓰지 않고 자기 마음대로 할 수 있는 느낌인 것 같다. 그래서 아래의 그림처럼 Sync-Blocking과 Async-NonBlocking의 조합은 나름 쉽게 이해할 수 있다.  

![BNSA5](https://user-images.githubusercontent.com/53072057/106227145-0bbdc000-622c-11eb-9a6c-0ac551da5b5b.JPG)  

하지만 칸을 2x2칸으로 나눈 것은 Sync-NonBlocking과 Async-Blocking의 조합도 가능하다는 것이다. 그렇다면 남은 2개의 조합에 대해서 알아보자.  

먼저 **Sync-NonBlocking 조합**이다. 이는 호출되는 함수는 제어권을 바로 리턴하고, 호출하는 함수는 호출되는 함수의 작업 완료 여부를 신경 쓰는 상태이다. 신경 쓰는 방법은 **기다리거나(Wait) 물어보는(Question)** 두 가지 방법이 존재하는데, NonBlocking 함수를 호출했다면 사실 기다릴 필요는 없고 물어보는 일만 하면 된다.  

즉, **호출한 함수는 제언권을 받았기 때문에 다른 작업을 할 수 있게 되지만, 호출된 함수의 작업이 완료된 것이 아니기 때문에, 다른 작업을 하면서 호출된 함수에게 작업 완료 여부를 계속해서 물어보는 상태**이다.  

![BNSA6](https://user-images.githubusercontent.com/53072057/106227148-0bbdc000-622c-11eb-9eee-ed544db946c9.JPG)  

Sync-NonBlocking 조합을 사용하는 예시로는 future.isDone()이 있다.  

```java
Future ft = asyncFileChannel.read(~~~);

while(!ft.isDone()) {
    // isDone()은 asyncChannle.read() 작업이 완료되지 않았다면 false를 바로 리턴해준다.
    // isDone()은 물어보면 대답을 해줄 뿐 작업 완료를 스스로 신경쓰지 않고,
    // isDone()을 호출하는 쪽에서 계속 isDone()을 호출하면서 작업 완료를 신경쓴다.
    // asyncChannle.read()이 완료되지 않아도 여기에서 다른 작업 수행 가능 
}

// 작업이 완료되면 작업 결과에 따른 다른 작업 처리
```

다음으로 **Async-Blocking 조합**이다. 이는 호출되는 함수가 제어권을 바로 리턴하지 않았으며, 호출하는 함수는 호출된 함수의 작업 완료 여부를 신경 쓰지 않는 상태이다.  

즉, **호출된 함수의 작업 완료 여부를 몰라도 상관은 없지만 제어권이 없기 때문에 다른 작업을 할 수 없는 상태**이다.  

![BNSA7](https://user-images.githubusercontent.com/53072057/106227149-0c565680-622c-11eb-9537-b3d625193227.JPG)  

Async-Blocking 조합은 사실상 이점이 별로 없는 조합이라 이 방식을 사용하는 경우는 극히 드문데, 의도하지 않게 Async-Blocking 조합으로 동작하는 경우가 있다고 한다. 원래는 Async-NonBlocking 방식을 추구하다가 의도와는 다르게 Async-Blocking 방식으로 되어버리는 경우라고 하는데 바로 Node.js와 MySQL의 조합이다.  

Node.js 쪽에서 Async 상태를 유지해도, 결국 DB 작업 호출 시에는 MySQL에서 제공하는 드라이버를 호출하게 되는데, 이 드라이버가 Blocking 방식이라고 한다. Java의 JDBC도 이러한 방식이다.  

따라서 Async-Blocing은 별다른 장점이 없어서 일부로 사용할 필요는 없지만, Async-NonBlocking을 사용하는 과정에서 하나라도 Blocking 방식으로 동작한다면 의도하지 않게 Async-Blocking으로 동작할 수 있다.  


<h2>[ Reference ]</h2>  
* <http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/>  