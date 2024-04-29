---
tags:
  - Network
  - DNS
---
# CloudFalre란??
DNS를 제공해주는 사이트로써 유료로 사용하면 디테일한 모니터링과 다양한 설정을 할 수 있지만, 간단한 웹 사이트 운영시에는 무료버전만으로도 충분히 사용이 가능한 사이트이다.
Domain의 경우 가격이 천차만별인데 가격은... 음... 찾아보기 바란다. 어느정도의 모니터링과 설정을 할 수 있다는 점에서 비용이 과하다고는 생각하지 않는다.


cloudflare를 이용하면 와일드카드 발급, 발급시에 도메인 인증 등을 간편하게 할 수 있다. 이때 cloudflare 계정의 Global Api Key를 요구한다.

```bash
# /root에 .secrets 폴더를 생성
sudo mkdir /root/.secrets

# certbot-cloudflare.ini 파일 만듦 + 파일 수정
sudo vi /root/.secrets/certbot-cloudflare.ini
```

[https://dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens) 에서 Global Api Key를 볼 수 있다. certbot-cloudflare.ini파일 안에 정보에 맞게 수정하고 저장

```bash
dns_cloudflare_email = example@gmail.com
dns_cloudflare_api_key = abcdefg12345678
```

root만 접근 가능하게 권한 수정

```bash
sudo chmod 0700 /root/.secrets
sudo chmod 0400 /root/.secrets/certbot-cloudflare.ini
```

certbot, certbot cloudflare 플러그인 설치

```bash
sudo apt install certbot python3-certbot-dns-cloudflare
sudo apt install python3-certbot-dns-cloudflare
```

### 인증서 발급받기

> sudo certbot certonly --dns-cloudflare --preferred-challenges dns-01 --dns-cloudflare-propagation-seconds *20* --dns-cloudflare-credentials */root/.secrets/certbot-cloudflare.ini* -d *example.com* -d *\*.example.com*

20은 대기 시간인데 보통 20초면 됐었다. 도메인 이름도 변경하고 발급받자

이메일 설정, 약관 동의, 정보 수신 동의 등을 하면 머라머라 많이 나오는데 Congratulations!가 나오면 성공.

### 인증서 재발급

인증서는 90일 동안 유효하기 때문에 제때 재발급을 해줘야 하는데 우분투 17.10 버전 이상은 자동으로 재발급 봇이 작동하고 있어서 신경을 쓰지 않아도 된다.

```bash
# 재발급 봇 작동하는지 확인
sudo systemctl status certbot.timer
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/bc54e308-b555-4004-9ba3-ac0d3c344f76/1dae5d68-e93e-4b0a-b4cc-d2058a4d6188/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240417%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240417T140906Z&X-Amz-Expires=3600&X-Amz-Signature=4caebe128459b89446dd6e7543c90afac994bf1a4b309197e90ab4e771ad2481&X-Amz-SignedHeaders=host&x-id=GetObject)

그래도 재발급 테스트를 해보려면 `--dry-run` 을 붙여서 해주면 된다.

```bash
sudo certbot renew --dry-run
```

### certbot hook을 이용해 nginx 재실행

인증서가 갱신되면 새로 발급 받은 인증서를 nginx에도 재발급된 인증서를 제때 적용을 시켜줘야 갱신이 제대로 된다. bot의 hook기능이 있어서 이걸 이용하면 된다.

certbot-deploy-hook.sh 파일 생성, 수정


```bash
sudo vi /etc/letsencrypt/renewal-hooks/deploy/certbot-deploy-hook.sh
```

certbot-deploy-hook.sh 안에 아래 내용 추가하고 저장하자


```bah
#!/bin/bash

/bin/systemctl reload nginx
```

certbot-deploy-hook.sh에 실행권한 추가

```bash
sudo chmod u+x /etc/letsencrypt/renewal-hooks/deploy/certbot-deploy-hook.sh
```