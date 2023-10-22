
# SSO 구축

## 🚀구축 조건🚀
- String Boot
- Java 11
- Ubuntu 22.04.3 LTS Server
- Docker 환경 구축
- Docker를 이용한 Keycloak
- 각각 두개의 게시판 서버를 만들어 SSO 구축

---
# 1.  Ubuntu 설치 및 도커 설치
- 우분투 서버를 설치하고 설치 시 도커도 함께 설치해준다.

---
# 2. Java설치

### 우분투에 Java 설치
1. 시스템 업데이트를 진행한다.
~~~bash
sudo apt-get update
~~~

2. Java 설치
~~~bash
sudo apt install openjdk-11-jdk
~~~
![[스크린샷 2023-10-22 오후 1.41.25.png]]

---
# 3. DB설치
### MySQL 설치
1. MySQL을 설치
2. 설치시 비밀번호를 따로 설정하지 않았다면 기존 Ubuntu의 비밀번호와 비밀번호가 같다.
~~~bash
sudo apt-get install mysql-server
~~~

### 외부접속 기능 설정 (3306포트 오픈)
~~~bash
sudo ufw allow mysql
~~~

### MySQL 실행
~~~bash
sudo systemctl start mysql
~~~

### Ubuntu서버 재시작 시 MySQL 자동 재시작
~~~bash
sudo systemctl enable mysql
~~~

### MySQL 접속
~~~bash
sudo /usr/bin/mysql -u root -p
~~~


---
# 4. Keycloak 설치
### 우분투에 Keycloak 설치(일반 커멘드 버전)
1. Keycloak.tar파일을 다운받는다.
~~~bash
wget https://github.com/keycloak/keycloak/releases/download/17.0.0/keycloak-17.0.0.tar.gz
~~~

2. tar파일을 압축 해제 한다.
~~~bash
tar -zxvf keycloak-17.0.0.tar.gz
~~~

3. Keycloak을 시작하기 전에 관리자 계정을 생성해주어야 한다.
~~~bash
export KEYCLOAK_ADMIN=admin
export KEYCLOAK_ADMIN_PASSWORD=admin
~~~

4. 내보낸 환경변수를 적용하여 Keycloak을 실행시켜본다.
~~~bash
sudo -E bin/kc.sh start-dev
~~~

5. 주소창에 **"서버IP":8080**을 검색하여 아래와 같이 화면이 나온다면 성공
![[스크린샷 2023-10-22 오후 1.59.21.png]]

6. 로그인에 성공하면 아래와 같은 Dashboard화면이 나와야 한다.
![[스크린샷 2023-10-22 오후 2.00.48.png]]

### 우분투에 KeyCloak설치 (Docker Compose 버전)
1. 원하는 directory를 생성한다. (keycloakServer)
~~~bash
mkdir keycloakServer
~~~

2. 설치에 필요한 yml파일을 다운로드 한다.
~~~bash
wget https://github.com/keycloak/keycloak-containers/blob/88b2f48bd332c9ceed6ec681a99762f73e3bf16e/docker-compose-examples/keycloak-postgres.yml
~~~

3. 위파일은 pstaresSQL로 되어 있고, 필자는 MySQL을 활용할 것이므로 아래 파일을 사용했다.
~~~yml
# keycloak-postgres.yml

version: "3.8"
services:
  mysql_8:
    image: mysql:8.0.34
    container_name: mysql_8
    environment:
      - MYSQL_DATABASE=keycloak
      - MYSQL_USER=board01
      - MYSQL_PASSWORD=test01
      - MYSQL_ROOT_PASSWORD=test01
    ports:
      - "3308:3306" # 포트는 3308번으로 생성하겠습니다.
    healthcheck:
      test: "mysqladmin ping -u root -p$${MYSQL_ROOT_PASSWORD}"
~~~

4. Docker를 이용하여 사용시 docker의 권한문제가 발생 할 수 있다. 아래 명령어를 순서대로 입력 후 재부팅.
~~~bash
sudo groupadd docker
sudo usermod -aG docker $USER
sudo newgrp docker
~~~
 