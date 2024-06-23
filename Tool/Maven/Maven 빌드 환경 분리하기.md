---
tags:
  - Tool
  - Maven
  - Configuration
---
# Preview
우리는 개발을 하면서 항상 지긋지긋하게 OS별 폴더구조의 불편함에 대해  느끼게 됩니다.
특히나 `window`의 사용자가 `linux`환경에 배포하여 사용한다면 말이죠.

포트폴리오 사이트를 개발하던중 이미지를 저장함에 있어 로컬에서와 운영시 폴더 접근에 대한 문제 때문에 공부를 하게되었습니다.

그래서...!!

오늘은 환경별 구성 요소를 선택할 수 있게 하는 방법을 알아보겠습니다.

### 구성 환경
* spring boot 3.x
* .war 파일을 이용한 배포

### .properties 분리하기
우선 가장 먼저 해주어야 할건 폴더를 나누는 것입니다.
모든 환경에서 공통적으로 사용해야 하는 파일까지 나누어서 관리할 필요는 없겠죠??
공통적으로 사용해야하는 `.xml`, `.properties` 등과 같은 파일은 `resources`폴더에 넣습니다.
그리고 각 환경에 따라 폴더를 새로 만들어 줍니다.

저의 경우 로컬 환경에 `local`, 운영 환경에 `prod`명칭을 붙여 사용할 겁니다.
그렇다면 폴더구조는 다음과 같습니다.
![Maven 빌드 환경 분리하기](https://github.com/SubiYoon/SubiYoon.github.io/blob/main/Attached%20File/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-06-23%20%EC%98%A4%ED%9B%84%2010.11.36.png?raw=true)

Spring Boot에서는 `build`시 입력하는 프로파일 변수값에 따라 해당 폴더를 자동으로 찾아줍니다.
`resource-**`에서 `**`부분의 값을 찾는 것이죠.

### pom.xml
이제 우리는 각 환경에 따라 `build`시에  포함시키거나, 제외시키고 싶은 파일들도 존재할 겁니다.
저의 경우는 `.../resources/static`폴더의 경우 운영에서 따로 만들어서 사용하기에 제외시켜야 합니다.

우선, 환경별로 빌드하는 코드를 먼저 보겠습니다.

`<profiles>`태그에 우리가 나중에 빌드시 사용할 각각의 환경을 명시합니다.
`<properties>`태그가 보이시나요?? 우리가 사용할 .properties파일을 지정하기 위한 변수를 설정해 주는 곳입니다.

그럼 각각 어떤 태그들인지 알아 봅시다.

환경구성
* `profiles` : 환경구성들을 새팅하는 태그
* `profile` : 하나의 환경을 구성하는 태그 
* `id` : 해당 환경구성 설정의 이름
* `activationByDefault` : `ture`로 설정하면 변수값을 입력 안할 시 default로 사용하는 환경

빌드구성
* `build` : 빌드구성들을 세팅하는 태그
* `resources` : 각각의 리소스들에 대한 세팅을 하는 태그
* `directory` : 해당 리소스의 위치
* `includes` : 빌드시 포함할 리소스들을 명시
* `include` : 각 각의 파일 or 패턴을 하나하나 입력
* `filtering` : 리소스안의 `${...}`로 표기된 부분을 알아서 치환하여 바인딩하는 작업 여부

아래 `<directory>`태그에서 `${environment}`가 보이시나요? `<profiles>`태그에서 사용한 변수명도 보이시나요??

이해하셨다면 훌륭합니다!!

아래 빌드 내용을 보면

1. `src/main/java`의 `.properties`, `.json`, `.xml`이면 모두 포함시킨다.
2. `src/main/resources` 폴더를 포함시킨다. 리소스안의 변수 값은 치환한다.
3. `src/main/resources-${environmnet}` 해당 폴더의 리소스 중  `.properties`, `.json`, `.xml`파일을 모두 포함시킨다.

이해가 안되셨다면, 하나하나 직접 해보는 것을 추천합니다.

```xml title:"pom.xml"
<profiles>
    <profile>
        <id>local</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
            <environment>local</environment>
        </properties>
        </profile>
    <profile>
        <id>prod</id>
        <properties>
            <environment>prod</environment>
        </properties>
    </profile>
</profiles>

<build>  
    <resources>
        <resource>  
            <directory>src/main/java</directory>  
            <includes>
                <include>**/*.properties</include>  
                <include>/**/*.json</include>  
                <include>**/*.xml</include>  
            </includes>  
        </resource>  
        <resource>
            <directory>src/main/resources</directory>  
            <filtering>true</filtering>  
        </resource>  
        <resource>
            <directory>src/main/resources-${environment}</directory>  
            <filtering>true</filtering>  
            <includes>
                <include>**/*.properties</include>  
                <include>**/*.josn</include>  
                <include>**/*.xml</include>  
            </includes>  
        </resource>  
    </resources>  
</build>
```

### 특정 파일/폴더 제외 시키기
물론 이 작업도 `pom.xml`에서 작업합니다.
바로 `<plugins>`태그를 이용해서 말이죠. 길게 설명하지 않고 넘어가겠습니다.

아래 내용은 이렇습니다.
1. 플러그인에 lombok을 포함시킨다.
2. war로 배포할 시, **/static 폴더는 배제시킨다.

```xml title:"pom.xml"
<plugins>  
    <plugin>  
        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-maven-plugin</artifactId>  
        <configuration>
            <excludes>  
                <exclude>  
                    <groupId>org.projectlombok</groupId>  
                    <artifactId>lombok</artifactId>  
                </exclude>  
            </excludes>  
        </configuration>  
    </plugin>  
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>  
        <artifactId>maven-war-plugin</artifactId>  
        <configuration>
            <packagingExcludes>  
                **/static/**  
            </packagingExcludes>  
        </configuration>  
    </plugin>  
</plugins>  
```
