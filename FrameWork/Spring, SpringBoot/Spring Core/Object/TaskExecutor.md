---
tags:
  - Spring
  - Core
  - Object
  - Thread
---
## TaskExecutor
 `TaskExecutor`는 단순히 비동기 작업을 시행하기 위한 Interface입니다.
 Spring에서는 Executor 인터페이스를 구현하는 것으로 단일화 했는데요.
 
 Executor API는 의외로 간단합니다.
 
```java title:"Executor.java"
package java.util.concurrent;

public interface Executor {
	void execute(Runnable command);
}
```

## ExecutorService
`ExecutorService`는 Thread의 관리 기능이 강화된 인터페이스로 `shutdown()`처럼 Thread에 이벤트를 일으키는 메서드를 제공합니다.

`ExecutorService`에는 `Future\<T\>`형 객체를 반환하는 `submit()` 메서드가 있습니다.
`Future\<T\>` 인스턴스는 대게 비동기 실행 Thread의 진행 상황을 추적하는 용도로 사용됩니다. 예를 들어 `Future.isDone()`, `Future.isCancleed()`는 각각 어떠한 작업이 끝나고 취소되었는지를 확인하는 용도로 사용됩니다.

```java
Runnable task = new Runnable() {  
    @Override  
    public void run() {  
        try {  
            Thread.sleep(1000);  
            System.out.println("finish");  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }  
    }  
};  
  
ExecutorService executorService = Executors.newCachedThreadPool();  
if(executorService.submit(task, Boolean.TRUE).get().equals(Boolean.TRUE)){  
    System.out.println("Job has finished");  
}
```

아직 이해가 잘 안될 수 있습니다.
다른 예시를 들어보겠습니다.

Runnable을 응용해 시간 경과를 나타내는 Class 입니다.

```java title:"DemostractionRunnable.java"
public class DemonstrationRunnable implements Runnable {

	@Override
	public void run() {
		try {
			Thread.sleep(1000)
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		System.out.println(Thread.currentThread().getName());
		System.out.printf("Hello at %s \n", new Data());
	}
}
```

```java title:"ExecutorsDemo.java"
public class ExecutorsDemo {  
    public static void main(String[] args) throws Throwable{  
        Runnable task = new DemonstrationRunnable();  
  
        ExecutorService cachedThreadPoolExecutorService = Executors.newCachedThreadPool();  
  
        if(cachedThreadPoolExecutorService.submit(task).get() == null) {  
            System.out.printf("The cachedThreadPoolExecutorService has succeeded at %s \n", new Date());  
        }  
  
        ExecutorService fixedThreadPool = Executors.newFixedThreadPool(100);  
        if(fixedThreadPool.submit(task).get() == null) {  
            System.out.printf("fixedThreadPool has succeeded at %s \n", new Date());  
        }  
  
        ExecutorService singleThreadExecutorService = Executors.newSingleThreadExecutor();  
        if(singleThreadExecutorService.submit(task).get() == null) {  
            System.out.printf("singleThreadExecutorService has succeeded at %s \n", new Date());  
        }  
  
        ExecutorService es = Executors.newCachedThreadPool();  
        if(es.submit(task, Boolean.TRUE).get().equals(Boolean.TRUE)) {  
            System.out.println("Job has finished");  
        }  
  
		// 10초 후에 해당 Thread를 동작시키는 .schedule() 함수  
		ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(10);  
		if(scheduledExecutorService.schedule(task, 10, TimeUnit.SECONDS).get() == null) {  
		    System.out.printf("scheduledExecutorService has succeeded at %s \n", new Date());  
		}  
		  
		// 5초마다 구현된 Runnable을 동작시키는 .scheduleAtFixedRate() 함수  
		scheduledExecutorService.scheduleAtFixedRate(task, 0, 5, TimeUnit.SECONDS);
	}
}
```

결과가 다음과 같이 나온다면 정상적으로 동작한 것입니다.

