안녕하세요.
이번 게시글은 데이터 관리를 위해 Spring Boot에 Mybatis와 PostgreSQL을 접목 시키는 방법을 설명하겠습니다.

우선 환경은 다음과 같습니다.

| 구분                  | 언어 / 라이브러리 |
| --------------------- | ----------------- |
| 언어                  | Java17            |
| 프레임워크            | Spring Boot 3.2.1 |
| Persistence Framework | Mybatis 3.0.3     |
| 자동화 빌드 툴        | Maven             |

보통 Spring Boot의 데이터 흐름은 아래와 같습니다.<br>
Spring Boot와 Spring의 용어 차이는 아래와 같으니 참고하시기 바랍니다.<br>
(자주보는 그림이 될수도 있습니다. 중요한 부분이기에 다른글에서도 자주 등장할 예정입니다.)
![](https://github.com/SubiYoon/SubiYoon.github.io/blob/main/Attached%20File/스크린샷%202024-01-13%20오후%202.24.18.png?raw=true)

# 설정 파일 구성

### pom.xml
우선 **pom.xml**에 우리가 필요로하는 **dependency**를 추가해 주어야 합니다.
```xml
<!--mybatis-->
<dependency>  
    <groupId>org.mybatis.spring.boot</groupId>  
    <artifactId>mybatis-spring-boot-starter</artifactId>  
    <version>3.0.3</version>  
</dependency>
<!--테스트 파일을 생성하여 테스트하기를 위한다면 추가 Start-->
<dependency>  
    <groupId>org.mybatis.spring.boot</groupId>  
    <artifactId>mybatis-spring-boot-starter-test</artifactId>  
    <version>3.0.3</version>  
    <scope>test</scope>  
</dependency>
<!--테스트 파일을 생성하여 테스트하기를 위한다면 추가 End-->
<!--postgreSQL-->
<dependency>  
    <groupId>org.postgresql</groupId>  
    <artifactId>postgresql</artifactId>  
    <scope>runtime</scope>  
</dependency>
```

### application.properties
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/${DBname}
spring.datasource.username=${userId}
spring.datasource.password=${userPassword}  

# 데이터를 반환 받을때 snake case를 camel case로 변환여부
mybatis.configuration.map-underscore-to-camel-case=true  

# Mybatis에서 resultType에 vo의 full path를 입력하지 않고 vo명만 명시해도 데이터 받아올 수 있음
mybatis.type-aliases-package=com.database.link.**.vo

# Mybatis에서 mapper파일(.xml)의 위치를 집기위한 설정
mybatis.mapper-locations=classpath:/mapper/**/*.xml
```
