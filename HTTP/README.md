# HTTP

**Hypertext Transfer Protocol**

- 텍스트 기반의 통신 규약으로 인터넷에서 데이터를 주고 받을 수 있는 프로토콜
- **클라이언트 - 서버 모델**을 따른다.
- 애플리케이션 레벨의 프로토콜로 **TCP/IP위에서 작동**한다.

---

# 1. 클라이언트 - 서버 구조

![스크린샷 2023-06-15 오전 9 33 14](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/9cadb636-88b4-4322-986b-4a1fd8ba96db)

요청(Request) → 브라우저인 클라이언트에 의해 전송되는 메시지

응답(Response) → 서버에서 응답으로 클라이언트에게 전송되는 메시지

**TCP/IP**

- 패킷 통신 방식의 인터넷 프로토콜인 IP(인터넷 프로토콜) + 전송 조절 프로토콜인 TCP(전송 제어 프로토콜)
- **IP → 패킷 전달 여부를 보증하지 않고, 패킷을 보낸 순서와 받는 순서가 다를 수 있다.**
- **TCP → IP 위에서 동작하는 프로토콜, 데이터의 전달을 보증하고 보낸 순서대로 받게 해준다.**
- HTTP, FTP, SMTP 등 TCP를 기반으로 한 많은 수의 애플리케이션 프로토콜들이 IP 위에서 동작하기
때문에 묶어서 TCP/IP라고 부르기도 한다.

---

# 2. 비연결성(Connectionless)

![스크린샷 2023-06-15 오전 9 33 43](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/88234e77-9303-4ac6-88f6-9a8797f35372)

클라이언트와 서버가 한번 연결을 맺은 후, 클라이언트 요청에 대한 서버의 응답이 끝나면 연결을 끊는 성질

**TCP 3 way Hand shake & 4 way Hand shake**

![스크린샷 2023-06-15 오전 9 34 12](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/4c1772e2-6e52-4b20-9e3b-ff8c0f5ef957)

| 종류 | 설명 |
| --- | --- |
| SYN
(Synchronization, 연결 요청 플래그) | TCP 교환 과정의 첫번째 패스, 세션 연결을 위해 임의적 시퀀스 번호를 보냄. |
| ACK(Acknowledgement, 응답) | 첫번째 패킷을 받았다는 걸 알려주는 패킷, 성공/실패 판단하여 다음 패킷을 전송. |
| RST(Reset, 재 연결 종료) | 비 정상적인 세션 연결 끊기, 즉시 연결 끊는 행위. |
| PSH(Push) | OSI 7계층인 Application 계층으로 전송하는 Flag |
| URG(Urgent, 긴급 데이터) | 유효한 것인지 나타낸다. 
(예를 들면 명령 수행 도중 Ctrl+C를 눌러 인터럽트 하는 행위) |
| FIN(Finish, 연결 종료 요청) | 세션 종료 시킬 때 사용. |

**3 way-handshake(두 지점 간에 세션을 연결하는 작업)**

- 클라이언트는 서버와 커넥션을 연결하기 위해 SYN을 보낸다.
- 서버가 SYN을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보낸다.
- 클라이언트는 서버의 응답인 ACK와 SYN 패킷을 받고, ACK를 서버로 보낸다.

**4 way-handshake(두 지점 간에 세션을 종료하는 작업)**

- 클라이언트가 연결을 종료하겠다는 FIN 플래그를 서버에 전송한다.
- 서버는 일단 확인 메세지인 ACK를 보내고 자신의 통신이 끝날때가지 기다린다.(Time_Wait상태)
- 서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN 플래그를 전송한다.
- 클라이언트는 확인 메세지인 ACK를 다시 서버에 보냄

---

# 3. 무상태(StateLess) 프로토콜

**상태유지**

