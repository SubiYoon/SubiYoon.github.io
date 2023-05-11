# Maven

# Maven이란??

- 프로젝트의 전반적인 라이프사이클을 관리하는 도구
- 프로젝트 객체 모델(Project Object Model : POM)이라는 개념을 바탕으로 프로젝트의 의존성, 라이브러리, 라이프사이클 관리 기능 등을 제공하는 프로젝트 관리 도구

<img width="857" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2023-04-19_%EC%98%A4%EC%A0%84_9 04 34" src="https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/5b8658f1-587a-4655-8c29-541c4745b76a">


---

# 프로젝트 객체 모델 POM

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/maven-v4_0_0.xsd">
 
 <modelVersion>4.0.0</modelVersion>  <!--POM model의 버전-->
  <groupId>egovframework.dev.com</groupId>  <!--프로젝트를 생성하는 조직의 고유 아이디를 결정-->
  <artifactId>egovframework-dev-com</artifactId> <!--프로젝트 빌드시 파일 대표명 --> 
  <version>1.0</version>   <!--현 프로젝트의 버전 --> 
  <packaging>war</packaging>  <!--패키징 유형(jar, war, ear 등)-->
  <name>egovframework-dev-com Maven Webapp</name> <!--프로젝트 이름-->
  
  <dependencies>  <!--dependencies 태그 안에는 프로젝트와 의존관계에 있는 라이브러리들을 관리-->
    <dependency>
      <groupId>junit</groupId> 
      <artifactId>junit</artifactId>
      <version>4.4</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  
  <plugins> <!--빌드에 사용할 플러그인 목록--> 
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <configuration>
        <source>1.5</source>
        <target>1.5</target>
      </configuration>    
    </plugin>
  </plugins>
  
</project>
```

---

# **메이픈 레파지토리 Repository**

- **artifact** 들의 저장소
- 로컬 및 리모트로 구성되며 프로젝트는 pom.xml에서 선언한 dependency들을 저장소로부터 불러와 사용
- 메이븐 설치시 .m2폴더에 생성됨
    - settings.xml에 원하는 로컬 저장소의 경로를 지정할 수 있음
- settings.xml
    - maven build tool  관련 설정파일
    - Maven_home/conf 디렉토리에 위치한다
- 원격 저장소(remote repository)
    - 메이븐에서 제공하는 중앙저장소 or 각 기업에서 사용하는 사설 원격저장소(ex.Nexus 등)
    - 환경설정을 통해 하나이상의 외부 레파지토리에서 필요로 하는 arifact들을 다운로드 받아 사용 가능
- pom.xml에 선언하여 사용 및 관리한다
- **호출 프로세스**
    1. 해당 프로젝트의 pom.xml에 라이브러리 선언
    2. 개발자 **로컬 레파지토리**에서 선언한 라이브러리 검색
        1. 존재 시 프로젝트에 라이브러리 cache
        2. 미존재시 원격 레파지토리에 요청
    3. **원격 레파지토리**에서 선언한 라이브러리 검색
        1. 로컬 레파지토리에 원격 레파지토리에서 검색된 artifact를 내려받아 저장

---

# 메이븐 디렉토리 구조

- best practices를 기반으로 정규화된 디렉토리 구조를 제공
- 모든 소스파일은 /src에 블드된 output은 /target에 위치
    
    <img width="884" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2023-04-19_%EC%98%A4%EC%A0%84_9 36 48" src="https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/525f32c0-e119-48d9-a65d-c503cb86966e">


---

# **메이븐 라이프 사이클 Life Cycle**

- clean → 빌드 시 생성되었던 파일들을 모두 삭제
- vaildate → 프로젝트가 올바른지 확인하고 필요한 모든 정보를 사용할 수 있는지 확인
- compile → 프로젝트 소스코드 컴파일
- test → 유닛(단위)테스트를 수행, 실패시 빌드 실패로 처리(스킵 가능)
- package → jar, war 등 배포를 위한 패키지를 만듬
- verify → 통합 테스트결과에 대한 검사를 실행하여 품질기준을 충족하는지 확인
- install → 패키지를 로컬 저장소에 설치
- site → 프로젝트 문서와 사이트 생성
- deploy → 만들어진 패키지를 원격 조장소에 Release

- 단계순서에 따라 순차적으로 빌드가 실행된다.
    - mvn install 실행시, compile → test → package → verify → install 순으로 실행 됨

---

# 메이븐 플러그인 Plugins

- 메이븐은 여러 플로그인으로 구성되어 있으며 각각의 플러그인은 하나이상의 **goal**(명령, 작업)을 포함
- 메이븐은 대부분의 빌드 작업시 플러그인을 활용
- 메이븐 엔진이 생명주기 단계들과 플러그인을 바인딩 시켜 빌드 작업을 수행
- 플러그인은 다른 artifact와 같이 저장소에서 관리
- 여러 goal을 묶어서 lifecycle phases로 만들고 실행
    - ex. mvn clean install

<img width="886" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2023-04-19_%EC%98%A4%EC%A0%84_10 11 39" src="https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/1fbbd46d-5bac-4fae-bdb2-e0039f920cb3">


---

# **의존성 관리**

- 메이븐은 pom.xml에 선언된 dependency설정으로 빌드 및 배포시 필요한 라이브러리들을 관리한다
- 의존성 분석 순서
    1. Local Repository에서 의존성 확인
    2. Remote Repository에서 의존성 확인
    3. 위 두 과정 실패시 의존성 에러 보고
- 의존성 <dependency> 필수 선언 항목
    - <groupid> → 부분적인 프로젝트나 조직에서의 라이브러리 집합을 식별하기 위해 제공
    - <artifactid> → 프로젝트의 실제 이름으로 groupid와 합쳐져 프로젝트 식별에 사용
    - <version> → 선언한 의존성 artifact의 버전

---

# 용어 사전

- **artifact**
    
    → 자바 프로젝트를 빌드할 때 생성되는 결과물을 의미
    
- **로컬 레파지토리 (Local Repository)**
    
    → 로컬파일 시스템에 cache하는 기능으로 원격 레파지토리에서 다운로드 받아 artifact들을 저장/관리
    
- **원격 레파지토리 (Remote Repository)**
    
    → 주로 http서버로 thirdparty에서 제공하는 artifact들을 제외한 모든 arifact들을 제공
    
- **goal**
    
    → 메이븐의 실행 단위
