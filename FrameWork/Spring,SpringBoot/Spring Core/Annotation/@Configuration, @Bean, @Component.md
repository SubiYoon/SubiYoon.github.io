---
tags:
  - Spring
  - Core
  - Annotation
Reference link: "[[@PostConstrcut, @PreDestory]]"
---
### @Configuration, @Bean
@Configuration을 선언하면 해당 Class는 구성 Class임을 알리게 된느데요.
Spring은 @Configuration이 달린 구성 Class에서 @Bean을 붙인 자바 메서드를 찾게 되는데, 해당 메서드와 동일한 이름의 Bean이 생성됩니다.
이름을 따로 명시하려면 `@Bean(name="${name}")`을 붙이는 방법도 존재하니 특정 이름으로 생성하고 싶을 때에는 이 방법을 사용하면 됩니다.

```java title:"@Configuration 예시"
@Configuration
public class SequenceGeneratorConfiguration {
	@Bean
	public SequenceGenerator sequenceGenerator() {
		SequenceGenerator seqgen = new SequenceGenerator();
		
		...

		return seqgen;
	}

	@Bean(name="seqGen")
	public SequenceGenerator sequenceGenerator() {
		SequenceGenerator seqgen2 = new SequenceGenerator();
		
		...

		return seqgen2;
	}
}
```

또한 `@Bean(initMethod="${methodName}" destoryMethod="${methodName}"`을 통해 인스턴스를 생성하기 전에 실행할 함수와 빈을 폐기할 때 실행할 메서드를 설정할 수 있습니다.

```java title:"Cashier.java"
public Class Cashier {

	private String fileName;
	private String path;
	private BufferedWriter writer;

	public void setFileName(String fileName) {
		this.fileName = fileName;
	}

	public void setPath(String path) {
		this.path = path;
	}

	public void openFile() throws IOException {
		File targetDir = new File(path);

		if(!targetDir.exists()) {
			targetDir.mkdir();
		}

		File checkoutFile = new File(path, fileName + ".txt");

		if(!checkoutFile.exists()) {
			checkoutFile.createNewFile();
		}

		writer = new BufferedWriter(new OutputStreamWiter(new FileOutputStream(checkoutFile, true)));
	}

	public void checkout(ShoppingCart cart) throws IOEception {
		writer.write(new Date() + "\t" + cart.getItems() + "\r\n");
		writer.flush();
	}

	public void closeFile() throws IOEception {
		writer.close();
	}
}
```

위에 `Cashier.java`파일을 예시로 아래를 살펴보겠습니다.
`@Bean`으로 인해 Cashier인스턴스가 생성되기 이전에 `initMethod`로 인해 openFile() 메서드를 실행시키면서 대상 디렉터리와 파일이 있는지 확인한 후 해당 경로에 있는 파일을 열어 writer 필드에 할당합니다. 이 후 checkout() 메서드를 호출할 때마다 이 텍스트 파일에 내용을 덧붙입니다. 기록을 마치면 `destoryMethod`로 인해 closeFile() 메서드를 실행해 파일을 닫고 시스템 리소스를 반납하게 됩니다.

```java title:"ShopConfiguration.java"
@Configuration
public class ShopConfiguration {

	@Bean(initMethod = "openFile", dstroyMethod = "closeFile")
	public Cashier cashier() {
		String path = System.getProperty("java.io.tmpdir") + "/cashier";
		Cashier c1 = new Cashier();
		c1.setFileName("checkout");
		c1.setPath(path);
		return c1;
	}
}
```

ApplicationContext 객체를 통해서 생성한 Bean을 가져올 수 있는데요.
ApplicationContext는 `@Bean`이 붙어있는 메서드의 Class정보를 담고 있다고 생각하면 쉽습니다. 다음 예시로 한번 알아가 보겠습니다.

```java
ApplicationContext context = new AnnotationConfigApplicationContext(SequenceGeneratorConfiguration.class);
```

위의 예시는 `ApplicationContext`객체인 `context`에 `SequenceGeneratorConfiguration`Class 정보를 가져온 것입니다. 이제 우리는 `SequenceGeneratorConfiguration` Class에서 `@Bean`으로 명시된 녀석들을 가져오는 방법을 알아 보겠습니다.
* getBean()의 반환형은 Object입니다. 따라서 캐스팅을 해주어 사용하는 방법입니다.
```java
SequenceGeneratorConfiguration generator = (SequenceGenerator) context.getBean("sequenceGenerator");
```
* 두번째 parameter에 Class명을 지정해주는 방법입니다.
```java
SequenceGeneratorConfiguration generator = context.getBean("sequenceGenerator", SequenceGenerator.class);
```

