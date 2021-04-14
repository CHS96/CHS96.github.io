---
title:  "[Java] 스레드 풀(Thread Pool)(1)"
excerpt: "[Java] 스레드 풀(Thread Pool)(1)을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-04-14T18:35:00
---

일반적으로 하나의 프로세스는 하나의 스레드를 가지고 작업을 수행하게 되지만 다수의 작업이 요청되면 CPU는 이를 보다 효율적으로 처리하기 위해서 병렬 작업 처리를 위해 **멀티 스레드 방식**이라고 부르는 다수의 스레드를 생성하여 처리하게 된다.  

문제는 매번 작업이 요청될 때마다 스레드를 생성하고 작업에 스케줄링하는 것은 CPU가 바빠지며, 메모리 사용량이 늘어나기 때문에 애플리케이션의 성능이 급격히 저하된다.  

이러한 문제를 해결하기 위해서 **스레드 풀(Thread Pool)**이라는 것을 사용한다.  

스레드 풀은 스레드를 미리 생성해놓은 공간이라고 생각하면 쉽다. 작업 요청이 있을 때마다 매번 스레드를 생성하지 말고 **제한된 개수만큼 스레드를 미리 생성해놓고 작업 큐(Queue)에 들어오는 작업들을 하나씩 스레드가 맡아서 처리하는 방식**이다.  

작업 처리가 끝난 스레드는 작업 결과를 애플리케이션에게 전달하고 다시 작업 큐에 있는 작업을 가져와서 처리한다.  

![스레드풀1](https://user-images.githubusercontent.com/53072057/114653386-e61c5f00-9d22-11eb-8a8c-e0283170630d.JPG)  


Java에서는 java.util.concurrent 패키지 내의 **ExecutorService** 인터페이스와 **Executors** 클래스를 제공해 주며 Executors 클래스의 정적 메서드를 통해 스레드 풀인 ExecutorService 객체를 생성하고 사용할 수 있다.  

스레드 풀을 생성하는 정적 메서드는 대표적으로 newCachedThreadPool()과 newFixedThreadPool(int nThreads) 메서드가 존재한다.  

![스레드풀2](https://user-images.githubusercontent.com/53072057/114653389-e74d8c00-9d22-11eb-9059-0bdfe1aa37b0.JPG)  

​
또한, 이들은 내부적으로 ThreadPoolExecutor 객체를 생성해 준다. ThreadPoolExecutor 객체의 인자 값을 보면 두 정적 메서드의 차이점을 알 수 있다.  

![스레드풀3](https://user-images.githubusercontent.com/53072057/114653390-e7e62280-9d22-11eb-8f78-980afccb9487.JPG)  


corePoolSize는 코어 스레드 수를 뜻하며 더 이상 작업이 없더라도 스레드 풀에 최소한 유지해야 할 스레드 수를 뜻하며 maximumPoolSize은 스레드 풀에서 관리하는 최대 스레드의 개수를 의미한다.  

![스레드풀4](https://user-images.githubusercontent.com/53072057/114653391-e7e62280-9d22-11eb-8b47-f9e3551a2616.JPG)  


newCachedThreadPool()는 작업 요청이 많아질수록 운영체제의 상황에 따라 스레드가 계속해서 추가되지만 스레드가 60초 동안 아무런 작업을 수행하지 않으면 추가된 스레드를 종료하고 풀에서 제거한다.  

newFixedThreadPool(int nThreads)는 작업 요청이 많아져도 nThreads 개수만큼만 스레드가 추가되며 추가된 스레드가 아무런 작업을 수행하지 않더라도 스레드 개수가 줄지 않는다.  

```java
ExecutorService executorService1 = Executors.newCachedThreadPool();
ExecutorService executorService2 = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());
ExecutorService executorService3 = new ThreadPoolExecutor(0, 10, 60L, TimeUnit.MICROSECONDS, new SynchronousQueue<>());
```


스레드 풀은 데몬 스레드(main 스레드가 종료되면 종료되는 스레드)가 아니기 때문에 더 이상 사용하지 않을 경우 스레드 풀을 종료시켜줘야 한다.  

스레드 풀을 종료하는 방법은 크게 3가지가 존재한다. 웬만하면 shutdown() 메서드를 사용하는 것이 좋다.  

![스레드풀5](https://user-images.githubusercontent.com/53072057/114653392-e87eb900-9d22-11eb-8253-2235b2427c6a.JPG)  


이제 스레드 풀을 구현했다면 작업을 생성하여 작업 큐에 넣어줘야 한다. 작업은 **Runnable or Callable** 객체로 생성한다. 두 인터페이스의 차이점은 **작업 처리 완료 후 리턴 값의 유무**이다.  

```java
Runnable runnable = new Runnable() {
	@Override
	public void run() {
		//do Something
	}
};
	
Callable<T> callable = new Callable<T>() {
	@Override
	public T call() throws Exception {
		//do Something
		return T;
	}
};
```

스레드 풀의 스레드는 작업 큐에서 Runnable or Callable 객체를 가져와 run() or call() 메서드를 실행하여 해당 작업을 처리해 준다.  


작업을 생성하였으니 스레드 풀의 작업 큐에 넣어줘야 한다. ExecutorService는 작업 큐에 작업을 넣는 두 가지 메서드를 제공한다.  

![스레드풀6](https://user-images.githubusercontent.com/53072057/114653393-e87eb900-9d22-11eb-91da-d8c721491524.JPG)  


두 메서드의 차이는 **작업 결과 반환 유무**이다. 또한 작업 처리 도중 **예외가 발생할 경우** 차이도 존재한다.  

execute()는 예외가 발생할 경우 해당 스레드를 종료 및 제거한다. 따라서 다른 작업을 처리하기 위해선 새로운 스레드를 생성해야 한다.  

이에 반해 submit()은 예외가 발생할 경우 해당 작업을 버리고 다음 작업을 처리하도록 한다.  

따라서 스레드를 제거하고 생성하는 비용이 적은 편이 아니기 때문에 submit()을 사용하는 편이 좋다.  


이번 글을 통해 스레드 풀의 필요성과 스레드 풀의 동작 과정 중 1, 2, 3번에 해당하는 생성/종료, 작업 생성/처리 요청 방법에 대해서 공부하였다.  

다음 글에서는 4번 과정에 해당하는 스레드 풀이 처리한 작업 결과를 애플리케이션에게 전달하는 작업 완료 통보받기에 대해서 알아보도록 하자.  




<h2>[ Reference ]</h2>  
* <https://www.youtube.com/watch?v=YYhcu8BQseg>  
