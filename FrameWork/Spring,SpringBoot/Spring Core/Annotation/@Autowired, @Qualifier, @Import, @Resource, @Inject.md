---
tags:
  - Spring
  - Core
  - Annotation
---
### @Autowired
`@Autowired`는 Bean Factory에 저장되어 있는 인스턴스를 전부 검색한 후에 자동으로 연결시켜주는 기능을 합니다. 위에 `@Component`를 설명하면서 작성한 Class를 우리는 `@Component`를 선언해 줌으로써 Bean 생성을 하였습니다. 그렇게 생성된 인스턴스를 연결시켜준다고 생각하면 됩니다. 맞는 비유일지는 모르겠으나, 바인딩이 된다고 생각하면 좀 더 이해가 쉬울 것입니다.
위에서 우리가 선언한 `SequenceDaoImpl`Class를 예로 들어 보겠습니다.

```java title:"@Autowired 예시"
@Component
public class SequenceService {

	@Autowired
	private SequenceDao sequenceDao;

	...
}
```

`@Autowired`의 속성값으로 `required`를 붙이면 Spring이 Bean을 찾지 못 하더라도 그냥 지나치게 만들 수 있습니다.
```java title:"Autowired required 예시"
@Component
public class SequenceService {

	@Autowired(required = false)
	private SequenceDao sequenceDao;

	...
}
```

`@Autowired`는 생성자 및 메서드에도 선언할 수도 있는데, 매개변수에 선언되어 있는 객체에 자동으로 Bean을 연결시켜 주는 역할을 하게 됩니다.

```java
public class SequenceGenerator {

	...

	@Autowired
	public void setPrefixGenerator(prefixGenerator prefixGenerator) {
		this.prefixGenerator = prefixGenerator;
	}

	...
}
```


```java
@Component
public class SequenceService {

	private final SequenceDao sequenceDao;

	@Autowired
	public SequenceService(SequenceDao sequenceDao){
		this.sequenceDao = sequenceDao
	}
	
}
```

### @Qualifier
Spring에서 Bean에 등록된 id(name)으로 직접적으로 명시해 `@Autowired`를 시킬 수 있습니다. 해당 Annotation을 사용하면, 작성한 id의 Bean을 검색해 해당 Bean 인스턴스를 연결시킵니다.

```java title:"@Qualifier 예시"
public class SequenceGenerator {

	@Autowired
	@Qualifier("datePrefixGenerator")
	private PrefixGenerator prefixGenerator;
	
	...
}
```

`@Qualifier`는 메서드 인수를 연결할 때도 사용할 수 있습니다. 다음 예시를 보겠습니다.

```java
public class SequenceGenerator {

	...

	@Autowired
	public void myOwnCustomInjectionName(@Qualifier("datePrefixGenerator") PrefixGenerator prefixGenerator){
		this.prefixGenerator = prefixGenerator;
	}
}
```

### @Import
Application의 규모가 커질수록 POJO 설정을 하나의 자바 구성 Class에 담기 어려워집니다.
이를 해결하기 위해서 자바 구성 클래스가 위치한 경로마다 컨텍스트를 초기화하여 사용하는 방법이 있습니다.

```java
ApplicationContext context = new AnnotationConfigApplicationContext(SequenceGeneratorConfiguration.class, PrefixConfiguration.class);

PrefixGenerator prefixgenerator = context.getBean("datePrefixGenerator", DateprefixGenerator.class);
```

다른 방법으로는 `@Inport`를 사용하는 것입니다.
sequenceGenerator Bean에서는 반드시 prefixGenerator Bean을 설정해야 하는데 이 Class에는 없고 대신 다른 자바 구성 Class PrefixConfiguration에 정의되어 있을 경우 해당 구성 Class의 Scope를 `@Value`를 통해 가져 올 수 있습니다.
```java title:"@Import 예시"
@Configuration
@Import(PrefixConfiguration.class)
public class SequenceConfiguration {

	@Value("#{dataPrefixGenerator}")
	private PrefixGenerator prefixGenerator;

	@Bean
	public SequenceGenerator sequenceGenerator() {
		SequenceGenerator sequence = new SequenceGenerator();
		sequence.setInitail(10000);
		sequence.setSuffix("A");
		sequence.setPrefixGenerator(prefixGenerator);
		return sequence;
	}
}
```

