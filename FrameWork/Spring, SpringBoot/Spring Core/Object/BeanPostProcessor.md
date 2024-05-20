---
tags:
  - Spring
  - Core
  - Object
Reference link: "[[@Required]]"
---
빈 후처리기는 `BeanPostProcessor` 인터페이스를 구현한 객체입니다.
`BeanPostProcessor`를 구현한 Bean을 발견하면 Spring은 휘하의 Bean Instance 들에게 `postprocessBeforeInitailization()`, `postProcessAfterInitailization()` 두 함수를 적용하게 되는데 빈 상태를 조사, 수정, 확인한느 등 어떤 로직도 이 메서드에 넣을 수 있습니다.

두 메서드는 하는 동작이 없더라도 반드시 원본의 Bean Instance를 반환해야 합니다.

아래 예시를 보겠습니다.

`@Component`를 선언하여 Bean Instance를 생성하게 하였고, `BeanPostProcessor` 인터페이스를 구현하는 예시입니다.

```java title:"AuditCheckBeanPostProcessor"
@Component
public class AuditCheckBeanPostProcessor implements BeanPostProcessor {

	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		System.out.println("In AuditCheckBeanPostProcessor.postProcessBeforeInitialization, processing bean type: " + bean.getClass());
		return bean;
	}

	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		return bean;
	}
}
```

다양한 Class에 `BeanPostProcessor`를 상속받았을 경우 우리는 특정 Bean Instance만 처리하도록 후처리기를 생성할 수 있습니다.

java의 `intanceof`를 활용해 봅시다.

```java title:"AuditCheckBeanPostProcessor"
@Component
public class AuditCheckBeanPostProcessor implements BeanPostProcessor {

	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		if (bean instanceof Product) {
			String productName = ((Product) bean).getName();
			System.out.println("In AuditCheckBeanPostProcessor.postProcessBeforeInitialization, processing Product: " + prductName;
		}
		return bean;
	}

	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		if (bean instanceof Product) {
			String productName = ((Product) bean).getName();
			System.out.println("In AuditCheckBeanPostProcessor.postProcessAfterInitialization, processing Product: " + prductName;
		}
		return bean;
	}
}
```

또한, 두 메서드는 처리할 Bean Instance를 반드시 반환해야 합니다.
이를 다시 생각해본다면, 원본 인스턴스가 아닌 다른 인스턴스로 바꿔치기 할 수도 있다는 뜻이 됩니다.

```java title:"AuditCheckBeanPostProcessor"
@Component
public class AuditCheckBeanPostProcessor implements BeanPostProcessor {

	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		if (bean instanceof Product) {
			return new Product();
		}
		return bean;
	}

	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		if (bean instanceof Product) {
			return new Product();
		}
		return bean;
	}
}
```