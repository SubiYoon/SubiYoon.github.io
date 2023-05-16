# Log란??

- 로그(Log)는 소프트웨어의 이벤트를 기록하는 것
- 동작상태를 파악하고 문제가 발생했을 때 이 동작 파악을 통해 문제를 찾아 해결하기 위해 디자인 되었다.
- 디버깅 or 모니터링을 위해 정보를 기록하는데 사용
- Debugger, println의 차이점
    - 상황별 Level을 지정하여 Level별 선택가능
    - 응용 프로그램의 실행에 대한 흐름과 에러 확인 가능
    - 프레임워크를 이용하여 간단하고 쉬운 사용환경 조성 가능
    - 모듈 별로 유연하게 메세지 출력 가능
    - 자유로운 출력 위치 및 다양한 출력 형식 지원

# Log4j

- 가장 오래된 로깅 프레임워크이면서 2015년 기준 개발이 중단되었다.

**log4j의 구성**

| 요소 | 설명 |
| --- | --- |
| Logger | 출력할 메시지를 Appender에게 전달 |
| Appender | 전달된 로그를 어디에 출력할 것이지 결정(Console, File, JDBC 등) |
| Layout | 로그를 어떤 형식으로 출력할 것인지 결정 |

**log4j의 로그 레벨**

- FATAL > ERROR > WARN > INFO > DEBUG > TRACE

| 로그 레벨 | 설명 |
| --- | --- |
| FATAL | 아주 심각한 에러가 발생한 상태 |
| ERROR | 요청을 처리하는 중 문제가 발생한 상태 |
| WARN | 실행에는 문제가 없지만 향후 에러의 원인이 될 수 있는 경고성 메시지 |
| INFO | 상태변경과 같은 정보성 메시지 |
| DEBUG | 개발시 디버그 용도로 사용하는 메시지 |
| TRACE | 디버그 레벨 보다 상세한 이벤트를 나타내는 메시지 |

---

# Logback

- log4j 이후에 출시된 보다 향상되고 가장 널리 사용되고 있는 Java 로깅 프레임워크 중 하나이다.
- slf4j의 구현체로써 동작하며 SpringBoot 환경의 경우 spring-boot-starter-web 안에 spring-boot-starter-logging의 logback이 기본적으로 탑재되어 있어 별다른 dependency추가 없이 사용 가능하다.

![image](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/64bd5570-ac9b-4252-9f96-430ac2bc8be9)

- logback은 log4j에 비해 향상된 필터링 정책 및 기능(ex. 로그인 한 경우 logging 등)과 로그 레벨 변경등에 대해 서버 재시작 없는 자동 리로딩을 지원해준다.

**참고자료**