### @Resource, @Inject
Spring전용 `@Autowired`가 아닌 Java 표준 Annotation인 `@Resource`와 `@Inject`를 통해 자동 연결을 시킬 수 있습니다.
* `@Autowired` : org.springframework.benas.factory.annotation 스프링에서만 사용가능
* `@Resource` : `@Autowired` + `@Qualifier`를 합친 기능
* `@Inject` : `@Autowired`, `@Qualifier` 와 같은 기능

`@Resource`의 경우 name속성을 이용하여 어떤 Bean을 연결시킬 것인지 명시할 수 있습니다. 아래 예시를 참고해 보도록 하겠습니다. 기본적으로 속성없이 선언하면 `@Autowired`와 똑같이 동작하지만 name속성을 부여하여 datePrefixGenerator Bean을 연결하였습니다.

```java title:"@Resource 예시"
public class SequenceConfiguration {

	// prefixGenerator를 연결
	@Reousrce
	private PrefixGenerator prefixGenerator;

	// 또는

	// datePrefixGenerator를 연결
	@Reousrce(name="datePrefixGenerator")
	private PrefixGenerator prefixGenerator;
}
```

`@Inject`도 같은 방식으로 동작하지만 타입이 같은 POJO가 여럿일 때엔 다른 방법을 구사해야 합니다. 아래의 예시는 일반적인 방법으로 사용하는 `@Inject` Annotation입니다.

```java title:"@Inject 예시"
public class SequenceConfiguration {

	@Inject
	private PrefixGenerator prefixGenerator;
}
```

그럼 이제 타입이 같은 POJO가 여럿일 때 사용하는 방법을 알아보겠습니다.
우선 POJO 주입 클래스와 주입 지점을 구별하기 위해 커스텀 Annotation을 작성해야합니다. 이때 사용하는 Annotation인 `@Qualifier`은 Spring전용이 아닌 `javax.inject`에 속한 Annotation입니다.
* Target : 해당 Annotation이 사용되는 위치를 결정
	* ElementType.PACKAGE : 패키지 선언시
	- ElementType.TYPE : 타입 선언시
	- ElementType.CONSTRUCTOR : 생성자 선언시
	- ElementType.FIELD : 맴버 변수 선언시
	- ElementType.METHOD : 메소드 선언시
	- ElementType.ANNOTATION_TYPE : 어노테이션 타입 선언시
	- ElementType.LOCAL_VARIABLE : 지역 변수 선언시
	- ElementType.TYPE_PARAMETER : 매개 변수 타입 선언시
- Documented : JavaDoc 생성 시 Document에 포함되도록 설정
- Retention : 해당 Annotation의 정보를 어느 범위까지 유지할 것인지 설정
	- RetentionPolicy.SOURCE: 컴파일 전까지만 유효하며 컴파일 이후에는 소멸
	- RetentionPolicy.CLASS: 컴파일러가 클래스를 참조할 때까지 유효
	- RetentionPolicy.RUNTIME: Reflection에 의해 컴파일 이후 JVM에 의해 참조가 가능
- Inherited : 해당 Annoation을 하위 클래스에 적용
- Repeatable : Java8부터 지원, 연속적으로 Annotation을 선언하는 것을 허용
- `@Qualifier`, `@RestController`, ... : 적용할 Annotation을 선언


```java title:"@Inject를 사용하기 위한 Custom Annotation 예시"

@Qualifier
@Target({ElementType.Type, ElementType.FIELD, ElementType.PARAMETER})
@Documented
@Retention(RetentionPolicy.RUNTIME)
public @interface DatePrefixAnnotation {
	String value() default "Custom Annotation!!";
}

// 주입
@DataPrefixAnnotion
public class DataPrefixGenerator implements PrefixGenerator {
	...
}

// Annotation 변수 사용
DatePrefixAnnotation annotation = DataPrefixGenerator.class.getAnnotation(datePreFixAnnotation.class);

System.out.print(annotation.value()); // Custom Annotation!!
```

`@Autowired`, `@Resource`, `@Inject`는 타입을 기준으로하면 아무거나 사용해도 무관하고, 이름을 기준으로 하면 아무래도 `@Resource`가 구문이 가장 단순합니다.