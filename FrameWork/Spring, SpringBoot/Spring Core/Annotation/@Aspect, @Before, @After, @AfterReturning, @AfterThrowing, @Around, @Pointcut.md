---
tags:
  - Spring
  - Core
  - Annotation
  - AOP
Reference link: "[[FrameWork/Spring, SpringBoot/Spring Core/README|README]]"
cf: "[[AspectJ의 표현식]]"
---
## Spring 2.x의 AOP Proxy
과거 Spring 2.x 에서는 AOP Proxy를 구현할 때 JDK Dynamic Proxy를 사용했습니다.
JDK Dynamic Proxy는 Interface를 구현한 객체에 대해서만 AOP Proxy를 생성할 수 있었죠.
하지만 시간이 지나 Interface를 구현하지 않은 객체에도 AOP Proxy를 생성할 수 있는 CGLIB가 포함되면서  `Interface-구현체`관계가 아닌 Class에 대해서도 AOP Proxy가 구현이 가능해졌습니다.

## @Aspect
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

한가지 중요한 사실은 Spring에서 `@Aspect`를 Component Scan대상으로 인식하게 할 수 있다는 것입니다.
한가지 의문점은 '인식하게 할 수 있다??'라는 것은 기본적으로는 인식하지 않는다 일까?
라는 의문점 입니다. Spring에서도 `@Aspect`를 `@Component`와 같이 사용하기를 권장하고 있습니다. 만약 '@Component'를 사용하지 않을 것 이라면 다음과 같이 Configuration을 설정 할 때 `@ComponentScan`을 사용하여 대상에 포함시키는 방법이 있습니다.

```java
@ComponentScan(value = "aspect", includeFilters = @ComponentScan.Filter(value = Aspect.class))
```

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

## @Before 어드바이스
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

## JoinPoint
조인트컷으로 매치한 실행 지점을 joinPoint라고 합니다. 어드바이스가 현재 조인포인트 세부적인 내용에 액세스하려고 할 때 사용할 수 있습니다.
어드바이스를 선언한 메서드에 변수에 `JoinPoint`를 추가하여 사용합니다.
그러면 메서드명, 인수값 등 자세한 조인트포인트 정보를 조회할 수 있습니다.

받아올 수 있는 정보가 다양하지만 몇가지만 소개해 보겠습니다.
* getSignature().getName() : 조인포인트 실행 메서드명 (ex.add(), sub(), ...)
* getSignature().getDeclaringTypeName() : 메서드가 선언된 인터페이스의 이름
* getTarget().getClass().getName() : 메서드가 구현된 Class의 이름
* getArgs() : 실행한 메서드에서의 parameter 값들
* getTarget() : 대상 각체
* getThis() : 프록시 객체
* ...


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

## @After 어드바이스
조인포인트가 끝나면 실행되는 메서드로 `@After`를 붙여 표시합니다.
도중에 예외가 발생해도 상관없이 동작하게 되어 있습니다.
아래 코드는 `com.example.demo`의 하위 class에서 모두 동작하도록 만든 것입니다.
> Spring에서는 * *.*(..)으로 모든 class설정이 가능하나 해당 표현식으로 SpringBoot에서는 final로 선언된 class가 포함되어 있어 동작하지 않는 오류가 발생할 수 있습니다.


```java title:"CalculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	...

	@After("execution(* com.example.demo..*(..))")  
	public void logAfter(JoinPoint joinPoint) {  
	    log.info("The method " + joinPoint.getSignature().getName() + "() ends");  
	}
}
```

```
4.0 - 3.0 = 1.0
2024-05-28 11:01:00,388 [           main] INFO  spject.CalculatorLoggingAspect - The method sub() ends
2.0 * 3.0 = 6.0
2024-05-28 11:01:00,388 [           main] INFO  spject.CalculatorLoggingAspect - The method mul() ends
1.0 + 2.0 = 3.0
2024-05-28 11:01:00,388 [           main] INFO  spject.CalculatorLoggingAspect - The method add() ends
4.0 / 2.0 = 2.0
2024-05-28 11:01:00,388 [           main] INFO  spject.CalculatorLoggingAspect - The method div() ends
10.0 kilogram = 22.0 pound
2024-05-28 11:01:00,389 [           main] INFO  spject.CalculatorLoggingAspect - The method kilogramToPound() ends
5.0 kilometer = 3.1 mile
2024-05-28 11:01:00,389 [           main] INFO  spject.CalculatorLoggingAspect - The method kilometToMile() ends
```

