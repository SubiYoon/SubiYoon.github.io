---
tags:
  - FrameWork
  - Kuberntes
---
# 사전 준비
* Virtual Machine Program
* Ubuntu 22.04.x server(.iso)

# Ubuntu 기본 설정
### Ubuntu 설치 후 설치해야 하는 것들
Ubuntu 22.04.x LTS를 설치하면 minimized로 설치하지 않는이상 Ubuntu에서 자동으로 설치해주는 툴들이 있다. 하지만, 설치가 되지 않는 것들도 존재하는데 이것들을 설치해주자.
1) vim
	* 어떠한 에디터를 사용하던 상관은 없지만, 이미 vim에 노예이기에 vim을 설치하자.
	* Ubuntu 22.04에서 기본적으로 vim-tiny가 설치되어 있다. 우리는 vim이랑 똑같이 동작하지만, 하이라이트 적용을 위해 새로운 vim을 설치하자.
```bash
sudo apt update

# vim-tiny 삭제
sudo apt remove vim-tiny

#vim 설치
sudo apt install vim
```
2) net-tools
	* 기본적으로 설치된 환경에서는 `ifconfig`조차 사용 할 수 없기에 설치해주자.
```bash
sudo apt install net-tools
```
3) openvswitch-switch
	* `netplan apply`를 사용하면 에러가 발생하는데 이를 해결하기 위해 설치하자.
```bash
sudo apt install openvswitch-switch
```

### 기타 설치하면 유용한 것들
꼭 설치할 필요는 없지만, 사용하면 용이한 것들이다. 참고하자.
1) neofetch
	* 시스템 정보를 나타내주는 녀석... 이쁘다...
```bash
sudo apt install neofetch
```
![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/bc54e308-b555-4004-9ba3-ac0d3c344f76/4fc03d5a-d189-4a71-88b9-1b4f77c428e8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240415%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240415T072330Z&X-Amz-Expires=3600&X-Amz-Signature=9096caac302786b7e9566e598ae426ab7571ca0a8fc3f6b2af16351b82a2bda2&X-Amz-SignedHeaders=host&x-id=GetObject)

2) bpytop
	* 답답한 htop의 UI를 이쁘게 표현해주는 녀석이다.
```bash
sudo apt install bpytop
```

![image](https://prod-files-secure.s3.us-west-2.amazonaws.com/bc54e308-b555-4004-9ba3-ac0d3c344f76/9eaa11d2-a347-4084-abc1-d001e19cad01/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240415%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240415T072330Z&X-Amz-Expires=3600&X-Amz-Signature=d99ad337f3d7fa778f9eb95370489ba62b7f2da659781faee0488f5685aa6412&X-Amz-SignedHeaders=host&x-id=GetObject)


# 네트워크 환경 구성
### 고정 IP 설정
IP를 고정하려면 게이트웨이의 IP를 미리 확인해 두어야한다.
```bash
# sudo route -n

0.0.0.0 ${gateway ip} ...
```


```yaml
# sudo vi /etc/netplan/00-installer-config.yaml

# 해당하는 이더넷 설정을 변경하면 된다.
network:
  ethernets:
    # ifconfig로 이더넷포트 이름을 확인 후 알맞은 곳을 수정해야한다.
    enp0: false
    addresses:
      - ${gateway ip 앞 세자리}.104/24  # hosts에서 설정한 master의 ip
	routes:
	  - to: default
	    via: ${gateway ip}
	nameservers:
	  addresses: [${gateway ip 앞 세자리}.101, 8.8.8.8, 8.8.4.4]
  version: 2
```

이제 고정 ip가 정상적으로 작동을 하는지 적용 후 테스트를 해보아야 한다.
아래 문구를 입력 후 정상 적동한다면 netplan 설정엔 오류가 없다는 것이다.
```bash
sudo netplan apply
```

테스트를 해보자.
가만히 두면 계속 올라오니  `control + c`로 중간에 멈춰야 한다.
아래와 같이 나온다면 정상적으로 네트워크가 동작한다는 것을 의미한다.
```bash
# ping 8.8.8.8

PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=111 time=45.9 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=111 time=46.4 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=111 time=54.7 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=111 time=47.3 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=111 time=48.4 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=111 time=46.6 ms
64 bytes from 8.8.8.8: icmp_seq=7 ttl=111 time=47.8 ms
64 bytes from 8.8.8.8: icmp_seq=8 ttl=111 time=47.5 ms
64 bytes from 8.8.8.8: icmp_seq=9 ttl=111 time=47.4 ms
^C
--- 8.8.8.8 ping statistics ---
9 packets transmitted, 9 received, 0% packet loss, time 8019ms
rtt min/avg/max/mdev = 45.949/48.016/54.694/2.465 ms
```

### hostname 설정
각각의 서버에 Domain을 대신할 hostname을 설정하자.
```bash
# sudo vi /etc/hostname

# 맨 첫줄에 내용이 있다면 삭제하고 다음을 입력 후 저장하고 빠져나오자.
master.example.com
```

### hosts 설정
```bash
#sudo vi/etc/hosts

127.0.0.1 localhost
127.0.0.1 ${server-name}

#위의 내용을 다음과 같이 변경하자
128.0.0.1 localhost
 ${gateway ip 앞 세자리}.104 master.example.com master
 ${gateway ip 앞 세자리}.101 node1.example.com node1
 ${gateway ip 앞 세자리}.102 node2.example.com ndoe2
```

### 재시작
재시작을 통해 해당 설정이 정상적으로 되었는지 확인해보자.
```bash
sudo reboot
```

로그인 화면에서 일부러 비밀번호를 틀리면 hostname이 적힌 화면이 나오게 된다.
아래와 같이 나온다면 성공!!
```bash
master login : 
Password:

Login incorrect
master.example.com login : 
```

아래 커맨드를 입력후 결과값이 같다면 정상 동작한다는 것을 의미한다.
```bash
# hosthame
master.example.com
```

# docker 설치


# 복제품 만들기
복제품을 만드는 이유는 Kubernetes를 사용하려면 여러개의 서버가 필요하기 때문이다.
우리는 앞에서 한 작업을 하나하나 다시 만들어주는 과정이 너무 귀찮아!!!!
복제품을 만들어서 몇가지만 수정하자.