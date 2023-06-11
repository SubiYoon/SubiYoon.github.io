# Thread / Runnable / Thread Pool

# Thread

- 어떤 프로그램 내에서 실행되는 흐름의 단위
- 특히 프로세스 내에서 실행되는 흐름의 단위

# Runnable

- 쓰레드의 run( ) 메서드만 가지고 있는 인터페이스
- 다중 상속이 되지 않는 문제를 해결하기 위해 생긴 인터페이스
- 사용시 Runnable객체를 생성후 해당 객체를 Thread로 생성하여 사용한다.

```java
Runnable runnable1 = new MyServer();
Runnable runnable2 = new MyServer();

Thread thread1 = new Thread(runnable1);
Thread thread2 = new Thread(runnable2);

thread1.start();
thread2.start();
```

# Thread Pool

- 쓰레드를 미리 원하는 갯수 만큼 생성해 두는 것
- ExecutorService 인터페이스의 구현객체를 정적 팩토리 메서드로 제공하는 Executors클래스를 이용하여 구현이 가능하다.

# ThreadPoolExecutor

### corePoolSize( )

- 생성할 개수를 설정하는 메서드

### maximumPoolSize( )

- 생성할 최대 개수를 설정하는 메서드
- corePoolSize( )로 생성한 쓰레드 보다 많은 쓰레드가 필요시 설정한 개수 만큼 생성할 수 있다.

### keepAliveTime( )

- 유지 시간을 설정하는 메서드
- 현재 쓰레드 풀이 corePoolSize보다 많은 쓰레드를 가지고 있다면 초과한 쓰레드에 대해서 설정한 시간보다 오랫동안 할 일이 없으면 제거된다.

### TimeUnit.”???”

- keppAliveTime( ) 의 시간적 단위를 결정

### BlockingQueue<Runnable> workQueue( )

- 요청된 작업들이 저장될 큐 : 작업 대기열 관리
- corePoolSize보다 작업 쓰레드가 많아졌을 경우, 남는 쓰레드가 없을 경우, WorkQueue에서 대기한다.

### execute( )

- 쓰레드 풀에 작업을 요청한다.
- 예외 발생시 쓰레드가 종료되고 쓰레드풀에서 제거한 뒤 새로운 쓰레드를 생성한다.
- 처리결과를 반환하지 않는다.

### submit( )

- 쓰레드 풀에 작업을 요청한다.
- 예외가 발생하더라도 쓰레드가 종료되지 않고 다음 작업에 사용된다.
- 처리결과를 **Future<?>**로 반환한다.
    - **Future<?>**
        
        → 비동기적인 연산의 결과를 표현하는 클래스