## @AfterReturning 어드바이스
위에서 언급했듯이 `@After`어드바이스는 메서드의 실행의 성공여부와 상관없이 동작하게 되어 있습니다.
하지만, 우리는 조인포인트가 값을 반환하는 경우에만 로깅을 하고 싶을 수가 있겠죠??
그럴경우 사용하는것이 `@AfterReturning` 어드바이스 입니다.

이 때, 조인포인트가 반환한 결과값을 가져오려면 returning 속성으로 지정한 변수명을 어드바이스 메서드의 인수로 지정해야 합니다.
이때, 조인트컷 표현식은 pointcut 속성에 작성해야 합니다.


```java title:"CalculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	...

	@AfterReturning("execution(* com.example.demo..*(..))")  
	public void logAfterReturning(JoinPoint joinPoint, Obejct result) {  
	    log.info("The method {}()", joinPoint.getSignature().getName()); 
	}
}
```

```java title:"CalculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	...

	@AfterReturning(
		pointcut = "execution(* com.example.demo..*(..))",
		returning = "result")  
	public void logAfterReturning(JoinPoint joinPoint, Obejct result) {  
	    log.info("The method {}() ends with {}", joinPoint.getSignature().getName(), result);  
	}
}
```

## AfterThrowing 어드바이스
위에서 언급한 `@AfterReturning`은 메서드가 성공했을 경우라고 말씀드렸죠??
그러면 우리는 자연스럽게 예외가 날 경우는?? 이라고 생각하게 됩니다.(해야합니다...)
이 때, 사용하는 것이 `@AfterThrowing`입니다. 

동작의 원리는 `@AfterReturning`과 같습니다.
다른점 이라면 발생한 예외를 throwing 속성에 담아 전달한다는 것입니다.

```java title:"CalculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	...

	@AfterThrowing("execution(* com.example.demo..*(..))")  
	public void logAfterReturning(JoinPoint joinPoint, Obejct result) {  
	    log.error("An Exception has been thrown in {}()", joinPoint.getSignature().getName()); 
	}
}
```

```java title:"CalculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	...

	@AfterThrowing(
		pointcut = "execution(* com.example.demo..*(..))",
		throwing = "e")  
	public void logAfterReturning(JoinPoint joinPoint, Throwable e) {  
	    log.error("An Exception {} has been thrown in {}()", e, joinPoint.getSignature().getName()); 
	}
}
```

만약 특정 예외만 관심이 있다면 인수에 해당 타입을 선언하면 됩니다.

```java title:"CalculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	...

	@AfterThrowing(
		pointcut = "execution(* com.example.demo..*(..))",
		throwing = "e")  
	public void logAfterReturning(JoinPoint joinPoint, IllegalArgumentException e) {  
	    log.error("An Exception {} has been thrown in {}()", e, joinPoint.getSignature().getName()); 
	}
}
```

## @Around 어드바이스
`@Around`는 어드바이스 중 가장 강력한 어드바이스입니다. 조인포인트를 완전히 장악하기 때문에 앞서 살펴본 어드바이스 모두 `@Around`로 조합할 수 있습니다.
더 나아가 원본 조인트를 실행할 시점, 실행 자체 여부, 계속 실행할지 여부까지도 제어할 수 있습니다.

`@Around`는 매우 강력한 녀석이라 원본 인숫값을 바꾸거나 최종 반환값을 변경하는 일도 가능합니다. 하지만 간혹 원본 조인트를 진행하는 호출을 잊어버리기 쉬우므로 사용시에 주의가 반드시 필요합니다.

```java title:"CalculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	private Logger log = LoggerFactory.getLogger(this.getClass());

	@Around("execution(* com.example.demo..*(..))")
	public Object logAround(ProceedingJoinPoint joinPoint) throws Throable {

		log.info("The method {}() begins with {}", joinPoint.getSignature().getName(), Arrays.toString(joinPoint.getArgs()));

		try {
			Object result = joinPoint.proceed();
		    log.info("The method {}() ends with {}", joinPoint.getSignature().getName(), result);  
		    return result;
		} catch (IllegalArgumentException e) {
			log.error("Illegal argument {} in {}()", Arrays.toString(joinPoint.getArgs()), joinPoint.getSignature().getName());
			throw e;
		}
	}
}
```

