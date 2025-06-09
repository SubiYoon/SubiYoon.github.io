---
tags:
  - Spring
  - Core
  - Object
---
Factory를 통해 POJO를 생성한는 방법은 여러가지가 있습니다.

1. 정적 메서드를 사용
2. 인스턴스 메서드를 사용
3. 스프링 Factory Bean 사용

이 중 Spring은 FactoryBean 인터페이스를 상속한 간편한 템플릿 클래스 `AbstractFactoryBean`을 제공합니다.

우선 정적 메서드를 사용하여 POJO를 생성하는 예시부터 보겠습니다.

## 정적 메서드로 POJO 생성하기

```java title:"ProductCreator.java"
public class ProductCreator {

	public static Product createProduct(String productId) {
		if("aaa".equals(productId)) {
			return enw Battery("AAA", "2.5");
		} else if("cdrw".equals(productId)) {
			return enw Battery("CD-RW", "1.5");
		} else if("dvdrw".equals(productId)) {
			return enw Battery("DVD-RW", "3.0");
		}
	
		throw new IllegalArgumentException("Unkown product!!");
	}
}
```

```java title:"ShopConfiguration.java"
@Configuration
public class ShopConfiguration {

	@Bean
	public Product aaa() {
		return ProductCreator.createProduct("aaa");
	}


	@Bean
	public Product cdrw() {
		return ProductCreator.createProduct("cdrw");
	}

	@Bean
	public Product dvdrw() {
		return ProductCreator.createProduct("dvdrw");
	}
}
```

## 인스턴스 메서드로 POJO 생성하기

```java title:"ProductCreator.java"
public class ProductCreator {

	private Map<String, Product> products;

	public void setProducts(Map<String, Product> products) {
		this.products = products;
	}

	public Product createProduct(String productId) {
		Product product = products.get(productId);
		if(product != null){
			return product;
		}
			
		throw new IllegalArgumentException("Unkown product!!");
	}
}
```

ProductCreator에 상품을 생성하려면 `@Configuration`에 `@Bean`을 선언하여 해당 함수를 Instance화 하고 해당 함수를 호출하여 ProductCreator를 생성 후 Map에 해당 상품을 생성하는 방식으로 POJO를 생성할 수 있습니다. 

아래는 `@Configuration` Class를 생성하고 상품을 생헝하는 코드 예시입니다.

```java title:"ShopConfiguration.java"
@Configuration
public class ShopConfiguration {

	@Bean
	public ProductCreator productCreatorFactory() {
		ProductCreator factory = new ProductCreator();
		Map<String, Product> products = new HashMap<>();

		products.put("aaa", new Battery("AAA", 2.5));
		products.put("cdrw", new Disc("CD-RW", 1.5));
		products.put("dvdrw", new Disc("DVD-RW", 3.0));

		factory.setProducts(products);

		return factory;
	}

	@Bean
	public Product aaa() {
		return productCreatorFactory().createProduct("aaa");
	}

	@Bean
	public Product cdrw() {
		return productCreatorFactory().createProduct("cdrw");
	}

	@Bean
	public Product dvdrw() {
		return productCreatorFactory().createProduct("dvdrw");
	}
}
```

## Spring Factory Bean으로 POJO 생성하기
자!!
이제 우리가 궁극적으로 해보려고 하는 생성 방법입니다.
사실상 우리가 직접 Factory Bean을 구현할 일은 별로 없겠지만, 내부 작동 원리를 이해하는 건 여러모로 유익하다고하니... 한번 따라해 보겠습니다.

`AbstractFactoryBean<T>`의 메서드 종류를 보겠습니다.
* `getObjectType()` : 반환할 대상의 Bean을 선택해주는 메서드
* `createInstance()` : 생성할 대상의 인스턴스를 재정의하고 싶을 경우 사용하는 메서드

```java title:"DiscountFactoryBean.java"
public class DiscountFactoryBean extends AbstractFactoryBean<Product> {

	private Product product;
	private doule discount;

	public void setProduct(Product product) {
		this.product = product;
	}

	public void setDiscount(double discount) {
		this.discount = discount;
	}

	@Override
	public Class<?> getObjectType() {
		return product.getClass();
	}

	@Override
	protected Product createInstance() throw Exception {
		product.setPrice(product.getPrice() * (1 - discount));

		return product;
	}
}
```

마지막으로 상품 인스턴스를 생성하는 Factory Bean에 `@Bean`을 붙여 DiscountFactoryBean을 적용해보도록 하겠습니다.

```java title:"ShopConfiguration.java"
@Configuration
@ComponentScan("com.apress.springrecipes.shop")
public class ShopConfiguration {

	@Bean
	public Battery aaa() {
		Battery aaa = new Battery("AAA", 2.5);

		return aaa;
	}
	
	@Bean
	public Disc cdrw() {
		Disc cdrw = new Disc("CD-RW", 1.5);

		return cdrw;
	}
	
	@Bean
	public Disc dvdrw() {
		Disc dvdrw = new Disc("DVD-RW", 3.0);

		return dvdrw;
	}

	@Bean
	public DiscountFactoryBean discountFactoryBeanAAA() {
		DiscountFactoryBean factory = new DiscountFactoryBean();

		factory.setProduct(aaa());
		factory.setDiscount(0.2);

		return factory;
	}

	@Bean
	public DiscountFactoryBean discountFactoryBeanCDRW() {
		DiscountFactoryBean factory = new DiscountFactoryBean();

		factory.setProduct(cdrw());
		factory.setDiscount(0.1);

		return factory;
	}

	@Bean
	public DiscountFactoryBean discountFactoryBeanDVDRW() {
		DiscountFactoryBean factory = new DiscountFactoryBean();

		factory.setProduct(dvdrw());
		factory.setDiscount(0.1);

		return factory;
	}
}
```