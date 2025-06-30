---
tags:
  - Spring
  - Core
  - Annotation
---
## @Primary
Spring에서 `@Primary`를 붙여 자동연결시 우선순위를 부여해줄 수 있습니다.
만약 PrefixGenerator 인터페이스 구현체인 DataPrefixGenerator 클래스에 `@Primary`를 붙여놓았으면 PrefixGenerator 형의 인스턴스가 다수 존재하더라도 `@Primary`를 붙인 Class의 Bean이 우선적으로 연결됩니다.

```java title:"@Primary 예시"
@Componet
@Primary
public class DataPrefixGenerator implements PrefixGenerator {

	...
}
```