### 어드바이스 사용시 유의사항 & TIP
어드바이스의 경우 최소한의 요건을 충족하면서도 가장 기능이 약한 어드바이스를 사용하는 것이 바람직합니다. 이점을 꼭 유의하시기 바랍니다.

## @Order
Aspect 간 우선순위를 정하기위해서는 두가지 방법이 있습니다.
첫 번째, `@Order` Annotation을 사용하여 지정
두 번째, Ordered Interface를 사용하여 지정

테스트를 위해 검증용 Aspect를 하나 더 생성해보겠습니다.

```java title:"CalculatorVaildationAspect.java"
@Aspect
@Component
public class CalculatorVaildationAspect {

	@Before("exxcution(* com.example.demo..*(..))")
	public void validateBefore(JoinPoint joinPoint) {
		for (Object arg : joinPoint.getArgs()){
			validate((Double) arg);
		}
	}

	private void vaelidate(double a) {
		if (a < 0 ) {
			throw new IllegalArgumentException("Positive numbers only");
		}
	}
}
```

우선, `Ordered` Interface를 통해 우선순위를 지정하는 방법부터 알아보겠습니다.
`Ordered` Interface를 implements하게 되면 getOrder() 이라는 메서드를 구현하게 됩니다. 이 메서드를 통해 우선순위를 정하는 것이죠.
숫자가 낮을수록 우선수위가 높다는 뜻이 됩니다.

그럼 검증을 먼저 고치고 로그를 남기도록 한번 작성해보도록 하죠.

```java
@Aspect
@Component
public class CalculatorVaildationAspect implements Ordered {
	...

	@Override
	public int getOrder() {
		return 0;
	}
}

@Aspect
@Component
public class CalculatorLoggingAspect implements Ordered {
	...

	@Override
	public int getOrder() {
		return 1;
	}
}
```

이번엔 `@Order`를 사용해 우선순위를 좀 더 깔끔하게 구현해 봅시다.

```java
@Aspect
@Component
@Order(0)
public class CalculatorVaildationAspect {
	...
}

@Aspect
@Component
@Order(1)
public class CalculatorLoggingAspect {
	...
}
```

## @Pointcut
Pointcut 표현식을 여러번 사용해야 할 경우가 있을 수 있습니다.
그럴경우 `@Pointcut`을 이용하면 포인트컷만 따로 정의해 여러 어드바이스에서 재사용 할 수 있습니다.
해당 Annotation을 사용하면 가시성에 있어서 코드의 반복이 발생하지 않아 좋습니다.

```java title:"CaculatorLoggingAspect.java"
@Aspect
@Component
public class CalculatorLoggingAspect {

	private Logger log = LoggerFactory.getLogger(this.getClass());

	@Pointcut("execution(* com.example..*(..))")
	private void loggingOperation() {}

	@Before("loggingOperation()")
	public void logBefore(JoinPoint joinPoint) {
		...
	}

	@AfterReturning(
		pointcut = "loggingOperation()"
		returning = "result")
	public void logAfterReturning(JoinPoint joinPoint, Object result) {
		...
	}

	@AfterThroing(
		pointcut = "loggingOperation()"
		throwing = "e")
	public void logAfterThrowing(JoinPoint joinPoint, IllegalArgumentException e) {
		...
	}

	@Around("loggingOperation()")
	public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
		...
	}
}
```

만약, 여러 `Aspect`가 포인트컷을 공유하는 경우라면 공통 클래스 한 곳에 포인트컷을 모아두는 편이 사용기 좋습니다. 물론!! 당연히 포인트컷 메서드는 public으로 선언해야겠죠??

```java title:"PointcutFactory.java"
@Aspect
public class PointcutFactory {

	@Pointcut("execution(*. LogGenerator.*(..))")
	public void loggingOperation() {}
}
```
**
```java title:"CalculatorLoggingAspect.java"
@Aspect
public class CalculatorLoggingAspect {
	...

	@Before("PointcutFactory.loggingOperation()")
	public void logBefore(JointPoint joinPoint) {
		...
	}

	@AfterReturning(
		pointcut = "PointcutFactory.loggingOperation()"
		returning = "result")
	public void logAfterReturning(JoinPoint joinPoint, Obejct result) {
		...
	}

	@AfterThrowing(
		pointcut = "PointcutFactory.loggingOperation()"
		throwing = "e")
	public void logAfterThrowing(JoinPoint joinPoing, IllegalArgumentException e) {
		...
	}

	@Around("PointcutFactory.loggingOperation()")
	public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
		...
	}
}
```
