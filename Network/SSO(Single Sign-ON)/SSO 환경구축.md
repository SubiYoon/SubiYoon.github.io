
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
docker run -p 8080:8080 jboss/keycloak
~~~
2. 브라우저에서 8080포트로 접속시 해당 페이지가 나온다면 성공!
![[스크린샷 2023-10-26 오후 10.19.16.png]]

---
# Keycloak 환경설정
- 우선 해당 서버에 SSH접속에 터널을 사용하여 접속한다.
~~~bash
SSH -p ["포트"] ["계정"]@["ip"] -L ["로컬포트"]:localhost:["타겟포트"]
~~~
- Keycloak을 설치하면 기본적으로 관리자 계정이 없어 해당 계정을 만들어 주어야 한다.
- 우선 터미널을 두개 켜서 하나는 keycloak 이미지를 도커로 실행시켜 준다.
~~~bash
docker run -p 8080:8080 jboss/keycloak

# 유주럴 추가해준다.
docker exec ["컨테이너 ID"] /opt/jboss/keycloak/bin/add-user-keycloak.sh -u admin -p admin123$
~~~

- 아래와 같이 결과가 나왔다며 성공적으로 추가가 된 것이다.
![[스크린샷 2023-10-28 오전 10.52.39.png]]

- 이제 도커를 재시작한다.
~~~bash
docker restart ["컨테이너 ID"]
~~~

- 만약 아래와 같이 관리자 콘솔이 나타나지 않으면 터널링을 통해 Localhost로 변경해주어야 한다.
![[스크린샷 2023-10-28 오전 11.34.30.png]]

- 아래와 같은 화면으로 진입이 가능하면 성공!
- 이제 아까 쉘스크립트에 작성한 user 정보로 로그인을 하면 된다.
![[스크린샷 2023-10-28 오전 11.39.55.png]]

---
# Realm, User, Client

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn6O7t%2FbtrZA1S7BQs%2FGrqj74xurQ9Igu8Czf2zV0%2Fimg.png)

## **Realm**이란??
- Realm은 사용자, 인증, 인가, 권한, 그룹이 관리하는 범위이다.
- 해당 Realm안의 Client(위사진에서는 Application)들은 서로 SSO를 공유하게 된다.
- 처음 관리자 계정으로 로그인하면 Master Realm만 존재하는데 해당 Realm은 다른 Realm을 관리할 때에만 사용한다.

## **User**란??
- User는 Client를 이용하는 사용자를 뜻한다.
- 한 User안에 있는 Client(위사진에서는 Application)끼리만 SSO를 공유하고, 다른 User에게 간섭 할수 없다.

## **Client**란??
- Client란 인증, 인가 업무를 Keycloak에게 요청할 수 있는 주로 어플리케이션이나 서비스를 뜻한다.
- Keycloak은 Client들에게 보안이나, SSO를 제공한다.


## Realm 생성
- 로그인 후 좌측 네비바에서 새로운 realm을 생성해준다.
![[스크린샷 2023-10-28 오후 12.27.06.png]]
![[스크린샷 2023-10-28 오후 12.28.49.png]]

## User 생성
- Realm생성후 좌측 네비바에서 Users를 클릭해 새로운 사용자를 생성해고, 비밀번호도 새로 할당한다.
![[스크린샷 2023-10-28 오후 12.29.15.png]]
![[스크린샷 2023-10-28 오후 12.29.49.png]]
![[스크린샷 2023-10-28 오후 12.30.09.png]]

## Client 생성
- 이제는 client를 생성할 차례.. 좌측 네비바에서 clients를 클릭하여 화면으로 진입한다.
- **Client**는 Application인증을 위한 단위라고 보면 된다.
- 아래와 같이 세팅해서 생성해준다.
- 연습은 H2 DB를 사용하가 위해 8082포트를 사용하기로 한다.
![[스크린샷 2023-10-28 오후 12.39.26.png]]

- Clinet로 돌아와 account-console의 base URl을 클릭하여 로그인 후 해당 화면이 나오면 정상!
![[스크린샷 2023-10-28 오후 12.55.52.png]]
![[스크린샷 2023-10-28 오후 12.56.46.png]]

---