- 상태를 유지하고 있으면 해당 서버가 해당 유저를 기억하고 있기에 그 서버만 응답해야 한다.
- 만약 특정서버가 장애가 발생한다면, 유저 상태가 사라지기 때문에 처음부터 다시 서버에 요청해야 한다.
![image](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/7389dbd4-2ff6-4465-b043-f1c3e68465ce)

**무상태**

- 상태가 유지되지 않기 때문에 아무 서버에서나 호출이 가능 해진다.
- 서버에 장애가 생기더라도 다른서버에서 응답하여 전달 할 수 있다.
- 즉, 응답 서버를 쉽게 바꿀 수 있기에 수평적인 확장에 유리하다.
![image 1](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/68312210-e5f1-4221-86c5-953ba9e00177)

---

# HTTP 역사(큰 틀로 간단히 정리)

### 버전 작성시 유의 사항

- 버전 번호는 분수로 다루어지지 않는다.
- 버전의 각 숫자는 각각 분리된 숫자로 비교한다.
    - 때문에 각 숫자는 반드시 따로따로 비교해야 한다.
    - ex) 1.0 은 1 과 0으로 나누어 진다.
    - ex) 2.22와 2.3을 비교시 22와 3으로 비교하기에 22가 더 높은 버전으로 간주한다.

**HTTP 0.9**

- HTTP가 발전하기 전에 버전이 명시되어 있지 않아 이전 버전을 0.9로 칭함
- 간단히 GET 메서드만 존재했다.

**HTTP 1.0**

```html
GET /mypage.html HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)

200 OK
Date: Tue, 15 Nov 1994 08:12:31 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/html    <!-- 리소스의 media type을 나타내기 위해 사용 -->
<HTML>
A page with an image
  <IMG SRC="/myimage.gif">
</HTML>
```

- 여러가지 특징들이 추가되어 좀더 융통성 있게 확장 되었다.
- 상태 코드 라인 또한 응답의 시작 부분에 붙어 전송되어 request에 대한 성공/실패 여부를 알게 되었다.
- HTTP 헤더 개념은 요청과 응답 모두를 위해 도입되어, 메타데이터 전송을 허용하고 프로토콜을 극도로 유연하고 확장 가능하도록 만들어 주었다.
- 새로운 HTTP 헤더의 도움으로, HTML 파일들 외에 다른 문서들을 전송하는 기능이 추가되었다.

**HTTP 1.1**

- **용어사전**
    
    **오버헤드(Overhead)**
    
    - TCP의 다양한 기능을 수행하기 위해서는 TCP헤더에 다양한 정보가 필요한데, 데이터를 전송할 때에 꼭 필요하지 않은 처리나 정보 등을 일컫는 말
![image 2](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/66ceac65-b99f-4a53-84a1-f0a6dafdddb2)

- 컨텐츠의 수가 증가하면서 TCP의 연결에 부담이 걸려 그 문제를 해결해야 하는 필요성이 생겨 발달했다.
- 위 그림에서 초창기에는 요청마다 왼쪽과 같이 TCP를 새로 연결시켜 줘야 했다.
- 때문에 1.1버전에서는 지속연결(presistent Connection)을 통해 여러개의 컨텐츠를 요청할 수 있게 되었다.
- 1.0에서는 **`Connection: keep-alive`** 을 통해 연결을 유지 했지만, 1.1부터는 모든 요청/응답이 Connection을 재사용하도록 설계 되었고 **`Connection: close`** 를 통해 명시적으로 끊어주여야 했다.

**파이프라이닝**

