---
tags:
  - Spring
  - Core
  - Annotation
Reference link: "[[BeanPostProcessor]]"
---
`@Required`는 Spring에 내장된 `RequiredAnnotationBeanPostProcessor`가 지원하는 Annotation입니다.
`RequiredAnnotationBeanPostProcessor` 후처리기는 `@Required`를 붙인 모든 Bean Property가 설정되었는지 확인합니다.
하지만, 해당 proeprty값의 설정 여부만 체크할 뿐 그 값이 null인지 아니면 다른 값인지는 신경쓰지 않습니다. 정말 확인 용도로만 사용하는 것이죠.

아래 코드로 예시를 들어보겠습니다.
prefixGenerator와 suffix는 시퀀스 생성기의 필수 property입니다. 따라서 해당 property의 생성여부를 판단하기 위해 setter 메서드에 `@Required`를 붙입니다.

Spring이 해당 property를 감지해서 값의 존재 여부를 조사하고 property값이 없으면 `BeanInitializationException`예외를 던지게 될것입니다.

```java title:"SequenceGenerator"
public class SequenceGenerator {

	private PrefixGenerator prefixGenerator;
	private String suffix;

	@Required
	public void setPrefixGenerator(PrefixGenerator prefixGenerator) {
		this.prefixGenerator = prefixGenerator;
	}

	@Required
	public void setSuffix(String suffix) {
		this.suffix = suffix;
	}

	...
}
```
