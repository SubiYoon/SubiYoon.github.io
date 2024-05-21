# 필수개념
### POJO(Plain Old Java Object)??
POJO란 직역하자면 순수한 오래도니 자바 객체입니다.
Java로 생성하는 순수한 객체를 의미합니다.
객체지향 원리에 충실하면서 환경 및 기술에 종속되지 않고, 필요에 따라 재활용할 수 있다는 것이 가장 큰 특징입니다.

### Spring Container를 사용하는 이유
우리는 객체를 생성하기 위해 `new`라는 연산자를 사용해야 한다.
객체간의 참조가 많으면 많을수록 의존성이 높아지게 된다.
이는 낮은 결합도와 높은 캡슐화를 지향하는 객체지향 프로그래밍의 핵심과는 반대의 방식이다.
따라서, **객체간의 의존성을 낮추어 결합도는 낮추고, 높은 캡슐화를 위해 사용**한다.

### IoC Container
Spring에서 객체를 생성하고 관리하고 책임지고 의존성을 관리해주는 Container로 `BeanFactory`와 `ApplicationContext`가 있다.
* 장점
	* 객체의 생성을 책임지고 의존성을 관리
	* POJO의 생성, 초기화, 서비스, 소멸에 대한 권한을 가짐
	* 개발자가 직접 POJO를 생성할 수 있지만, Container에게 맡김
	* 개발자는 비지니스 로직에 집중할 수 있도록 도와줌
	* TDD가 용이함

![image](https://github.com/SubiYoon/SubiYoon.github.io/blob/main/Attached%20File/Pasted%20image%2020240521141058.png?raw=true)

### BeanFactory
Spring Container의 최상위 Interface이다.
Bean의 등록, 생성, 조회 등의 Bean을 관리하는 역할을 한다.
`getBean()` 메서드를 통해 Bean을 인스턴스화 할 수 있다.

### ApplicationContext
ApplicationContext는 BeanFactory의 기능을 상속받아 제공한다.
Bean을 관리하고 검색하는 기능을 BeanFactory가 담당하고, 그 외의 부가 기능을 제공한다.
* MessageSource : 다국어 지원
* EnvironmentCapable : Application 구동환경 분할(개발, 운영, 환경변수 등)
* ApplicationEventPublisher : 이벤트 관련 기능을 제공하는 Interface
* ResourceLoader : File, ClassPath 등 Resource를 편리하게 조회
다음과 같은 방법으로 Bean을 가져올 수 있다.
`getBean()`을 하는 방법은 `@Autowired`를 하는 방법과 같은 효과를 가진다.
`getBean(${BeanName}, ${Type})`으로 작성하여 사용한다.
```java 
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
```
