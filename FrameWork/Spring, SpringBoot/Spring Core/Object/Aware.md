---
tags:
  - Spring
  - Core
  - Object
---
## Aware
Bean이 IoC Container와 직접적인 의존관계를 가지도록 설계하는 방법은 바람직하지 않지만, 때때로 필요할 경우가 있습니다.
때문에 해당 방법을 사용하기 위해서는 `Aware(인지)` Interface를 구현해합니다.

다음 표는 Spring에서 자주 쓰는 `Aware Interface` 입니다.

| Aware Interface                 | Target Resource Type                                                           |
| ------------------------------- | ------------------------------------------------------------------------------ |
| BeanNameAware                   | IoC Container에 구성한 Instance Bean Name                                          |
| BeanFactoryAware                | 현재 BeanFactory, ContainerService를 호출하는데 사용                                     |
| ApplicationContextAware         | 현재 ApplicationContext, ContainerService를 호출하는데 사용                              |
| MessageSourceAware              | MessageSource, TextMessage를 해석하는데 사용                                           |
| ApplicationEvent PublisherAware | ApplicationEvent Publisher, ApplicationEvent를 발행하는데 사용                         |
| ResourceLoaderAware             | ResourceLoader, 외부 Resource를 로드하는데 사용                                          |
| EnvironmentAware                | ApplicationContext Interface의org.springframework.core.env.Environment Instance |

`ApplicationContext`는 `MessageSource, ApplicationEventPublisher`, `ResourceLoader`를 모두 상속한 인터페이스라서 ApplicationContext만 인지하면 나머지 서비스도 액세스할 수 있습니다.

**But!!!**
요건을 충족하는 최소한의 범위 내에서 `Aware Interface`를 선택하는게 바람직합니다.

## Aware Interface의 동작 순서
`Aware Interface`의 Setter 메서드는 Spring이 Bean Property를 설정한 이후, 초기화 콜백 메서드를 호출하기 이전에 호출합니다.

1. 생성자나 Factory 메서드를 호출해 Bean Instance를 생성
2. Bean Property에 value, BeanReference를 설정
3. Aware Interface에 정의한 Setter 메서드 호출
4. Bean Instance를 각 Bean 후처리기에 있는 `postProcessBeforeInitialization()` 메서드로 넘겨 초기화 콜백 메서드 호출
5. Bean Instance를 각 Bean 후처리기에 있는 `postProcessAfterInitialization()` 메서드로 넘김. Bean을 사용할 준비가 완료.
6. Container가 종료되면 폐기 콜백 메서드를 호출

이렇게 `Aware Interface`를 구현한 Class는 Spring과 엮이게 되어 IoC Container외부에서는 제대로 동작하지 않는 점을 유의해야 합니다.
따라서, 구현시 정말 구현이 필요한지를 잘 따져서 구현해야 합니다.

## 예시 코드
아래의 코드는 Cashier Class의 POJO 인스턴스가 자신의 Bean 이름을 인지하도록 하는 코드의 예시입니다. 
해당 Class의 Bean의 이름을 알고 싶을 때 사용할 수 있습니다.
```java title:"Cashier.java"
@Component
public class Cashier implements BeanNameAware {
	...
	private String fileName;
	
	@Override
	public void setBeanName(String beanName) {
		this.fileName = beanName;	
	}

	public void printName() {
		System.out.println("Bean Name is " + fileName);
	}
}
```