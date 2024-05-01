---
tags:
  - VPN
  - Ubuntu
  - Network
---

# 필요 요소
* Ubuntu 22.04 서버
* 슈퍼 권한을 가지고 있는 계정
* VIM(선택사항; cat을 사용해도 됨)

# WireGuard 설치
```bash
sudo apt update
sudo apt install wireguard
```

# WireGuard 구성
* WireGuard의 인터페이스를 구성하고 관리 할 수 있는 wg, wg-quick이라는 두가지 명령 도구를 함께 제공한다.
* 다음 명령을 통해 공용 및 개인키를 생성한다.
```bash
wg genkey | sudo tee /etc/wireguard/privatekey | wg pubkey | sudo tee /etc/wireguard/publickey
```

* 생성된 키는 /etc/wireguard 디렉토리에 생성된다.
* cat, vim 등과 같은 편집기를 통해 해당 데이터를 읽을 수 있지만, 개인키는 절대 누구와도 공유되어서는 안되다는 것을 명심해야한다.

* wg0.conf라는 새 파일을 생성하고 아래 내용을 추가하도록 하자.
	* 파일 이름은 마음대로 지정할 수 있다.
* 인터페이스의 옵션
	* Address : IPv4 or IPv6 주소 목록
	* SaveConfig : 인터페이스의 현재 상태를 구성 파일에 저장할지 여부
	* ListenPort : WireGuard가 들어오는 연결 포트
	* PrivateKey : wg gwnkey를 통해 생성된 개인 키(sudo vi /ect/wireguard/privatekey로 확인)
	* PostUp : 인터페이스를 시작하기 전에 실행되는 명령 or 스크립트
		* 아래 내용에서는 iptables를 사용하여 위장을 활성화한다.
	* PostDown : 인터페이스를 중단하기 전에 실행되는 명령 or 스크립트
		* 인터페이스 종료시 iptables 규칙이 제가된다.
* `NETWORK_INTERFACE_NAME` : ip -o -4 route show to default | awk '{print $5}'를 통해 확인
```bash
sudo vi /etc/wireguard/wg0.conf
```
```bash
[Interface]
Address = 10.0.0.1/24
SaveConfig = true
ListenPort = 51820
PrivateKey = ${SERVER_PRIVATE_KEY}
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o ${NETWORK_INTERFACE_NAME} -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o ${NETWORK_INTERFACE_NAME} -j MASQUERADE
```

* 일반 사용자는 wg0.conf 및 개인 키 파일을 읽을 수 없다. 따라서, 권한을 부여한다.
```bash
sudo chomd 600 /etc/wireguard/{private, wg0.conf}
```

* 완료되면 구성 파일에 지정된 속성을 사용하여 wg0 인터페이스를 불러온다.
```bash
sudo wg-quick up wg0

# 잘올라왔는지 상태 및 구성 확인
sudo wg show wg0

# 인터페이스 상태 확인
ip a show wg0
```

* 부팅 시 WireGuard 인터페이스를 자동으로 가져오게 명령을 실행하자.
```bash
sudo systemctl enable wg-quick@wg0
```

# 네트워크 및 방화벽 설정
* NAT이 작동하도록 IP 전달을 활성화한다.
* `/etc/sysctl.conf` 파일을 열어 아래 내용을 추가하자.
```bash
net.ipv4.ip_forward=1

# 위 내용 입력 후 저장하고 변경 내용을 적용
sudo sysctl -p
```

* 방화벽에 WireGuard에서 사용할 포트(UDP)를 열어야 한다.
```bash
sudo ufw allow 51820/udp
```

### 여기까지가 서버에서 WireGuard 설정 완료.
---
# WireGuard Application을 이용한 접속
* PublicKey : Ubuntu 서버의 공개 키(/etc/wireguard/공개키파일)
* Endpoint : Ununtu 서버의 IP와 WireGuard Port(51820)
* AllowedIPs : WireGuard를 통한 접속을 허용할 IP 설정
```bash
[Interface]
privateKey = ${CLIENT_PRIVAT_KEY}
Address = 10.0.0.2/24

[Peer]
Publickey = ${SERVER_PUBLIC_KEY}
Endpoint = ${SERVER_IP_ADDRESS}:51820
AllowedIPs = 0.0.0.0/0
```

### 여기까지가 클라이언트에서 WireGuard를 통한 접속 환경 설정 완료.
---
# Server에서 허용해주기
* 서버에 클라이언트의 Peer를 추가해준다.
```bash
sudo wg set wg0 peer ${CLIENT_PUBLIC_KEY} allowed-ips 10.0.0.2
```

### 끝!

---
# 참고사항
* WireGuard 올리기
	* 예시 Interface : wg0
```bash
sudo wg up wg0
```
* WireGuard 내리기
	* 예시 Interface : wg0
```bash
sudo wg down wg0
```