[[Logback] Logback이란? log4J의 후속작 로그백(Logback) 살펴보기 및 비교](https://dololak.tistory.com/632)

---

# Log4j2

- 가장 최신에 나온 로깅 프레임워크로써 Apache의 log4j의 다음 버전이다.
- logback처럼 필터링 기능과 자동 리로딩을 지원한다.
- logback과의 가장 큰 차이는 Multi Thread환경에서 비동기 로거(Async Logger)의 경우 다른 로깅 프레임워크보다 처리량이 훨씬 많고, 대기 시간이 훨씬 짧다.
- java8부터 도입된 람다식을 지원하고, Lazy Evalutation을 지원한다.

![image 1](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/10d78061-a274-472d-81cd-0c1c95d19736)

---

# Slf4j(Simple Logging Facade For java)

- **lombok @Slf4j 커스텀 설정 및 적용 방법 간단 정리**
    
    # **1. pom.xml 설정**
    
    - 버전관리를 쉽게하기 위해 <dependencies> 태그 시작 전 위에 별도의 properties를 설정
    
    ```xml
    <properties>
    	<java-version>1.8</java-version>
    	<org.slf4j-version>1.6.6</org.slf4j-version>
    </properties>
    ```
    
    - <dependencies> 안에 위 xml 내용을 삽입한다.
    
    ```xml
    <!-- Logging -->
    <dependency>
    	<groupId>org.slf4j</groupId>
    	<artifactId>slf4j-api</artifactId>
    	<version>${org.slf4j-version}</version>
    </dependency>
    <dependency>
    	<groupId>org.slf4j</groupId>
    	<artifactId>jcl-over-slf4j</artifactId>
    	<version>${org.slf4j-version}</version>
    	<scope>runtime</scope>
    </dependency>
    <dependency>
    	<groupId>org.slf4j</groupId>
    	<artifactId>slf4j-log4j12</artifactId>
    	<version>${org.slf4j-version}</version>
    	<scope>runtime</scope>
    </dependency>
    <dependency>
    	<groupId>ch.qos.logback</groupId>
    	<artifactId>logback-classic</artifactId>
    	<version>1.2.3</version>
    </dependency>
    ```
    
    ---
    
    # **2. lombok 설치**
    
    [Download](https://projectlombok.org/download)
    
    ---
    
    # **3. logback.xml 생성**
    
    - 경로 : **src/~/resources 폴더 안에 logback.xml 파일을 생성한다.**
    - 아래 내용을 붙여 넣는다.
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration scan="true" scanPeriod="10 seconds">
    	<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
    		<encoder>
    			<Pattern>%d{HH:mm:ss} [%-5level] %logger{36}[line: %L] - %msg%n</Pattern>
    		</encoder>
    	</appender>
    
    	<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    		<file>/tmp/access.log</file>
    		<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
    			<fileNamePattern>/tmp/access-%d{yyyy-MM-dd}.log</fileNamePattern>
    			<maxHistory>50</maxHistory>
    		</rollingPolicy>
    
    		<encoder>
    			<Pattern>%d{HH:mm:ss} [%-5level] %logger{36}[line: %L] - %msg%n</Pattern>
    		</encoder>
    	</appender>
    
    	<logger name="com.artiplace.api" level="debug"/>
    
    	<root level="debug">
    		<appender-ref ref="CONSOLE"/>
    		<appender-ref ref="FILE"/>
    	</root>
    </configuration>
    ```
    
    <appender>
    
    - 어디에 어떻게 로그를 남길 것인지 방법을 작성하여 제공
        - ConsoleAppender : 콘솔에 로그를 어떻게 남길 것인지 작성
        - FileAppender : 파일에 로그를 어떻게 남길 것인지 작성
        - RollingFileAppender : 하루 단위로 로그 파일을 백업하면서 로그를 남기기 위한 방법을 작성
    
    <Pattern>
    
    - 로그 남기는 패턴(방법)을 정리.
        - HH:mm:ss 는 시:분:초 를 의미.
        - maxHistory 는 로그 파일의 내용 최대 개수
        - %-5level 은 trace, debug, info, warn, error 총 5가지의 레벨이 존재하는데
        이 중 로그에 기록하고싶은 레벨을 표현한 것.
    
    <logger>
    
    - name은 패키지 경로를 넣으면, 해당 패키지에 속한 클래스에서 출력하는 로그는 level 이상의 로그를 출력하라는 뜻
    
    <appender-ref>
    
    - <appender> 참조
    
    <root>
    
    - root(패키지의 루트) 하위 대상 전부 해당 레벨에 포함하는 로그 기록
    
    ---
    
    # **4. java파일에서 로그 사용**
    
    ![https://blog.kakaocdn.net/dn/buUuEe/btq3IS7iugN/NFqSLCt4O96Em3MfcP3Qz1/img.png](https://blog.kakaocdn.net/dn/buUuEe/btq3IS7iugN/NFqSLCt4O96Em3MfcP3Qz1/img.png)
    
    @Slf4j 어노테이션은 클래스 위에
    
    ![https://blog.kakaocdn.net/dn/u5zIl/btq3PQTJvol/a6jEDseHzhH15AUj9oKbp0/img.png](https://blog.kakaocdn.net/dn/u5zIl/btq3PQTJvol/a6jEDseHzhH15AUj9oKbp0/img.png)
    
    함수 내부에서는 log.debug()를 통해 사용하면 끝!
    

- 자체적인 로킹 프레임워크가 아닌 logger추상체로써 다른 로깅 프레임워크가 접근 할 수 있도록 도와준다.
- 즉, logback이나 log4j2와 같은 로깅 프레임워크의 인터페이스의 역할을 해준다.
- 코드를 일정하게 유지하면서 구현체의 전환을 통해 다른 로깅 프레임워크로의 전환을 쉽고 간단하게 해준다.
