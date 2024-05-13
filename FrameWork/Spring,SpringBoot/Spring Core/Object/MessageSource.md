---
tags:
  - Spring
  - Core
  - Object
---
### MessageSource
`ResourceBundleMEssageSource`는 가장 많이 쓰이는 `MeessageSource` 구현체로 다국어를 사용 할 때 사용합니다.

```properties title:"message_en_US.properties"
alert.checkout=A shopping cart has been checked out.
alert.inventory.checkout=A shopping cart with {0} has been checked out at {1}
```

Bean 인스턴스는 반드시 messageSource라고 명명해야 컨텍스트가 알아서 감지합니다.
`setBasenames()`로 message로 시작하는 파일들을 찾도록 설정하고 캐시주기는 1초로 설정합니다.
각 Locale별로 리소스 번들 파일을 제일 먼저 찾아가고, 파일이 없는 경우 `message_en.properties`파일을 찾게됩니다. 만약 이 파일마저 없다면 기본파일인  `message.properties`파일을 선택합니다.
```java title:"ShopConfiguration.java"
@Configuration
public class ShopConfiguration {

	@Bean
	public ReloadableReosurceBundleMessageSource messageSource() {
		ReloadalbeResourceBundelMessageSource messageSource = new ReloadalbeResourceBundelMessageSource();
		messageSource.setBasenames("classpath:message");
		messageSource.setCacheSeconds(1);
		return messageSource;
	}
}
```

`getMessage()` 메서드로 메시지를 해석 할 수 있습니다.
* 첫번째 인자 : `key`를 지정
* 두번째 인자 : `{0~}`에 선언된 parameter에 삽입할 데이터를 생성하여 삽입
* 세번째 인자 : Locale지정
```java title:"Main.java"
public class Main {
	public static void main(String[] args) throws Exception {

		ApplicationContext context = new AnnotationConfigApplicationContext(ShopConfiguration.class);

		String alert = context.getMessage("alert.checkout", null, Locale.US);
		String alert.inventory =  context.getMessage("alert.inventory.checkout", new Object[] {"[DVD-RW 3.0]", new Date()}, Locale.US);

		System.out.println("The I18N message for alert.checkout is : " + alert);
		System.out.println("The I18N message for alert.inventory.checkout is : " + alert_inventory);
	}
}
```

Main 클래스는 `ApplicationContext`를 직접 가져 올 수 있지만, 텍스트 메시지를 해석하는 Bean에는 `MessageSource`구현체를 넣어야 합니다.
```java
@Component
public class Cashier {

	@Autowired
	private MesaageSource messageSource;

	public void setMessageSource(MessageSource messageSource) {
		this.messageSource = messageSource;
	}

	public void checkout(ShoppingCart cart) throws IOException {
		String alert =  context.getMessage("alert.inventory.checkout", cart.getItems(), new Date()}, Locale.US);
		System.out.println(alert);
	}
}
```