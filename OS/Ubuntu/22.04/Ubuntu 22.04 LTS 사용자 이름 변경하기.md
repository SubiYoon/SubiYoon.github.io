---
tags:
  - Ubuntu
  - Linux
---
* CLI로 우분투 설치시 대문자로 유저 이름을 만들지 못해 찾아보게 되었다. 그럼 시작해보자.

---

### 1. 우선 이름을 변경에 사용할 임시 사용자를 만들고 sudo 권한 부여해야 한다.

```bash
# 새로운 임시 사용자 이름 생성
sudo adduser {tmp username}

# 새로 만든 임시 사용자 sudo 권한 부여
sudo adduser {tmp username} sudo
```

* 진행 중인 프로세스가 존재하면 안된다. 생각해보면 ssh연결이 되어 있는 상태라는건???
* 현재 작동하고 있는 프로세스가 존재하는 것을 의미한다.
* 임시 사용자로 로그인하고 다음을 진행하기로 하자.

### 2. 기존 사용하던 사용자 이름으로 진행 중인 모든 process 죽이기
* 프로세스를 죽이지 않으면 진행이 안될 수 있다.
> usermod : user {기존 사용자 이름} is currently used by process 16555

```
sudo pkill -u {기존 사용자 이름} pid

sudo pkill -9 -u {기존 사용자 이름}
```

### 4. 사용자 이름 변경 및 변경된 사용자 이름으로 홈디렉토리 변경 

```
sudo usermod -l {변경하고자 하는 사용자 이름} {기존 사용자 이름}

sudo usermod -d /home/{변경하고자 하는 사용자 이름} -m {변경하고자 하는 사용자 이름}
```

### 5. groupname 변경

```
sudo groupmod -n {변경하고자 하는 사용자 이름} {기존 사용자 이름}

# 확인하기
id {변경한 사용자 이름}
ls -ld /home/{변경한 사용자 이름}
```

### 6. 접속 끊고 **변경된 사용자 이름으로 로그인**

* kill process!!!!
* 이유는 말안해도 알 것이라고 생각하고 생략한다.

```
sudo pkill -u {임시 사용자 이름} pid

sudo pkill -9 -u {임시 사용자 이름}
```

### 7. 임시 사용자 계정 및 디렉토리 삭제

```
sudo deluser {임시 사용자 이름}

sudo rm -r /home/{임시 사용자 이름}
```