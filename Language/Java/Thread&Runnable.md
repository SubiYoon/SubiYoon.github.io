# Thread / Runnable

# Thread

- 쓰레드를 사용하려는 클래스에 Thread를 상속받아 사용한다.
- 각 쓰레드는 각각의 Stack을 가지고 있다.
- 각각의 Stack을 가지고 있기 때문에 개별로 로직처리가 가능해 동시에 작업이 가능하다.

### run( )

- 쓰레드를 상속받으면 반드시 재정의해 주어야 하는 메서드
- 쓰레드 작업을 할 로직을 작성해준다.

```java
public class MyThread extends Thread {
	@Override
	public void run(){
		...
	}
}
```

### start( )

- Thread를 상속받은 클래스의 run( ) 메서드를 실행 시켜주는 메서드

```java
public class TestThread {
	MyThread myThread = new MyThread();

	myThread.start();
}
```

# Runnable

- 쓰레드를 상속받게 되면 다른 클래스를 상속받을 수 없는 문제를 해결하기 위해 만들어진 run( ) 메서드만 가지고 있는 인터페이스
- Thread와 마찬가지로 run 메서드를 오버라이드(재정의)하여 사용해야 한다.

```java
public class MyThread implements Runnable {
	@Override
	public void run(){
		...
	}
}
```

### execute( )

- 쓰레드를 실행 시키는 메서드
- 예외 발생시 예외를 나타내주고 쓰레드를 종료시킨다.

```java
public class TestThread {
	Thread myThread = new Thread(new MyThread());

	myThread.execute();
}
```

### submit( )

- 쓰레드를 실행 시키는 메서드
- 예외 발생시 예외를 Future<?> 클래스에 저장하고 나타내주고 쓰레드를 종료시키지 않고 계속 남겨둔다.

```java
public class TestThread {
	Thread myThread = new Thread(new MyThread());

	myThread.submit();
}
```