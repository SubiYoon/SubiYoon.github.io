---
tags:
  - Linux
  - Ubuntu
  - Nginx
---
## Nginx란??
트래픽이 많은 웹사이트의 서버(WAS)를 도와주는 비동기 이벤트 기반구조의 경량화 웹 서버이다. Nginx에는 다양한 기능이 있다. 기본적으로 정적 리소스에 대한 응답을 해주는 Web Server의 역할과 Reverse Proxy Server로 활용하여 WAS의 부하를 줄이는 로드밸런서 역할을 하기도 한다.

## Nginx 설치
### apt update하기
항상 명심해야 하는 apt update... 무언가를 설치하기 전에는 항상 해주기로 하자.
```bash
sudo apt update
sudo apt-get update
```

### Nginx 설치하기
```bash
sudo apt install nginx
```

### Nginx 실행하기
```bash
# Nginx 시작하기
sudo systemctl start nginx

# Nginx 상태 확인
sudo systemctl status nginx

# 부팅 시 Nginx 자동 실행
sudo systemctl enable nginx

# 변경사항 적용
sudo systemctl restart nginx

# Nginx 설정 변경 후 바로 적용시키지 않고 테스트하기
sudo nginx -t
```

## 방화벽 뚫어주기
UFW 방화벽을 실행중인 경우 Nginx의 프로필을 허용해야 한다.
뭐 대부분이 방화벽을 사용하니 우리도 한번 따라해 보도록 하자.
```bash
# 방화벽 활성화
sudo ufw enable

# 80포트와 443포트를 전부 허용
sudo ufw allow 'Nginx Full'
# 80포트 허용
sudo ufw allow 'Nginx HTTP'
# 442포트 허용
sudo ufw allow 'Nginx HTTPS'

# 방화벽 변경사항 적용(서비스를 재시작해도 됨)
sudo ufw reload

# 방화벽 상태 확인
sudo ufw status
```

## 참고할 파일들
* /etc/nginx/nginx.conf
	* 기본 구성 파일
	* 서버의 요구사항에 맞게 설정을 수정
* /etc/nginx/sys-available
	* 서버 블록 구성을 저장하는 디펙토리
* /etc/nginx/http
	* 디렉토리에는 이미 활성화된 사이트별 Nginx 서버 블록이 포함되어 있다.
* conf.d
	* sites-enabled와 같이 nginx에 반영되는 작업들이 있지만 비활성화 해야하는 경우 삭제하거나 변경해야 한다.
* sites-enabled
	* 설정 파일을 복사 또는 심볼릭 링크로 걸어 nginx에 반영하는 폴더
* sites-available
	* 설정을 저장하는 폴더 nginx에 반영하지 않고 sites-enabled에서 심볼릭을 걸어 사용
* /var/log/nginx/access.log
	* 웹 서버에 대한 접속 기록
* /var/log/nginx/error.log
	* Nginx에서 발생하는 모든 오류를 기록