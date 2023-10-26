
# SSO 환경 구축

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

-  Docker를 이용하여 사용시 docker의 권한문제가 발생 할 수 있다. 아래 명령어를 순서대로 입력 후 재부팅.
~~~bash
sudo groupadd docker
sudo usermod -aG docker $USER

# 666권한은 잠재적으로 위험할 수 있으니 신중해야 한다.
sudo chmod 666 /var/run/docker.sock

# 도커 재시작
systemctl restart docker.service
sudo newgrp docker
~~~

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

### 우분투에 keyCloak설치 (Docker Image 버전)
1. docker 명령어를 사용해 해당 이미지가 로컬에 없으면 다운받게 할 수 있다.
~~~bash
docker run jboss/keycloak

# 로컬에 띄우고 싶다면 아래 명령어를 사용하면 된다.
docker run -p 8080:8080 jboss/keycloakㅠ
~~~
2. 브라우저에서 8080포트로 접속시 해당 페이지가 나온다면 성공!
![[스크린샷 2023-10-26 오후 10.19.16.png]]