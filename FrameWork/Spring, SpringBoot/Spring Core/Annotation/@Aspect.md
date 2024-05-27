---
tags:
  - Spring
  - Core
  - Annotation
  - AOP
Reference link: "[[FrameWork/Spring, SpringBoot/Spring Core/README|README]]"
---
### Spring 2.x의 AOP Proxy
과거 Spring 2.x 에서는 AOP Proxy를 구현할 때 JDK Dynamic Proxy를 사용했습니다.
JDK Dynamic Proxy는 Interface를 구현한 객체에 대해서만 AOP Proxy를 생성할 수 있었죠.
하지만 시간이 지나 Interface를 구현하지 않은 객체에도 AOP Proxy를 생성할 수 있는 CGLIB가 포함되면서  `Interface-구현체`관계가 아닌 Class에 대해서도 AOP Proxy가 구현이 가능해졌습니다.

### @Aspect
Aspect를 정의하려면 일단 자바 Class에 `@Aspect` Annotation을 붙이고 메서드별로 적절한 Anntation을 붙여 Advice로 만들어야 합니다.
Advice Annotation의 종류는 다음과 같습니다.
* `@Before`
* `@After`
* `@AfterReturning`
* `@AfterThrowing`
* `@Around`

Spring에서 사용하는 `@Aspect`는 AspectJ와 동일한 AOP를 구현합니다.
차이점 이라면 AOP의 런타임 자체는 순수 Spring의 AOP이기 때문에 AspectJ 컴파일러와 위버(Weaver)는 무관합니다.

각 Annotation별로 어떻게 작용하는지 잠깐 참고해 보세요.

