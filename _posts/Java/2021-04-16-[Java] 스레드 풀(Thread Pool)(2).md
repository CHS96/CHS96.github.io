---
title:  "[Java] 스레드 풀(Thread Pool)(2)"
excerpt: "[Java] 스레드 풀(Thread Pool)(2)을 알아보자!"

categories:
  - Java
  
last_modified_at: 2021-04-16T18:35:00
---

이번 글에서는 스레드 풀이 처리한 작업 결과를 애플리케이션에게 전달하는 **작업 완료 통보 받기**에 대해 알아보자.  

작업 완료 통보 받기는 블로킹 방식의 작업 완료 통보 받기, 작업 완료 순으로 통보 받기, 콜백 방식의 작업 완료 통보 받기, 3가지의 방법이 존재한다.  

먼저 **블로킹 방식의 작업 완료 통보 받기**이다.  

블로킹 방식은 요청한 결과가 올 때까지 기다리는 방식을 의미한다. 작업을 요청하는 메서드 중 submit() 메서드는 작업 처리 결과를 반환한다고 하였다. 이때 Future라는 객체를 반환한다.  

![스레드풀](https://user-images.githubusercontent.com/53072057/114973573-b18fdb00-9ebb-11eb-984a-fab30084cce0.JPG)  

​

Future 객체는 작업 결과가 아니라 **지연 완료(pending completion) 객체**라고 불리며 작업이 완료될 때까지 기다렸다가 최종 결과를 얻기 위해서 사용된다.  

get() 메서드를 호출하면 스레드가 작업을 완료할 때까지 블로킹되었다가 작업이 완료되면 해당 결과 V를 리턴하여 작업 완료를 통보받는다. 리턴타입 V는 submit(Runnable task, V result) or submit(Callable<V> task)의 V를 의미한다.  

![스레드풀1](https://user-images.githubusercontent.com/53072057/114973578-b2c10800-9ebb-11eb-86b4-384a6b52cf80.JPG)  


Future 객체는 get() 메서드 말고도 다른 메서드를 가진다.  

![스레드풀2](https://user-images.githubusercontent.com/53072057/114973580-b2c10800-9ebb-11eb-9523-9df3cfed5640.JPG)  


상황에 따라서 스레드가 작업한 결과를 외부 객체에 저장해야 할 경우가 있다. 이때 submit() 메서드의 반환 값인 Future<V> 객체를 공유 객체로 이용하여 작업의 결과를 누적해서 저장할 수 있다.  

예를 들어 작업 A는 1~5까지의 수를 더하는 작업이고 작업 B는 6~10까지의 수를 더하는 작업이라고 하자. 우리는 두 작업의 결과를 합쳐서 1~10까지의 수를 더하는 작업 결과를 얻고 싶다고 하자.  

![스레드풀3](https://user-images.githubusercontent.com/53072057/114973581-b3599e80-9ebb-11eb-8a8f-cd87b5d3403b.JPG)  


이는 Future<T>를 공유 객체로 활용해서 두 스레드의 작업 결과를 누적해서 저장해 주면 된다.  

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

//공유 객체
class Result {
	int accumValue;

	//공유 객체이기 때문에 동기화 처리
	synchronized void addValue(int value) {
		accumValue += value;
	}
}

public class Main {

	public static void main(String[] args) {
		ExecutorService executorService = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());

		System.out.println("[작업 처리 요청]");

		//작업 A : 1~5까지의 합
		class TaskA implements Runnable {
			Result result;

			TaskA(Result result) {
				this.result = result;
			}

			@Override
			public void run() {
				int sum = 0;
				for (int i = 1; i <= 5; ++i) {
					sum += i;
				}
				result.addValue(sum);
			}
		}

		//작업 B : 6~10까지의 합
		class TaskB implements Runnable {
			Result result;

			TaskB(Result result) {
				this.result = result;
			}

			@Override
			public void run() {
				int sum = 0;
				for (int i = 6; i <= 10; ++i) {
					sum += i;
				}
				result.addValue(sum);
			}
		}

		//공유 객체(외부 객체)
		Result result = new Result();
		//두개의 작업을 정의
		Runnable task1 = new TaskA(result);
		Runnable task2 = new TaskB(result);
		//두개의 작업을 작업 큐에 넣고 외부 객체에 저장
		Future<Result> future1 = executorService.submit(task1, result);
		Future<Result> future2 = executorService.submit(task2, result);

		try {
			result = future1.get();
			result = future2.get();
			System.out.println("[두 작업 처리 결과] " + result.accumValue);
			System.out.println("[작업 처리 완료]");
		} catch (Exception e) {
			System.out.println("[실행 예외 발생] " + e.getMessage());
		}

		executorService.shutdown();
	}
}
```

![스레드풀4](https://user-images.githubusercontent.com/53072057/114973583-b3599e80-9ebb-11eb-9943-5ef0f5567a5b.JPG)  


​

두 번째로 **작업 완료 순으로 통보 받기**이다.  

무조건 작업 요청 순서(작업 큐에 저장된 순서)대로 작업 처리가 완료되는 것은 아니다. 작업의 양과 스레드 스케줄링에 따라서 먼저 요청한 작업이 나중에 완료되는 경우도 발생할 수 있다.  

따라서 여러 개의 작업들이 순차적으로 처리될 필요성이 없고, 처리 결과도 순차적으로 이용할 필요가 없다면 작업 처리가 완료된 것부터 결과를 얻어 이용하는 것이 좋다.  

CompletionService의 poll()과 take() 메서드를 이용하면 스레드 풀에서 작업 처리가 완료된 작업만 통보받을 수 있다.  

![스레드풀5](https://user-images.githubusercontent.com/53072057/114973584-b3f23500-9ebb-11eb-9fcc-67d53b8e15ff.JPG)  


CompletionService의 객체 구현 방법은 인자 값으로 스레드 풀 객체인 ExecutorService 객체를 넘겨주면 된다.  

```java
ExecutorService executorService = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());
CompletionService<V> completionService = new ExecutorCompletionService<>(executorService);
```


주의해야 할 점은 poll()과 take() 메서드를 통해 작업 완료 순으로 통보받으려면 스레드 풀에 작업 처리 요청 시 ExecutorService의 submit() 메서드가 아닌 CompletionService의 submit() 메서드로 작업 처리 요청해야 한다.  

```java
executorService.submit(task); (x)
completionService.submit(task); (o)
```



마지막으로 **콜백 방식의 작업 완료 통보 받기**이다.  

콜백이란 애플리케이션이 스레드에게 작업 처리를 요청한 후, 다른 기능을 수행할 동안, 스레드가 작업을 완료하면 애플리케이션의 메서드를 자동 실행하는 기법을 말한다. 이때 자동 실행되는 메서드를 콜백 메서드라고 한다.  

블로킹 방식은 메인 스레드가 스레드 풀에게 작업 처리 요청하면 Future.get() 메서드를 통해 작업 완료를 통보받기 전까지 블로킹 상태로 기다려야 했다.  

하지만 콜백 방식은 메인 스레드가 스레드 풀에게 작업 처리 요청한 후 기다리지 않고 자기 일을 계속해서 진행하며 작업 처리가 완료될 시 콜백 메서드를 호출하게 하여 처리한 결과를 이용하도록 하는 차이가 있다.  

![스레드풀6](https://user-images.githubusercontent.com/53072057/114973585-b48acb80-9ebb-11eb-8b04-fc6056af9c86.JPG)  


콜백 방식을 사용하려면 콜백 메서드를 가지고 있는 콜백 객체를 생성해 줘야 한다.  

java.nio.channels.CompletionHandler 인터페이스를 활용해서 생성할 수 있다.  

completed() 메서드는 작업을 정상 처리 완료했을 때 호출되는 콜백 메서드이며 failed() 메서드는 작업 처리 도중 예외가 발생했을 때 호출되는 콜백 메서드이다.  

```java
CompletionHandler<V, A> callback = new CompletionHandler<V, A>() {
@Override
public void completed(V result, A attachment) {
	// TODO Auto-generated method stub
    }
};

@Override
public void failed(Throwable exc, A attachment) {
	// TODO Auto-generated method stub
	}
};
```


스레드에서 콜백 객체의 메서드를 호출하여 콜백 방식으로 작업 완료를 통보받을 수 있다.  

```java
Runnable task = new Runnable() {
	@Override
	public void run() {
		try {
			//작업 처리
			V result = ...;
			callback.completed(result, null); //작업 완료 통보
		} catch (Exception e) {
			callback.failed(e, null); //작업 실패 통보
		}
	}
};
​```



<h2>[ Reference ]</h2>  
* <https://www.youtube.com/watch?v=YYhcu8BQseg>  