### @Component
`@Bean` Annotation을 사용하지 않고, 각 Class에 선언하여 Bean을 생성하는 방법입니다.
즉, 이 방법은 `@Configuration`을 선언하여 구성 Class가 아닌 녀석들의 Bean을 생성하는 방법입니다. `@Repository`, `@Service`, `@Controller`  등 POJO의 쓰임새가 명확하지 않을 때  사용하는 방법입니다.
`@Component("sequenceDao")`에서 "sequenceDao"는 인스턴스 ID로 설정되며, 값이 없으면 소문자로 시작하는 비규격 Class명을 빈 이름으로 기본할당합니다. 예를 들어보겠습니다.
* `@Component("sequenceDao")`의 경우 Bean 이름 : sequenceDao
* `@Component`의 경우 Bean 이름 : sequenceDaoImpl

```java title:"@Component 예시"
@Component("sequenceDao")
public class SequenceDaoImpl implements SequenceDao {
	...
}
```

Spring은 `@Configuration`, `@Bean`, `@Repository`, `@Service`, `@Controller`가 달린 클래스를 모두 감지하게 됩니다. 이때 우리는 하나 이상의 포함/제외 필터를 적용해 스캐닝 과정을 커스터마이징 할 수 있습니다. 종류는 다음과 같습니다.
* Annotation : 애너테이션
* Assignable : Class / Interface
* Regex : 정규식
* AspectJ : 포인트컷 표현식
* Custom : 직접 필터식을 구현하여 사용 할 경우
* 
```java title:"@ComponentScan 예시"
@ComponentScan(
	inCludeFilters = {
		@ComponentScan.Filter(
			type = FilterType.REGEX,
			pattern = {"com.apress.springrecipes.sequence.*Dao",
					   "com.apress.springrecipes.sequence.*Service"})
	},
	exCludeFilters = {
		@ComponentScan.Filter(
			type = FilterType.ANNOTATION,
			pattern = {"org.springframework.stereotype.Controller.class"})
	}
)
```

### @Bean과 @Component의 차이
'`@Component`는 템플릿을 정의하는 것이고, `@Bean`은 Bean 인스턴스를 정의하는 것이다.'
라고 나와있습니다. 무슨 말인지 이해하셨나요?
저는 전혀 이해가 되지 않았습니다. 그래서 GPT에게도 물어보았지만 이해가 되진 않더라구요. 제가 확실히 이해한건지는 모르겠으나, 사수분께 여쭤본바라는 대강 이렇게 설명 할 수 있겠습니다.
`@Component`는 Class 자체를 `@ComponentScan`에 의해 Bean을 생성해 `@Bean`과 달리 메뉴얼하게 작성하지 않는다는 것입니다.
이것도 어렵죠? 최대한 쉽게 설명해 보겠습니다. 아래의 예시는 `@Bean`입니다. 아래처럼 설정할 경우 `new`를 통해 객체를 생성하고 해당 객체에 대한 설정을 하나하나 `set`할 수 있습니다.

```java
@Configuration
public class SequenceGeneratorConfiguration {
	@Bean
	public SequenceGenerator sequenceGenerator() {
		SequenceGenerator seqgen = new SequenceGenerator();
		
		...

		return seqgen;
	}

	@Bean(name="seqGen")
	public SequenceGenerator sequenceGenerator() {
		SequenceGenerator seqgen2 = new SequenceGenerator();
		seqgen2.setPrefix("class_");
		seqgen2.setmiddlefix(" :: ");
		
		...

		return seqgen2;
	}
}
```

하지만, `@Component`의 경우 Class에 선언해 Class 자체를 Bean으로 생성하기 때문에 해당 전체의 템플릿(`Class`)를 Bean으로 생성한다는 것입니다.

```java
public class SequenceGenerator {
	String prefix = "";
	String middlefix = "";

	public SequenceGenerator(String prefix, String middelfix){
		this.prefix = prefix;
		this.middlefix = middfix;
	}
}
```

Bean으로 등록된다는 것에 차이는 없으나,  방식의 차이와 이를 극복하기 위해 `@Scope`를 사용한다는 차이를 이해해야 합니다. `@Scope`의 경우 아래에서 설명하니 해당 부분을 참고해 주세요.