```java
pool-1-thread-1
Hello at Mon Jun 03 13:15:17 KST 2024 
The cachedThreadPoolExecutorService has succeeded at Mon Jun 03 13:15:17 KST 2024 
pool-2-thread-1
Hello at Mon Jun 03 13:15:20 KST 2024 
fixedThreadPool has succeeded at Mon Jun 03 13:15:20 KST 2024 
pool-3-thread-1
Hello at Mon Jun 03 13:15:23 KST 2024 
singleThreadExecutorService has succeeded at Mon Jun 03 13:15:23 KST 2024 
pool-4-thread-1
Hello at Mon Jun 03 13:15:26 KST 2024 
Job has finished
pool-4-thread-2
Hello at Mon Jun 03 13:15:26 KST 2024 
Job has finished
pool-4-thread-3
Hello at Mon Jun 03 13:15:36 KST 2024 
Job has finished
```

이해가 되셨나요??
Thread 실제로 많이 사용해보지 않으면 이해하기 어렵다고들 합니다.
저도 아직 완벽하게 이해가 가지는 않는 부분이라... 더 자세히 설명하기는 벅차지만,
조금 더 설명을 이어나가 볼게요!

이번에는 `Callable\<T\>`에 관한 설명입니다.
`Callable\<T\>`자체 보다는 `ExecutorService`에 있는`submit(Callable\<T\> call)`이라는 메서드를 설명하려고 합니다.

`Runnable`이 아닌 `Callable`을 인자로 받은 `submit()`함수를 실행하게 되면 `Callable`의 `call()` 메서드가 반환한 값을 그대로 다시 반환하게 이는 실행한 작업의 결과를 반환하는데 중점을 두고 있습니다.

```java title:"Callable.java"
package java.util.concurrent;

public intrerface Callable<V> {
	V call() throws Exception;
}

// Callable을 구현하게 되면 다음과 같이 구현하게 됩니다.
Callable<String> str = new Callable<String>() {  
    @Override  
    public String call() throws Exception {  
        return null;  
    }  
}
```

## TaskExecutor
`TaskExecutor`는 `Excutor`를 베이스로 한 Interface입니다.
기존 Java SE의 `Executor`나 `ExecutorService`를 `TaskExecutor`에서 사용할 수 있게 지원하는 경우도 있지만, 어차피 스프링 관점에서는 베이스 클래스가 `Executor`이므로 별로 중요하치 않습니다.

```java title:"TaskExecutor.java"
package org.springframwork.task;
...

public interface TaskExecutor extends Executor {
	void execute(Runnable task);
}
```

앞서 사용했던 `DemonstrationRunnable`를 `TaskExecutor`에 넣어 한번 살표보도록 하죠.

```java title:"SpringExecutorsDemo.java"
@Component
public class SpringExecutorsDemo {
	@Autowired  
	private SimpleAsyncTaskExecutor asyncTaskExecutor;  
	  
	@Autowired  
	private SyncTaskExecutor syncTaskExecutor;  
	  
	@Autowired  
	private TaskExecutorAdapter taskExecutorAdapter;  
	  
	@Autowired  
	private ThreadPoolTaskExecutor threadPoolTaskExecutor;  
	  
	@Autowired  
	private DemonstrationRunnable task;  
	  
	@PostConstruct  
	public void submitJobs() {  
	    syncTaskExecutor.execute(task);  
	    taskExecutorAdapter.submit(task);  
	    asyncTaskExecutor.submit(task);  
	  
	    for (int i = 0; i < 500; i++) {  
	        threadPoolTaskExecutor.submit(task);  
	    }  
	}  
	  
	public static void main(String[] args) {  
	    new AnnotationConfigApplicationContext(ExecutorsConfiguration.class).registerShutdownHook();  
	}
}
```

