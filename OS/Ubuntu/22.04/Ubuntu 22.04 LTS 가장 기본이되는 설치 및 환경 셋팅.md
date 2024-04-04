---
tags:
  - Ubuntu
  - Linux
---
# 기본 OS 설치
* 해당 글은 SSH, docker를 OS설치시 설치했다는 것을 가정한다.

---
# VIM 설치
```bash
# 우분투를 설치하면 Mac의 homebrew, 윈도우의 chocolate 같은 역할을 하는 녀석이 설치된다.
# 그녀석을 업데이트 시켜주자
sudo apt update -y
sudo apt-get update -y

# 우분투를 minimize하게 설치하지 않았으면 vim-tiny를 제거해주자.
# 제거하지 않아도 되지만, vim을 사용하는 것이 syntax hilight가 되기 때문에 더욱 좋다.
sudo apt remove vim-tiny
sudo apt install vim
```

---
# NET-TOOLS 설치
```bash
# 기본적으로 설치하면 ifconfig를 사용할 수 없다. net-tools를 설치해 해결하도록 하자.
sudo apt install net-tools
```

---
# OPENVSWITCH-SWITCH
```bash
# 고정 IP사용시 문제가 될 수 있는 녀석을 미리 설치해주자.
sudo apt install openvswitch-switch
```

---
# root 계정 비밀번호 생성
```bash
sudo passwd root
```

---
# 시간대 변경(한국-서울)
```bash
sudo timedatectl set-timezone Asia/Seoul
```


---
# 기타 있으면 유용한 것들
```bash
# 시스템사항을 간략히 볼 수 있다.
sudo apt install neofetch

# htop의 UI가 맘에 안든다면 설치
sudo apt install bpytop
```