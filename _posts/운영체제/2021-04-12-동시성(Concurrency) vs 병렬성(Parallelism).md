---
title:  "동시성(Concurrency) vs 병렬성(Parallelism)"
excerpt: "동시성(Concurrency) vs 병렬성(Parallelism)을 알아보자!"

categories:
  - 운영체제
  
last_modified_at: 2021-04-12T18:35:00
---

동시성과 병렬성은 전혀 다른 개념이지만 헷갈리기 쉬운 용어라 혼동되어 사용하는 경우가 많다. 이번 기회에 이 둘에 대해서 확실하게 개념을 정리하도록 하자.  

음료를 판매하는 카페를 예시로 두 개념의 차이를 알아보자.  

먼저 **동시성(Concurrency)**이다. 동시성은 말 그대로 **동시에 작업을 수행하는 것**을 의미한다. 카페에서 손님이 음료를 주문하게 되면 직원은 결제 후 음료를 만들어서 제공한다.  

이는 CPU에 해당하는 직원이 주문, 결제, 생산이라는 3가지의 프로세스를 진행하여 최종적으로 손님에게 음료를 제공한다고 이해할 수 있다.  

![동시성(Concurrency) vs 병렬성(Parallelism)](https://user-images.githubusercontent.com/53072057/114364376-4ccf3a80-9bb4-11eb-8c01-d6cf38df4d5d.JPG)  

만약 여러 명의 손님이 대기하고 있다고 하자. CPU는 한 번에 하나의 프로세스만 수행할 수 있다. 현재 손님이 주문한 음료가 만드는데 시간이 많이 걸리는 음료라면 뒤에 있는 손님들은 무작정 대기해야 하는 상황이 발생하게 된다.  

이러한 비효율적인 상황을 어떻게 개선할 수 있을까? 특정 음료가 오래 걸릴 경우 주문-결제-생산 프로세스를 무조건적으로 따를 필요가 있을까?  

내가 만약 직원이라면 한 번에 한 개의 음료를 생산하기보다는 여러 개의 주문을 한 번에 받아 결제를 한 후 함께 생산하는 식으로 수행할 것이다. 이것이 바로 동시성이다.  

![동시성(Concurrency) vs 병렬성(Parallelism)1](https://user-images.githubusercontent.com/53072057/114364380-4d67d100-9bb4-11eb-99b7-95e8fd32dafd.JPG)  



다음은 **병렬성(Parallelism)**이다. 병렬성은 **병렬처리**를 의미한다. 직원이 한 명이기 때문에 한 명의 직원이 모든 작업을 수행했다면 병렬성은 여려 명의 직원을 둬서 작업을 보다 효율적으로 수행하는 것을 의미한다.  

즉, 여러 개의 CPU로 병렬처리를 수행하는 것을 의미한다.  

![동시성(Concurrency) vs 병렬성(Parallelism)2](https://user-images.githubusercontent.com/53072057/114364384-4e006780-9bb4-11eb-9718-f42e0e5799f5.JPG)  



이를 더욱 직관적으로 표현한 그림은 다음과 같다.  

![동시성(Concurrency) vs 병렬성(Parallelism)3](https://user-images.githubusercontent.com/53072057/114364385-4e98fe00-9bb4-11eb-86c3-0624070740f1.JPG)  


![동시성(Concurrency) vs 병렬성(Parallelism)4](https://user-images.githubusercontent.com/53072057/114364389-4e98fe00-9bb4-11eb-9e22-0fa9e394c92e.JPG)  



<h2>[ Reference ]</h2>  
* <https://seamless.tistory.com/42>   