```java title:"ExecutorsConfiguration.java"
@Configuration  
@ComponentScan  
public class ExecutorsConfiguration {  
  
    @Bean  
    public TaskExecutorAdapter taskExecutorAdapter() {  
        return new TaskExecutorAdapter(Executors.newCachedThreadPool());  
    }  
  
    @Bean  
    public SimpleAsyncTaskExecutor simpleAsyncTaskExecutor() {  
        return new SimpleAsyncTaskExecutor();  
    }  
  
    @Bean  
    public SyncTaskExecutor syncTaskExecutor() {  
        return new SyncTaskExecutor();  
    }  
  
    @Bean  
    public ScheduledExecutorFactoryBean scheduledExecutorFactoryBean(ScheduledExecutorTask scheduledExecutorTask) {  
        ScheduledExecutorFactoryBean scheduledExecutorFactoryBean = new ScheduledExecutorFactoryBean();  
        scheduledExecutorFactoryBean.setScheduledExecutorTasks(scheduledExecutorTask);  
        return scheduledExecutorFactoryBean;  
    }  
  
    @Bean  
    public ScheduledExecutorTask scheduledExecutorTask(Runnable runnable) {  
        ScheduledExecutorTask scheduledExecutorTask = new ScheduledExecutorTask();  
        scheduledExecutorTask.setPeriod(1000);  
        scheduledExecutorTask.setRunnable(runnable);  
        return scheduledExecutorTask;  
    }  
  
    @Bean  
    public ThreadPoolTaskExecutor threadPoolTaskExecutor() {  
        ThreadPoolTaskExecutor taskExecutor = new ThreadPoolTaskExecutor();  
        taskExecutor.setCorePoolSize(50);  
        taskExecutor.setMaxPoolSize(100);  
        taskExecutor.setAllowCoreThreadTimeOut(true);  
        taskExecutor.setWaitForTasksToCompleteOnShutdown(true);  
        return taskExecutor;  
    }
```

위에 `SpringExecutorsDemo.java`에서 사용한 객체들이 보이시나요??
`TaskExecutor` Interface의 다양한 구현체들이죠.
* `TaskExcutorAdapter`
	* java.util.concurrence.Executors 인스턴스를 감싼 단순 래퍼라서 Spring을 이용해 TaskExecutor Interface와 같은 방식으로 다룰 수 있습니다.
	* Executor 인스턴스를 구성하고 생성자에 인수로 전달하고 있습니다.
* `SimpleAsyncTaskExecutor`
	* 전송한 Job마다 새로 Thread를 제공합니다.
	* Thread를 풀링하거나 재사용하지 않습니다.
	* 전송한 각각의 Job은 Thread에서 비동기로 실행됩니다.
* `SyncTaskExecutor`
	* 가장 단순한 `TaskExecutor`구현체로써 동기적으로 Thread를 띄워 Job을 실행하고 `join()` 메서드로 연결합니다.
	* 사실상 Threading은 완전히 건너띄고 호출 Thread에서 `run()`메서드를 수동 실행한 것이나 다를바 없습니다.
* `ScheduledExecutorFactoryBean`
	* `ScheduledExecutorTask` 빈으로 정의된 Job을 자동 트리거합니다.
	* `ScheduledExecutorTask` 인스턴스 목록을 지정해서 여러 Job을 동시에 실행할 수도 있고, 작업 실행 간 공백 시간을 인수로 넣을 수도 있습니다.
* `ThreadPoolTaskExecutor`
	* java.util.concurrent.ThreadPoolExecutor를 기반으로 모든 기능이 완비된 Thread Pool 구현체입니다.

---
# 참고사항

## TreadFactory
Tread를 할당하고 실행할 때 지정한 동작을 할 수 있도록 만들어 주는 Interface입니다.
현재 제가 이해한 바로는 Log를 작성할 때 찾기 쉽게 넘버링을 해준다던가, 네이밍을 해주는 용도로 사용하는 것으로 파악 됩니다.
물론 사전에 필요한 동작들을 설정해 줄 수 있겠지만, 특별한 사례가 생각 나지는 않습니다.

그럼 TreadFactory를 사용한 방법의 예시를 한번 볼까요??