- 하나의 연결에서 한번에 순차적인 여러 요청을 연속적으로 하고 서버는 그 순서에 맞게 응답하는 것
- 다수의 Request와 Response를 처리할 수 있게 끔 Network Latency를 줄일 수 있다.
- But!! 응답 처리를 미루는 방식이기에 응답에대한 처리는 순차적으로 진행되야하고 후순위는 지연된다.
    - 이러한 현상을 HOL(Head Of Line) Blocking 이라고 한다.
        - 클라이언트에서 요청을 여러 개 동시에 보내면 서버의 응답 Queue에 쌓인다.
        - 서버는 받은 요청을 처리한다.
        - 응답 Queue에서 데이터 전송의 한계 발생
        - 선 요청 받은 건을 다 전송하지 못하면 후전송 건도 늦어진다.
        - 서버의 버퍼에는 후전송 건이 쌓여 리소스가 소모된다.

![image 3](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/fc833600-fdd1-4a56-8ae2-052c8df2d535)
![image 4](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/47b5868f-b55e-4964-ba0b-351ba1405654)
        
        - **RTT(Round Trip Time)** 증가와 네트워크 지연을 초래하여 성능이 저하되는 문제점이 발생한다.
            - 패킷망(인터넷) 위에서 패킷을 보내고자 하는 측에서 패킷을 목적지에 보낼 때, 패킷이 목적지에 도달하고 나서 해당 패킷에 대한 응답이 출발지로 다시 돌아오기가지의 시간
            - 네트워크 성능 측정 시, 네트워크 연결의 속도와 안정성을 진단할 때 사용된다.
            - **파이프라이닝**은 매 요청별로 connection을 만들고 3-way-handshake가 반복적으로 일어나 RTT를 증가 시킨다.
- 현재는 산발적으로 존재하는 우려점 때문에 요즈음의 브라우저들은 해당 기능을 막고 있다.

**도메인 샤딩 Domain Sharding(deprecated)**

- 파이프라이닝을 개선하기 위해 브라우저들이 connection을 생성하여 병렬로 요청을 보내는 것
- 브라우저별로 Domain당 Connection 개수의 제한이 존재한다.
- 현재는 사용하지 않는다.

**호스트헤더 (Host Header)**

- 호스트 헤더의 등장으로 동일한 IP주소에 다른 도메인을 호스트하는 기능이 가능해졌다.

```
GET /en-US/docs/Glossary/Simple_header HTTP/1.1
ACCEPT: */*
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101
```

- 호스트 헤더가 없던 HTTP/1.0 버전에는 위의 요청이 서버로 바로 요청이 된다.
- 웹서버는 위의 정보로는 어떤 홈페이지에 접속했는지 알 수 없기 때문에 IP주소로만 구분이 가능하다.
- 즉, 이전에는 도메인마다 IP를 준비해야 했고, 그만큼 서버의 개수도 늘어났다.

```
GET /en-US/docs/Glossary/Simple_header HTTP/1.1
ACCEPT: */*
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101
```

- HTTP/1.1의 등장으로 IP주소 대신 Host의 헤더값으로 구별이 가능해 졌다.
- 또한, 그로인해 하나의 웹서버에 여러개의 애플리케이션을 구동할 수 있게 되었다.

**시험용 프로토콜 스피디(SPDY)**

- google에서 개발한 시험용 프로토콜
- HTTP/1.1에 비해 상당한 성능 향상과 효율성을 보여줌
- 차 후 HTTP/2.0 초안의 참고규격이 되었다.

![image 5](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/5fcbb117-1f30-4666-8e65-aba8c9386934)

**HTTP 2.0**

- **주요목표**
    - 전체 요청을 통해 지연 시간을 줄이기
    - 응답 다중화를 지원
    - HTTP헤더 필드의 효율적 압축을 통해 프로토콜 오버헤드를 최소화
    - 요청 우선순위 지정을 추가하며, 서버 푸시를 지원
    - 보안 강화

- 주요 목표를 이루기 위해 다양한 보조 프로토콜을 개선
- HTTP/2.0은 HTTP의 애플리케이션 의미 체계를 어떤 식으로도 수정하지 않는다.
- 모든 핵심 개념(ex. HTTP메서드, 상태코드, URI 및 헤드 필드)은 그대로 유지된다.
- But!! 클라이언트와 서버 간에 데이터 서식(프레임)이 지정되는 방식과 데이터가 전송되는 방식을 수정
- 클라이언트와 서버는 전체 프로세스를 관리하며 애플리케이션의 모든 복잡성을 바이너리 프레이밍계층 내에 숨긴다.

