---
tags:
  - Spring
  - Core
  - AOP
Reference link: "[[@Aspect, @Before, @After, @AfterReturning, @AfterThrowing, @Around, @Pointcut]]"
---
### AspectJ Pointcut 표현식
Spring AOP가 지원하는 조인포인트 대상은 IoC Contatiner안에 선언된 Bean에 국한됩니다.
하지만 Spring AOP에서도 AspectJ Pointcut언어를 활용하여 pointcut을 정의하며 Runtime에 AspectJ 라이브러리를 이용하여 Pointcut 표현식을 해석하게 됩니다.

우리가 가장 기억해야 할 것은 Spring AOP에서는 IoC Container안에 선언된 Bean에만 조인 포인트를 지원한다는 것입니다.

다음 내용은 와드카드 수정자(public, protected, private)와 반환형, 인수 개수에 상관없이 매치하겠다는 뜻입니다.

```java
excution(* com.example.demo.calculator.ArithmeticCaculator.*(..))
```

만약 대상 Class나 Interface가 Aspect와 같은 패키지에 있다면 패키지명은 안써도 됩니다.

```java
execution(* ArithmeticCaculator.*(..))
```

Interface에 선언된 public 메서드만 매치하는 방법입니다.

```java
execution(public * ArithmeticCaculator.*(..))
```

이제 반환형도 한번 선택해봅시다.
public으로 선언된 메서드 중 반환형이 double인 메서드만 매치하는 방법입니다.

```java
execution(public double ArithmeticCaculator.*(..))
```

인수형과 개수를 정확히 매치시킬 수도 있습니다.

```java
execution(public double ArithmeticCaculator.*(double, double))
```

메서드 명도 선택할 수 있죠.

```java
execution(public double ArithmeticCaculator.add(double, double))
```

### AspectJ 표현식으로 매칭이 안될경우
위에 말씀드린 방법 외에 매치하고 싶은 메서드 사이에 공통된 특성이 없는 경우가 있을 수 있습니다. 예외 상황인거죠....
우리는 개발자로써 항상 예외상황을 고려해야 하잖아요??
이 부분을 고려 안하지 않았을 거란 말이죠??
이럴 때에는 메서드/타입 레벨에 다음과 같은 Custom Annotation을 만들 수 있습니다.

자! 그럼 한번 살펴봅시다.

우선 Custom Annotation을 생성합니다.

```java title:"LoggingRequired.java"
package com.apress.springrecipes.calculator;

@Target({ElementType.METHOD, EelementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface LoggingRequired {
}
```

이제 로깅이 필요한 메서드에 Custom Annotation을 붙이는 겁니다.
단, Annotation은 상속되지 않으므로 Interface가 아닌 구현 Class에 선언해야 합니다.

```java title:"ArithmeticCalculatorImpl.java"
@LoggingRequired
public class ArithmeticCalculatorImpl implements ArithmeticCalculator {

	public double add(double a, double b) {
		...
	}

	public double sub(double a, double b) {
		...
	}

	...
}
```

이제 모든 준비가 끝났습니다.

그럼 이제 우리는 무엇을 해야 할까요??

맞습니다!!

`@Aspect`가 선언된 Class에서 해당 Annotation을 스캔해 포함하도록 만들면 됩니다.
그럼 Annotation을 스캔하여 등록하는 방법을 알아보겠습니다.

``` java title:"CaclulatorPointcuts.java"
@Aspect
public class ClaculatorPointcuts {

	@Pointcut("annotation(com.example.demo.calculator.LoggingRequired)")
	public void loggingOperation {}
}
```

### 타입 시그니처 패턴
특정 타입 내부의 모든 조인포인트를 매치하는 Pointcut 표현식도 존재합니다.
Spring AOP에 적용하면 그 타입 안에 구현된 메서드를 실행할 때에만 어드바이스가 적용되도록 Pointcut 적용 번위를 줄일 수 있습.

```java
within(com.apress.springrecipes.calculator.*)
```

하위 패키지도 함께 매치하려면 와일드카드 앞에 점 하나를 더 추가합니다. 

```java
within(com.apress.springrecipes.calculator..*)
```

어느 한 클래스 내부에 구현된 메서드 실행 조인포인트를 매치할 경우

```java
within(com.apress.springrecipes.calculator.ArithmeticCalculatorImpl)

// 해당 Class의 패키지가 Aspject와 같을 경우 패키지명은 생략 가능합니다.
within(ArithmeticCalculatorImpl)
```

특정 인터페이스를 구현한 모든 클래스의 메서드 실행 조인퍼인트를 매치하려면 맨 뒤에 `+`기호를 붙여주면 됩니다.

```java
within(ArithmeticCalculator+)
```

Custom Annotation을 매칭시킬 수도 있습니다.

```java title:"ArithmeticCalculatorImpl.java"
@LoggingRequired
public cass ArithmeticCalculatorImpl implements ArithmeticCalculator {
	...
}
```

```java 
@Pointcut("within(com.apress.springrecipes.calculator.LoggingRequired)")
```

### 포인트컷 표현식 조합하기
AspectJ Pointcut 표현식은 `연산자(&&, ||, !, ...)`를 활용할 수 있습니다.

```java
within(ArithmeticCalculator+) || within(UnitCalculator+)

// 또는

@Aspect
public class CalculatorPointcuts {

	@Pointcut("within(ArithmeticCalculator+)")
	public void arithmeticOeration() {}

	@Pointcut("within(UnitCalculator+)")
	public void unitOperation() {}

	@Pointcut("arithmeticOperation() || unitOperation()")
	public void loggingOperation() {}
}
```

### 포인트컷 매개변수 선언하기
위에서 언급한 바와같이 우리는 JointPoint 객체를 통해 조인트포인트 정보를 가져 올 수 있습니다.
JointPoint를 선언하지 않고 표현식을 통해 어드바이스에 매개변수로 사용할 수 있습니다.

```java title:"CalculatorLoggingAspect.java"
@Aspect
public class CalculatorLoggingAspect {

	...
	@Before("excution(* *.*(..)) && target(target) && agrs(a,b)")
	public void logParameter(Object target, double a, double b) {
		log.info("Target class : )" + target.getClass().getName());
		log.info("Arguments : " + a + ", " + b);
	}
}
```

독립적인 Pointcut을 선언해 사용할 경우 Pointcut 메서드의 인수 목록에도 함께 넣습니다.

```java title:"CalculatorPointcuts.java"
@Aspect
public class CalculatorPointcuts {

	...
	@Pointcut("excution(* *.*(..)) && target(target) && agrs(a,b)")
	public void parameterPointcut(Object target, double a, double b) {}
}
```

위에 매개벼수화환 포인컷을 참조하게 만들면 됩니다.

```java title:"CalculatorLoggingAspect.java"
@Aspect
public class CalculatorLoggingAspect {

	...
	@Before("CalculatorPointcuts.parameterPointcut(target, a, b)")
	public void logParameter(Object target, double a, double b) {
		log.info("Target class : )" + target.getClass().getName());
		log.info("Arguments : " + a + ", " + b);
	}
}
```

