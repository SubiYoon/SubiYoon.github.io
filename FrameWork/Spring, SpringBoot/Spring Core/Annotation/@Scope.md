---
tags:
  - Spring
  - Core
  - Annotation
---
### @Scope
`@Scope`는 Bean 스코프를 지정하는 Annotation입니다.
모든 Bean의 기본 스코프는 `sigleton`으로 설정 되어 있습니다.
이를 변경하려고 할 때 아래의 스코프중 하나를 선택해서 변경할 수 있습니다.

| 스코프           | 설명                                                            |
| ------------- | ------------------------------------------------------------- |
| singleton     | IoC 컨테이너당 빈 인스턴스 하나를 생성                                       |
| prototype     | 요청할 때마다 빈 인스턴스를 새로 생성                                         |
| request       | HTTP 요청당 빈 인스턴스 한개 생성. Web Application Context에만 해당           |
| session       | HTTP 세션당 빈 인스턴스 한개 생성. Web Application Context에만 해당           |
| globalSession | 전역 HTTP 세션당 빈 인스턴스 한개 생성. <br>Potal Application Context에만 해당. |

예시로 두 고객이 상품을 등록하는 예시를 들어보겠습니다.
우선 장바구니 Class입니다.
```java title:"ShoppingCart.java"
@Component
public class ShoppingCart {
	private LIst<Product> imtes = new ArrayList<>();

	public void addItem(Product item) {
		items.add(item);
	}

	public List<Project> getItems() {
		return items;
	}
}
```

다음은 카트에 상품을 추가 할 수 있게 구성파일을 만들어 보겠습니다.
```java title:"ShopConfiguration"
@Configuration
@ComponentScan("com.xxx.xxx.shop")
public class ShopConfiguration {

	@Bean
	public Product aaa() {
		Battrey p1 = new Battery();
		p1.setName("AAA");
		p1.setPrice(2.5);
		p1.setRechargeable(true);
		return p1;
	}
	
	@Bean
	public Product cdrw() {
		Disc p2 = new Disc("CD-RW", 1.5);
		p2.setCapacity(700);
		return p2;
	}

	@Bean
	public Product dvdrw() {
		Disc p3 = new Disc("DVD-RW", 3.0);
		p3.setCapacity(700);
		return p3;
	}
}
```

이제 두 고객이 장바구니에 상품을 넣어 보겠습니다.
```java title:"Main.java"
public class Main {  
    public static void main(String[] args) {  
        ApplicationContext context = new AnnotationConfigApplicationContext(ShopConfiguration.class);
        
        Product aaa = context.getBean("aaa", Product.class);  
        Product cdrw = context.getBean("cdrw", Product.class);  
        Product dvdrw = context.getBean("dvdrw", Product.class);  
        
        ShoppingCart cart1 = context.getBean("shoppingCart", ShoppingCart.class);  
        cart1.addItem(aaa);  
        cart1.addItem(cdrw);  
        System.out.println("Shopping cart 1 contains " + cart1.getItems());  
  
        ShoppingCart cart2 = context.getBean("shoppingCart", ShoppingCart.class);  
        cart2.addItem(dvdrw);  
        System.out.println("Shopping cart 2 contains " + cart2.getItems());  
    }  
}
```
```bash
Shoping cart 1 contains [AAA 2.5, CD-RW 1.5]
Shoping cart 2 contains [AAA 2.5, CD-RW 1.5, DVD-RW 3.0]
```

위의 결과는 Spring의 기본 스코프는 `singleton`이라서 IoC 컨테이너당 장바구니 인스턴스가 한개만 생성되어 나타난 결과입니다.

그러면 우리는 서로 다른 두 고객의 장바구니를 어떻게 해야 나눌 수 있을까요?
맞습니다. 장바구니를 가져올 때마다 서로 상이한 인스턴스를 가져 오면 되겠죠??
```java title:"ShoppingCart.java"
@Component
@Scope("prototype")
public class ShoppingCart {
	private LIst<Product> imtes = new ArrayList<>();

	public void addItem(Product item) {
		items.add(item);
	}

	public List<Project> getItems() {
		return items;
	}
}
```

위와 같이 생성한다면 아래와 같은 결과를 얻을 수 있습니다.
```bash
Shoping cart 1 contains [AAA 2.5, CD-RW 1.5]
Shoping cart 2 contains [DVD-RW 3.0]
```