**바이너리 프레이밍 계층(Binary Framework)**

- **전송단위**
    - Frame : HTTP/2.0에서 통신의 최소 단위, Header 또는 Data
    - Message : HTTP/1.1과 똑같은 오청 또는 응답으 ㅣ최소 단위, 다수의 프레임으로 이루어져 있다.
    - Stream : 클라이언트와 서버가 맺은 연결을 통해 양방향으로 주고받는 하나 또는 여러개의 메시지들
    - 여러개의 프레임이 하나의 메시지를 이루고, 그 메시지들이 모여 하나의 스트림을 만든다.
    
    ![image 6](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/aa136dbb-c5e5-47f7-ae0e-423bf020ae08)
    
![image 7](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/27defa87-a34d-4763-a1b5-444b3963922a)

- 이전 버전의 HTTP에서는 메시지(Message)를 전송하는 것이 기본 단위였다.
    - 메시지의 구조를 이해하기 쉽지만, 메시지의 크기가 커지면, 메시지의 구조 파악이 어려워지며, 메시지를 처리하는데도 많은 오버헤드가 발생하게 된다.
    - 메시지의 크기가 클 경우 TCP의 패킷 재전송 때문에 전송 속도가 느려지는 문제가 발생하기도 한다.
- 바이너리 프레이밍 계층에서는 이와 같은 단점을 보완하기 위해, 프레임이라는 단위로 데이터를 나누어 전송
- 프레임이라는 작은 단위로 나누어 전송하므로, 오버헤드를 최소화할 수 있다.
- 프레임의 순서를 조합하여 메시지를 완성하므로, 메시지의 구조 파악이 쉽다.

**멀티 플렉싱**

![image 8](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/b0aacb4e-8dd6-47b2-983d-e58d2183ffb6)
![image 9](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/d1a02bf0-8de7-4b8d-b7ae-21aca41aaf82)

- 하나의 커넥션으로 동시에 여러개의 메시지를 주고 받을 수 있으며, 응답은 순서에 상관없이 처리되는 것
- 요청, 응답은 뒤섞여 도착하게 되고, 뒤 섞인것이 도착한 곳(서버 or 클라이언트)에서 프레임을 재조립한다.
- 바이너리 프레이밍과 멀티플렉싱을 사용하여 여러 요청과 응답을 병렬처리 할 수 있다.

**Stream 우선순위 (Stream Prioritization)**
<img width="1371" alt="image 10" src="https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/0656bc72-4c8c-4948-be68-da4910467d91">

- HTTP 메시지가 많은 개별 프레임으로 분할될 수 있고 여러 스트림의 프레임을 멀티플렉싱 할 수 있게 되면서, 스트림들의 우선순위를 지정할 필요가 발생했다.
- 우선순위를 지정하기 위해 ‘우선순위 지정 트리’를 사용하여 서버의 스트림처리 우선순위를 지정 할 수 있다.
- 서버는 우선순위가 높은 응답이 클라이언트에 우선적으로 전달도리 수 있도록 대역폭을 설정한다.

**헤더 압축**

- **원리**
    
![image 11](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/4f86ccc3-3c1d-49b9-af6e-4392f558e7c7)

