---
tags:
  - Ubuntu
  - Linux
  - SAMBA
---

# 구성 요소
* Ubuntu 22.04
* sudo 권한이 있는 계정
* VIM(선택사항 cat을 사용해도 무관)

---
# SSH 설치
* Ubuntu에는 기본적으로 ssh client는 설치되어 있지만 server는 없기에 설치하기로 하자.
```bash
sudo apt install openssh-server
# 설치 상태를 확인해보자
# active라고 뜨고 초록불이 들어온다면 정상작동 중이다.
sudo systemctl status ssh.service
```

* `sshd_config`파일로 ssh의 설정을 변경하게 되는데 port나 sftp를 설정 할 수 있다.
```bash
sudo vi /etc/ssh/ssh_config
```
```bash
# 원하는 포트번호 설정
Port ${PORT_NUMBER}
# ssh사용 가능한 유저 설정
AllowUsers ${USER}
...
```

* sftp 설정을 해보자.
* `Subsystem sftp /usr/lib/openssh/sftp-server`라고 써있는 부분을 주석처리한다.
```bash
# Subsystem sftp /usr/lib/openssh/sftp-server

# 여러 사용자를 선택할 수 있고, 구분은 공백으로 구분한다.
AllowUsers [사용자] [사용자2]
Subsystem sftp internal-sftp -f AUTH -l VERBOSE
```

* 변경을 완료했다면, 재실행해서 적용을 시켜주자.
```bash
sudo systemctl restart ssh.service
```

---
# samba 설치
```bash
sudo apt install samba
```

* Ubuntu의 사용자와 별개로 samba사용자를 생성해 주어야 한다. 다음 명령어를 참고하여 유저를 관리하도록 하자.
```bash
smbpasswd -a ${user} # 사용자 추가
smbpasswd -x ${user} # 사용자 제거

smbpasswd [사용자명] # 사용자 비밀번호 변경

smbpasswd -d ${user} # 사용자 일시적 비활성화
smbpasswd -e ${user} # 사용자 활성화

# 설정파일에서 `null passwords = yes` 설정을 해야 비밀번호 없이 로그인 가능
smbpasswd -n ${user} # 비밀번호 없이 로그인 가능하게 설정

# root사용자는 다른 사용자도 설정이 가능하다.
sudo smbpasswd ${options} ${user} ${pwd}

# 등록된 사용자들 확인하는 방법
sudo pdbedit -L
sudo pdbedit -L -v # 좀더 상세한 정보
```

* `smb.conf` 파일을 통해 그룹, 예외 파일, 공유, 휴지통등을 설정 할 수 있다.
```bash
sudo vi /etc/samba/smb.conf
```

* 우선 쓸모없는 타 운영체제의 탐색기 임시파일을 예외 시키도록 하자.
* `[global]`밑에 아래 내용을 작성해주자.
	* MacOS : .DS_Store, .\_*
	* 윈도우 : Thumbs.db
```bash
delete veto files = Yes
veto files = /.DS_Store/._*/Thumbs.db
```

* 새로운 공유 주소를 추가하려면 아래와 같이 `[${PathName}]`안에 경로를 넣어주면 된다.
	* `smb://${원격지 주소}/${PathName}`
* 읽고 쓸 수 있게 공유폴더로 사용할 폴더에 쓰기/읽기 권한을 넣어주자
```bash
sudo chmod 1777 ${폴더명}
```

* 본격적으로 설정을 해보자.
```bash
[${PathName}]
comment = share data # 설명
path = /[${path}] # 공유할 디렉토리
browseable = no # 네트워크에서 보일지 여부
guest ok = no # 게스트 접근가능 여부
writeable = no # 다른 사용자 쓰기 권한 여부
valid users = ${user} # 해당 사용자 접근
read list = ${user} # 해당 사용자 읽기 허용
write list = ${user} # 해당 사용자 쓰기 허용
create mask = 0666 # SMB를 통해 생성된 파일 권한 644
directory mask = 0777 # SMB를 통해 생성된 디렉토리 권한 755
```

* 설정이 완료되면 재실행을 해주자.
```bash
sudo systemctl restart smbd
```

---
# 참고사항
* 만약 방화벽을 설정 중이라면 방화벅의 포트를 뚫어주어야 한다.
* samba의 포트는 139, 445를 사용하니 해당 포트는 다른 용도로 사용하지 말아야한다.
```bash
sudo ufw allow 139
sudo ufw allow 445
```

* `scp`를 사용하고 싶다면 다음 명령어를 사용하면 된다.
	* options
		* -r : 폴더 복사
		* -a : 아카이브 모드
		* -p : 권한 유지
		* -c : 압축 복사
		* -P : 포트번호
```bash
# 로컬에서 원격으로 보낼 때
scp [옵션] [파일 위치] [유저 이름]@[원격주소]:[보낼 위치]

# 원격에서 로컬로 받을 때
scp [옵션] [유저 이름]@[원격주소]:[파일 위치] [받을 위치]
```
