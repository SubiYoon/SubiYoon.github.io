---
tags:
  - Spring
  - Core
  - Annotation
---
### @DependsOn
POJO가 늘어나면 Bean생성 과정 중 다른 Bean의 생성이 우선시 되어야 하는 경우가 발생할 수 있습니다.
예를 들어 A, B라는 Bean의 로직은 C라는 Bean에서 필요하게 될 때 발생합니다.
아직 Spring이 A, B를 초기화하지 않아 C라는 Bean이 먼저 초기화되면서 에러가 발생하게 될 것입니다.
이 때 Bean의 초기화 순서를 보장해주는 Annotation이 `@DependsOn`입니다.

`@DependsOn("${BeanName}")`으로 선언하고 해당 Class의 Bean은 반드시 `${BeanName}`의 빈보다 나중에 생성됩니다.
* `${BeanName}`은 콤마(,)를 기준으로 여러개를 선언할 수 있습니다.
	* `@DependsOn({"Bean1","Bean2","Bean3",...})` 

아래의 경우 sequenceGenerator Bean은 반드시 dataPrefixGenerator Bean 생성 이후 생성될 것입니다.
```java title:"SequenceConfiguration.java"
@Configuration
public class SequenceConfiguration {

	@Bean
	@DependsOn("dataPrefixGenerator")
	public SequenceGenerator sequenceGenerator() {
		SequenceGenerator sequence = new SequenceGenerator();
		sequence.setInitial(100000);
		sequence.setSuffix("A");
		return sequence;
	}

}
```
