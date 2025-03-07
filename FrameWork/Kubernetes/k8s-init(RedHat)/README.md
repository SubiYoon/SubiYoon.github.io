---
tags:
  - FrameWork
  - Kubernetes
  - RedHat
  - TrubleShooting
---
# 🚀TrubleShooing
### openssl mismatch
- 버전의 불일치로 ssh 접속시 22번포트의 접속이 불가했다.
- 재설치를 한 후에 정상 동작하는 것을 확인

```bash
yum update openssl openssh-server -y
```