![image 12](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/84a3716f-cfab-4128-98e7-e483fa4832a5)

    - HTTP/2.0의 Header 압축은 내부적으로 HPACK ****이라고 불리는 Module이 담당하는데 HPACK은 Huffman Algorithm과 Static Table, Dynamic Table을 통해서 압축을 수행한다.
    - Huffman Algorithm : 자주 나오는 문자열 순서대로 짧은 Bitmap으로 Mapping하여 Data를 압축하는 기법
    - Static Table : HTTP/2.0 Spec에 정의된 Table로 HTTP/2.0 Header로 자주 사용되는 Key-value 값 쌍을 저장하고 있는 Table
    - Dynamic Table : 한번 전송/수신한 Header의 Key-value 값을 임의로 저장하는 Buffer 역할을 수행하는 Table
    - Static Header Table : 모든 연결에 사용될 가능성이 있는 공용 HTTP 헤더 필드를 제공
    (ex.올바른 헤더 이름)
    - Dynamic Header Table : 처음에는 비어있으며, 특정 연결에서 교환되는 값에 따라 업데이트 된다.

- 클라이언트가 두번의 요청을 보낼 때 HTTP/1.x는 두개의 요청 Header에 중복값이 존재해도 중복 전송한다.
- HTTP/2.0은 중복값이 존재하는 경우 Static/Dynamic Header Table 개념을 사용하여 중복을 검출한다.
- Header는 index값만 전송한다.
- 중복되지 않은 Header정보의 값은 Huffman Encoding 기법으로 인코딩 처리하여 전송한다.

**서버 푸쉬**

![image 13](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/56a8d236-c977-4d69-8604-22e93490d134)

- HTTP/2.0 이전 버전은 HTML을 요청 후 응답받게 되는데, 그 후 내장되어 있는 JavaScript나 CSS파일 등을 추가로 요청하는 작업이 필요하다.
- HTTP/2.0 이후 버전은 HTML을 요청 후 JavaScript, CSS 등이 존재할 경우 요청하지 않은 컨텐츠까지 같이 보내 줄 수 있다. 이로인해 전송에 필요한 RTT가 줄어 응답속도를 개선시켰다.

---

# URL escape String

| Character | URL Escape Codes | String Literal Escape Code |
| --- | --- | --- |
| SPACE | %20 | $20 |
| < | %3C | $3C |
| > | %3E | $3E |
| # | %23 | $23 |
| % | %25 | $25 |
| + | %2B | $2B |
| { | %7B | $7B |
| } | %7D | $7D |
| \| | %7C | $7C |
| \ | %5C | $5C |
| ^ | %5E | $5E |
| ~ | %7E | $7E |
| [ | %5B | $5B |
| ] | %5D | $5D |
| ‘ | %60 | $60 |
| ; | %3B | $3B |
| / | %2F | $2F |
| ? | %3F | $3F |
| : | %3A | $3A |
| @ | %40 | $40 |
| = | %3D | $3D |
| & | %26 | $26 |
| $ | %24 | $24 |

---

# Types of Schme

