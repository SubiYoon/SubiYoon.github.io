---
tags:
  - Linux
  - systemctl
---
### systemctl 이란??
systemctl은 서비스들을 관리할 수 있는 유틸리티다.
현재 관리하고 싶은 서비스들의 상태를 확인할 수 있으며, 시작, 중지 등 다양한 명령을 내릴 수 있다.
부팅시 서비스를 자동으로 실행해주는 등 백그라운드 서비스를 관리할때 사용한다.

### systemctl의 명령어
```bash
# 서비스 시작
sudo systemctl start ${SERVICE-NAME}
# 서비스 재시작
sudo systemctl restart ${SERVICE-NAME}
# 서비스 중지
sudo systemctl stop ${SERVICE-NAME}
# 서비스 상태 확인
sudo systemctl status ${SERVICE-NAME}
# 부팅시 자동 시작 활성화
sudo systemctl enable ${SERVICE-NAME}
# 부팅시 자동 시작 비화성화
sudo systemctl disable ${SERVICE-NAME}
# 서비스 로그 보기
sudo journalctl ${SERVICE_NAME}
# systemctl 내에 등록되어 있는 unit확인
sudo systemctl list-units
```

### systemctl 설정
우산 해당 파일을 생성할 폴더에 이동한다.
```bash
cd /etc/systemd/system
```

사용하고자하는 서비스의 이름을 작성한다. 나의 경우에는 tomcat으로 작성하고 실행해 보려고한다.
그런 다음 파일을 생성 후 아래 내용을 삽입하자.
* ${JAVA-HOME-PATH} : java가 설치되어 있는 경로를 작성하자
* ${TOMCAT-PATH} : tomcat이 설치괴어 있는 경로를 작성하자
```bash
# sudo vi tomcat.service

[Unit]  
Description=Tomcat
After=syslog.target network.target

[Service]  
Type=forking

Environment="JAVA_HOME=${JAVA-HOME-PATH}"  
Environment="CATALINA_HOME=${TOMCAT-PATH}"

User=root  
Group=root

ExecStart=${TOMCAT-PATH}/bin/startup.sh  
ExecStop=${TOMCAT-PATH}/bin/shutdown.sh

[Install]  
WantedBy=multi-user.target
```

### Daemon-reload 하기
위와 같이 파일을 작성했으면 해당 항목을 적용 시켜주어야 한다.
다음 명령어를 실행하여 systemctl에 등록해주도록 하자.
```bash
sudo systemctl daemon-reload
```

### 서비스 시작 및 설정하기
모든 준비는 완료 되었다.
시스템을 실행하고 부팅시에 동작할 수 있게 설정해주자.
```bash
# 기존에 tomcat이 실행중이라면 restart, 정지상태이면 start
sudo systemctl restart tomcat.service
# 정상 동작하는지 상태를 확인
sudo systemctl status tomcat.service
# 부팅시 자동 ㅅ작 활성화
sudo systemctl enable tomcat.service
```