---
tags:
  - Spring
  - Core
  - Object
  - Event
---
### ApplicationEvent
이벤트 기반의 통신을 하려면 제일 먼저 무엇이 이루져 있어야 할까요??
당연히 이벤트 자체를 정의하는 것 부터가 순서겠죠??
이렇게 이벤트를 정의할 때 `ApplicationEvent` Class를 사용하게 됩니다.

쇼핑 카트를 체크아웃하면 Casher Bean이 체크아웃 시각이 기록된 CheckoutEvent를 발행한다고 해봅시다.

```java title:"CheckoutEvent.java"
public class CheckoutEvent extends ApplicationEvent {

	private final ShoppingCart cart;
	private final Date time;

	public CheckoutEvent(ShoppingCart cart, Date time) {
		super(cart);
		this.cart = cart;
		this.time = time;
	}

	public ShoppingCart getCart() {
		return cart;
	}

	public Date getTime() {
		return time;
	}
}
```

이벤트를 정의했으면 다음 순서는 무엇일까요??? 이벤트를 발행해야겠죠???
해당 이벤트를 인스턴스화한 다음 Application Event 발행기에서 `publishEvent()` 메서드를 호출하면 이벤트가 발행됩니다.

```java title:"Cashier.java"
public clas Cashier implements ApplicationEventPublisherAware {
	...
	private ApplicationEventPublisher applicationEventPublisher;

	@Override
	public void setApplicationEventPublisher(ApplicationEventPublisher applicationEventPublisher) {
		this.applicationEventPublisher = applicationEventPublisher;
	}

	public void checkout(ShoppingCart cart) throws IOException {
		...
		CheckoutEvent event = new CheckoutEvent(this, new Date());
		applicationEventPublisher.publishEvent(event);
	}
}
```

다른 방법도 존재합니다.
필드 프로퍼티에 자동 연결해서 참조하는 방법인데요. 살펴보시죠.

```java title:"Cashier"
public class Cashier {
	...
	@Autowired
	private ApplicationEventPublisher applicationEventPublisher;

	public void checkout(ShoppingCart cart) throws IOException {
		...
		CheckoutEvent event = new CheckoutEvent(this, new Date());
		applicationEventPublisher.publishEvent(event);
	}
}
```

이렇게해서 이벤트를 발행하는 순서를 마쳤습니다.
이제 발신자(sender)의 작업을 마쳤으니 수신자(receiver)의 작업을 해주어야 겠죠???

이벤트를 리스닝하는 작업을 해주어야 합니다.

```java title:"CheckoutListener.java"
@Component
public class CheckoutListener implements ApplicationListener<CheckoutEvent> {

	@Override
	public void onApplicationEvent(CheckoutEvent event) {
		// 체크아웃 시각으로 할 일을 여기에 구현하면 됩니다.
		System.out.println("Checkout event [" + event.getTime() + "]");
	}
}
```

Spring 4.2 부터는 `ApplicationListener` Interface 없이 `@EventListener`를 붙여도 리스너로 만들 수 있습니다.

```java title:"CheckoutListener.java"
@Component
public class CheckoutListener {

	@EventListener
	public void onApplicationEvent(CheckoutEvent event) {
		// 체크아웃 시각으로 할 일을 여기에 구현하면 됩니다.
		System.out.println("Checkout event [" + event.getTime() + "]");
	}
}
```

### 마치며
발행하고자 하는 event를 다음 두가지 방법으로 발행합니다.
`ApplicationEventPublisherAware` Interface를 구현하는 방법이 있고, `ApplicationEventPublisher` Class를 통해 발행하는 방법이 있습니다.

그 후 해당 event를 받아 실질적으로 이벤트를 발생시키는 리스너를 구현하는 것입니다.
`ApplicationListener<T>` Interface를 구현하는 방법이 있고, 메서드를 생성하고 `@EventListener` Annotation을 선언하는 방법이 있습니다.
