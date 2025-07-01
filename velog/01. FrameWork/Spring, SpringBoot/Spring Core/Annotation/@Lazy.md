---
tags:
  - Spring
  - Core
  - Annotation
---
## @Lazy
기본적으로 Spring은 모든 POJO를 급하게 초기화 합니다.
쉽게 말하자면 Application이 시동과 동시에 POJO를 초기화하는 것이지요.
하지만, 환경에 따라 우리는 처음 요청하기 전까지 해당 과정을 미뤄야 하는 경우도 발생합니다. 그럴 경우 사용하는 개념이 느긋한 초기화인 `@Lazy`입니다.

```java title:"ShoppingCart.java"
@Component
@Scope("prototype")
@Lazy
public class ShoppingCart {
	private List<Product> items = new ArrayList<>();

	public void addItem(Product item) {
		items.add(item);
	}

	public List<Product> getItems(){
		return items;
	}
}
```