```java title:"ThreadFactorySample.java"
public class ThreadFactorySample implements ThreadFactory {  
    private final String threadPrefix;  
    private long threadCounter;  
    private StringBuilder builder;  
  
    public ThreadFactorySample(String threadName) {  
        this.threadPrefix = threadName;  
        this.threadCounter = 0;  
        this.builder = new StringBuilder();  
    }  
  
    @Override  
    public Thread newThread(Runnable r) {  
        Thread thread = new Thread(r, threadPrefix + "-" + threadCounter++);  
        builder.append(String.format("Create Thread %d with name on %s, Active thread %s %n", thread.getId(), thread.getName(), new Date(), Thread.activeCount()));  
        return thread;  
    }  
  
    public String getThreadLog () {  
        String s = builder.toString();  
        builder.setLength(0);  
        return s;  
    }  
}
```

우리가 사용하려는 Thread의 이름이 재 할당 된 것을 확인 할 수 있는데요.
또한, 해당 Thread의 로그 정보를 가져올 수 있게 builder에 `append()`하여 해당 문구를 가져 올 수 있습니다.

아래에서 한번 실행해 보겠습니다.

```java title:"ExecutorsDemo.java"
public class ExecutorsDemo {  
    public static void main(String[] args) throws Throwable{  
        Runnable task = new DemonstrationRunnable();  
	    ThreadFactorySample tfs = new ThreadFactorySample("테스트");
  
        ExecutorService cachedThreadPoolExecutorService = Executors.newCachedThreadPool(tfs);  
  
        if(cachedThreadPoolExecutorService.submit(task).get() == null) {  
            System.out.printf("The cachedThreadPoolExecutorService has succeeded at %s \n", new Date());  
            System.out.println(tfs.getThreadLog());
        }  
  
        ExecutorService fixedThreadPool = Executors.newFixedThreadPool(100);  
        if(fixedThreadPool.submit(task).get() == null) {  
            System.out.printf("fixedThreadPool has succeeded at %s \n", new Date());  
        }  
  
        ExecutorService singleThreadExecutorService = Executors.newSingleThreadExecutor();  
        if(singleThreadExecutorService.submit(task).get() == null) {  
            System.out.printf("singleThreadExecutorService has succeeded at %s \n", new Date());  
        }  
  
        ExecutorService es = Executors.newCachedThreadPool();  
        if(es.submit(task, Boolean.TRUE).get().equals(Boolean.TRUE)) {  
            System.out.println("Job has finished");  
        }  
  
		// 10초 후에 해당 Thread를 동작시키는 .schedule() 함수  
		ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(10);  
		if(scheduledExecutorService.schedule(task, 10, TimeUnit.SECONDS).get() == null) {  
		    System.out.printf("scheduledExecutorService has succeeded at %s \n", new Date());  
		}  
		  
		// 5초마다 구현된 Runnable을 동작시키는 .scheduleAtFixedRate() 함수  
		scheduledExecutorService.scheduleAtFixedRate(task, 0, 5, TimeUnit.SECONDS);
	}
}
```

아래와 같이 첫번째 Thread의 이름이 나왔다면 성공입니다.
해당 Thread에서 builder를 통해 얻은 String에 대한 Log도 정상적으로 나온 모습을 확인 할 수 있습니다.

```java
테스트중-0
Hello at Mon Jun 03 17:02:37 KST 2024 
The cachedThreadPoolExecutorService has succeeded at Mon Jun 03 17:02:37 KST 2024 
Create Thread 24 with name on ThreadFactory테스트중-0, Active thread Mon Jun 03 17:02:34 KST 2024 

pool-1-thread-1
Hello at Mon Jun 03 17:02:40 KST 2024 
fixedThreadPool has succeeded at Mon Jun 03 17:02:40 KST 2024 
pool-2-thread-1
Hello at Mon Jun 03 17:02:43 KST 2024 
singleThreadExecutorService has succeeded at Mon Jun 03 17:02:43 KST 2024 
pool-3-thread-1
Hello at Mon Jun 03 17:02:46 KST 2024 
Job has finished
pool-4-thread-1
Hello at Mon Jun 03 17:02:59 KST 2024 
scheduledExecutorService has succeeded at Mon Jun 03 17:02:59 KST 2024 
pool-4-thread-1
Hello at Mon Jun 03 17:03:02 KST 2024 
pool-4-thread-2
Hello at Mon Jun 03 17:03:07 KST 2024 
```