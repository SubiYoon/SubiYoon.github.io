---
tags:
  - Spring
  - Core
  - Annotation
Reference link: "[[Resource]]"
---
### @PropertySource
Sping에서 제공하는 Annotaion으로 .properties 파일의 데이터를 `key=value`쌍을 읽어 올 수 있습니다.

```properties title:"discounts.propeties"
specialcustomer.dicount=0.1
summer.discount=0.15
endofyear.discount=0.2
```

아래의 예시를 보겠습니다.

기본적인 application.properties의 파일 내용을 가져오지 않고, 특정 파일 `discounts.properties` 파일 내용을 가져 오려면 `@PropertySource`에 선언해서 해당 파일을 가져 오면 됩니다. `@Value`를 사용함으로써 해당 파일에 명시된 `key=value=`를 불러옵니다.

```java title:"ShopConfiguration.java"
@Configuration
@PropertySource("classpath:discounts.properties")
@ComponentScan("com.apress.springrecipes.shop")
public class ShopConfiguration {

	@Value("${endofyear.discount:0}")
	private double specialEndofyearDiscountField;

	@Bean
	public static PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer() {
		return new PropertySourcesPlaceholderConfigurer();
	}

	@Bean
	public Product dvdrw() {
		Disc p2 = new Disc("DVD-RW", 3.0, specialEndofyearDiscountField);
		p2.setCapacity(700);
		return p2;
	}
}
```