![image](https://github.com/SubiYoon/SubiYoon.github.io/blob/main/Attached%20File/Pasted%20image%2020240522171850.png?raw=true)

IoC Container에서 Aspect Annoation 기능을 활성화하려면 구성 Class 중 하나`@EnableAspectJAutoProxy`를 붙입니다.
만약 Interface를 사용할 수 없거나 애플리케이션 설계상 사용하지 않을 경우엔 proxyTargetClass 속성을 ture로 설정하면 동적 Proxy 대신 CGLIB를 사용하게 합니다.

```java title:"ArithmeticCalculator.java"
package com.apress.springrecipes.caculator;

public interface ArithmeticCalculator {
	public double add(double a, double b);
	public double sub(double a, double b);
	public double mul(double a, double b);
	public double div(double a, double b);
}
```

```java title:"UnitCaculator.java
package com.apress.springrecipes.caculator;

public interface UnitCalculator {
	public double kilogramToPound(double kilogram);
	public double kilometToMile(double kilometer);
}
```

```java title:"ArithmeticCalculatorImpl.java"

@Component("arithmeticCalculator")
public class ArithmeeticCalculatorImpl implements ArithmeticCalculator {
	@Override
	public double add(double a, double b) {
		double result = a + b;
		System.out.println(a + " + " + b + " = " + result);
		return result;
	}
	
	@Override
	public double sub(double a, double b) {
		double result = a - b;
		System.out.println(a + " - " + b + " = " + result);
		return result;
	}
	
	@Override
	public double mul(double a, double b) {
		double result = a * b;
		System.out.println(a + " * " + b + " = " + result);
		return result;
	}
	
	@Override
	public double div(double a, double b) {
		if(b == 0) {
			throw new IllegalArgumentException("Division by zero");
		}

		double result = a / b;
		System.out.println(a + " / " + b + " = " + result);
		return result;
	}
}
```

```java title:"UnitCaculatorImpl.java"
@Component("unitCalculator")
public class UnitCaculatorImpl implements UnitCalculator {

	@Override
	public double kilogramToPound(double kilogram) {
		double pound = kilogram * 2.2;
		System.out.println(kilogram + " kilogram = " + pound + " pound");
		return pound;
	}

	@Override
	public double kilometToMile(double kilometer) {
		double mile = kilometer * 0.62;
		System.out.println(kilometer + " kilometer = " + mile + " mile");
		return mile;
	}
}
```

### @Before 어드바이스
`@Before`어드바이스는 특정 프로그램 실행 지점 이전 처리하는 메서드입니다.
ArithmeticCalculator Interface의 add() 메서드 실행을 가르키고 와일드카드(*)는 모든 수정자(public, protected, private), 모든 반환형을 매치함을 의미합니다. 인수목록에 작성되어 있는 두 점(..)은 인수 개수는 몇개라도 좋다는 것을 의미합니다.

```java title:"CalculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	private Log log = LogFactory.getLog(this.getClass());

	@Before("execution(* ArithmeticCalculator.add(..))")
	public void logBefore() {
		log.info("The method add() begins");
	}
}
```

logback.xml을 작성해봅시다.

```xml title:"logback.xml"
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">  
        <layout class="ch.qos.logback.classic.PatternLayout">  
            <Pattern>%d [%15.15t] %-5p %30.30c - %m%n</Pattern>  
        </layout>  
    </appender>  
  
    <root level="INFO">  
        <appender-ref ref="STDOUT"/>  
    </root>  
</configuration>
```

이제 자바 구성 클래스에 `@EnableAspectAutoProxy`를 붙여 Aspejct를 스프링이 스캐닝하게 합니다.

```java title:"CalculatorConfiguration.java"
@Configuration
@EnableAspectJAutoProxy
@ComponentScan
public class CalculatorConfiguration{
}
```

이렇게 작성한 코드를 잘 동작하는지 테스트를 해봅시다.
Main Class를 만들어서 사용해도 되지만, 저는 Spring에 있는 Test Class를 사용해 보겠습니다.

```java title:"DemoApplicationTests" 
@SpringBootTest  
class DemoApplicationTests {  
  
    @Test  
    void contextLoads() {  
        ApplicationContext context = new AnnotationConfigApplicationContext(CaculatorConfiguration.class);  
  
        ArithmeticCalculator arithmeticCalculator = context.getBean("arithmeticCalculator", ArithmeticCalculator.class);  
        arithmeticCalculator.sub(4,3);  
        arithmeticCalculator.mul(2,3);  
        arithmeticCalculator.add(1,2);  
        arithmeticCalculator.div(4,2);  
  
        UnitCalculator unitCaculator = context.getBean("unitCalculator", UnitCalculator.class);  
        unitCaculator.kilogramToPound(10);  
        unitCaculator.kilometToMile(5);  
    }  
}
```

결과가 아래와 같이 나오는 것을 확인 할 수 있습니다.
add() 메서드에만 해당 문구가 나오도록 모듈화 해주었기 때문에 add() 메서드 실행전에 해당 로그가 찍히는 것을 확인 할 수 있습니다.

```
4.0 - 3.0 = 1.0
2.0 * 3.0 = 6.0
2024-05-27 17:04:17,133 [           main] INFO  spject.CalculatorLoggingAspect - The method add() begins
1.0 + 2.0 = 3.0
4.0 / 2.0 = 2.0
10.0 kilogram = 22.0 pound
5.0 kilometer = 3.1 mile
```

### JoinPoint
조인트컷으로 매치한 실행 지점을 joinPoint라고 합니다. 어드바이스가 현재 조인포인트 세부적인 내용에 액세스하려고 할 때 사용할 수 있습니다.
어드바이스를 선언한 메서드에 변수에 `JoinPoinst`를 추가하여 사용합니다.
그러면 메서드명, 인수값 등 자세한 조인트포인트 정보를 조회할 수 있습니다.

```java title:"CalculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	...

@Before("execution(* ArithmeticCalculator.*(..))")  
public void logBefore(JoinPoint joinPoint) {  
    log.info("The method add() begins");  
    log.info("The method " + joinPoint.getSignature().getName() + "() begins with " + Arrays.toString(joinPoint.getArgs()));  
}
	}
}
```

테스트 코드를 재실행 시켜보면 아래와 같이 결과가 나옵니다.
이번엔 함수에 `*`를 사용하여 ArithmeticCalculator Interface의 모든 함수에 어드바이스가 발생하도록 설정했습니다.
`Arrays.toString(joinPoint.getArgs())`를 통해 해당 함수에 사용된 parameter값을 파악 할 수 있습니다.

```
2024-05-27 18:15:39,368 [           main] INFO  spject.CalculatorLoggingAspect - The method add() begins
2024-05-27 18:15:39,369 [           main] INFO  spject.CalculatorLoggingAspect - The method sub() begins with [4.0, 3.0]
4.0 - 3.0 = 1.0
2024-05-27 18:15:39,370 [           main] INFO  spject.CalculatorLoggingAspect - The method add() begins
2024-05-27 18:15:39,370 [           main] INFO  spject.CalculatorLoggingAspect - The method mul() begins with [2.0, 3.0]
2.0 * 3.0 = 6.0
2024-05-27 18:15:39,370 [           main] INFO  spject.CalculatorLoggingAspect - The method add() begins
2024-05-27 18:15:39,370 [           main] INFO  spject.CalculatorLoggingAspect - The method add() begins with [1.0, 2.0]
1.0 + 2.0 = 3.0
2024-05-27 18:15:39,371 [           main] INFO  spject.CalculatorLoggingAspect - The method add() begins
2024-05-27 18:15:39,371 [           main] INFO  spject.CalculatorLoggingAspect - The method div() begins with [4.0, 2.0]
4.0 / 2.0 = 2.0
10.0 kilogram = 22.0 pound
5.0 kilometer = 3.1 mile
```