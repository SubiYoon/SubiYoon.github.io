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

각 Annotation별로 어떻게 작용하는지 잠깐 참고해 보세요.

![image](https://github.com/SubiYoon/SubiYoon.github.io/blob/main/Attached%20File/Pasted%20image%2020240522171850.png?raw=true)

IoC Container에서 Aspect Annoation 기능을 활성화하려면 구성 Class 중 하나`@EnableAspectJAutoProxy`를 붙입니다.
