---
tags:
  - Spring
  - Core
  - Annotation
---
`@Profile`은 우리가 각 환경에 맞게 Application Context를 설정하게 할 수 있습니다.
예를 들자면, 우리는 개발환경, 테스트환경, 운영환경에 따른 서버의 정보가 다를 수 있습니다.

이 경우, 우리는 환경에 따라 매번 소스코드를 수정하고 배포하고를 반복하기가 귀찮죠.
이럴 때 우리는 `@Profile` Annotation을 사용하게 됩니다.

예를 들어보겠습니다.
아래의 경우에는 `@Profile("global")`인 경우 해당 Class를 컴파일하는 방법입니다.
이 경우 다양한 방법으로 컴파일 시킬 수 있습니다.

### SpringBoot 환경 변수
Spring Boot를 사용하는 경우 

1. `application-local.properties`
2. `application-global.properties`
3. `application-prod.properties`

위 같은 파일명으로 .properties파일을 생성하면 `-`뒤의 문자를 자동으로 탐색합니다.

### Spring의 applicationContext.xml으로 class 지정
Spring (applicationContext.xml)을 사용한느 경우 아래와 같이 해당 Bean의 이름과 class위치를 통해 특정해 줄 수 있습니다.

```xml
<beans profile="global"> <bean id="datasourceConfig" class="oing.daddy.DatasourceConfig" /> </beans>
```

### @Profile(${name})을 지정
```java title:"ShopConfigurationGlobal.java"
@Configuration
@Profile("global")
@ComponentScan("com.apress.springrecipes.shop")
public calss ShopConfigurationGlobal {

	@Bean(initMethod = "openFile", destroyMethod = "closeFile")
	public Cashier cashier() {
		final String path = System.getProperty("java.io.tmpdis") + "cashier";

		Cashier c1 = new Cashier();

		c1.setFileName("checkout");
		c1.setPath(path);

		return c1;
	}
}
```

### @Profile(${name}) 사용시 2가지 환경 이상의 경우 지정
```java title:"ShopConfigurationSumWin.java"
@Configuration
@Profile({"summer", "winter"})
public calss ShopConfigurationSumWin {

	@Bean
	public Product aaa() {
		Battery p1 = new Battery();

		p1.setName("AAA");
		p1.setPrice(2.0);
		p1.setRechargeable(true);

		return p1;
	}

	@Bean
	public 	Product cdrw() {
		Disc p2 = new Disc("CD-RW", 1.0);

		p2.setCapacity(700);

		return p2;
	}

	@Bean
	public 	Product dvdrw() {
		Disc p3 = new Disc("DVD-RW", 2.5);

		p3.setCapacity(700);

		return p3;
	}
}
```

# Profile을 환경에 load
Profile 여러 개를 한 번에 오드하는 것도 가능하지만, Runtime flag나 WAR 파일 초기화 매개변수를 지정해 프로그램 방식으로 Profile을 로드할 수 있습니다.

### java 프로그램으로 컨트롤하는 방법
```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
context.getEnviroment().setActiveProfiles("global", "winter");
/* 기본 프로파일로 지정하는 방법
context.getEnviroment().setDefaultProfiles();
*/
context.scan("com.apress.springrecipes.shop");
context.refresh();
```

### java Runtime flag로 컨트롤하는 방법
``` bash
-Dspring.profiles.active=global,winter

# 기본파일로 지정하는 방법
-Dspring.profiles.default
```

### OS 환경변수에 직접 작성해주는 방법
``` bash
vi $HOME/.bashrc export SPRING_PROFILES_ACTIVE=global,winter $ $HOME/.bashrc
```

### maven cli로 package 사용하는 방법
```
mvn clean package -Pglobal,winter
```