| Schme | 설명 | 사용법 |
| --- | --- | --- |
| http | - 일반 기본 HTTP프로토콜 기본포트 : 80 | http://<호스트>:<포트>/<경로>?<질의>#<프래그먼트> <br>ex)http://www.naver.com/index.html |
| https | - http와 거의 비슷 <br>- 양끝단에서 암호화하기위해 넷스케이프에서 개발한 보안 소켓 계층을 사용 <br> - 기본포트 : 443 | http://<호스트>:<포트>/<경로>?<질의>#<프래그먼트> <br> ex)https://www.naver.com/index.html |
| mailto | - mailto URL은 이메일을 가르킴 <br> - 표준 URL과는 다른 포맷을 가짐 <br> - RFC 822에 문법이 기술되어 있음 | mailto:\<RFC-822-addr-spec> <br> ex)mailto:joe@joe-hardware.com |
| ftp | - 파일 전송 프로토콜 <br> - File Transfer Protocol <br> - URL이 등장하기 전부터 존재함 <br>- ftp서버의 디렉토리에 있는 컨텐츠 목록을 가져오는데 사용 할 수 있음 <br>- ; (세미콜론)을 사용하여 파라미터 명시하면 파일을 가져옴 | ftrp://<사용자이름>:<비밀번호>@<호스트>:<포트>/<경로>;<파라미터> <br> ex)ftp://annoymous:pwd%40naver.com@prep.ai.mit.edu:21/pub/gnu/ |
| rtsp / rtspu | - 실시간 스트리밍 프로토콜 <br> - Real Time Streaming Protocol <br> - utspu에서 u는 UDP 사용을 의미 | (rtsp or rtspu)://<사용자이름>:<비밀번호>@<호스트>:<포트>/<경로> <br> ex)rtsp://www.naver.com:544:/interview/cto_video |
| file | - 주어진 호스트 기기에서 바로 접근 가능한 파일들을 나타냄 <Br> - 각 필드도 일반적인 URL포맷을 따름 <br> - 호스트 생략시 URL을 사용중인 로컬 호스트가 기본값이 됨 | file://<호스트>/<경로> <br> ex)file://OFFICE-FS/policies/casual-fridays.doc |
| news | - 특정 문서나 뉴스 그룹에 접근시 사용 <br> - RFC 1036에 정의되어 있음 | news:\<newsgroup> <br> news:\<news-article-id> <br> ex)news:red.arts.startrek |
| talent | - 대화형 서비스에 접근시 사용 <br> - 객체를 가리지 않음 <br> - 리소스라고 할 수 있는 대화형 appliction은 이 프로토콜을 통해 접근가능 | talent://<사용자이름>:<비밀번호>@<호스트>:<포트>/ <br> ex)talent://user:passowrd@naver.com:30/ |

---

# PURL(Persistent Uniform Resource Locators)

- URL로 **URN**기능을 제공하게 해준다.
    - **URN**
        
        → 객체가 옮겨지더라도 항상 객체를 가리킬 수 있는 이름을 제공해준다.
        

---

# 시작줄 Method

- 모든 메서드는 그 뜻을 정의할 뿐 기능은 내가 만들어야한다.

| Method | Explain | isBody? |
| --- | --- | --- |
| GET | 서버에서 어떤 문서를 가져온다. | false |
| HEAD | 서버에서 어떤 문서에 대해 헤더만 가져온다. | false |
| POST | 서버가 처리해야 할 데이터를 보낸다. | true |
| PUT | 서버에 요청 메시지의 본문을 저장한다. | true |
| TRACE | 메시지가 프락시를 거쳐 서버에 도달하는 과정을 추적한다. | false |
| OPTIONS | 서버가 어떤 메서드를 수행할 수 있는지 확인한다. | false |
| DELETE | 서버에서 문서를 제거한다. | false |
| LOCK | 사용자가 리소스를 잠글 수 있게 해준다.
ex) 문서를 편집하는 동안 다른 사용자가 동시에 편집불가하게 처리 |  |
| MKCOL | 사용자가 문서를 생성할 수 있게 해준다. |  |
| COPY | 서버에 있는 리소스를 복사한다. |  |
| MOVE | 서버에 있는 리소스를 옮긴다. |  |

---

# 상태코드 Status Code

| 상태 코드 | 설명 |
| --- | --- |
| 1xx(정보) | 요청을 받았으며 프로세스를 계속 진행한다. |
| 2xx(성공) | 요청을 성공적으로 받았으며 인식했고 수용하였다. |
| 3xx(리다이렉션) | 요청 완료를 위해 추가 작업 조치가 필요하다. |
| 4xx(클라이언트 오류) | 요청의 문법이 잘못되었거나 요청을 처리할 수 없다. |
| 5xx(서버 오류) | 서버가 명백히 유효한 요청에 대한 충족을 실패했다. |
- 200 OK
    
    → 성공!
    
- 401 Unauthorized
    
    → 사용자 이름과 비밀번호를 입력해야 함!
    
- 404 Not Found
    
    → 요청한 URL을 찾지 못함!
    

---

#
