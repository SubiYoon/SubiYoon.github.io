---
tags:
  - HTTP완벽가이드
---

# 9. HTTP Connection Management

# Connection Header

- 커넥션 헤더에는 다음 세가지 종류의 토큰을 전달 할 수 있다.
    - HTTP 헤더 필드명
        
        → 이 커넥션에만 해당되는 헤더들을 나열
        
    - 임시적인 토큰 값
        
        → 커넥션에 대한 비표준 옵션을 의미
        
    - close 값
        
        → 커넥션이 작업이 완료되면 종료되어야함을 의미
        
- **Connection Header의 있는 모든 헤더 필드는 메시지를 다른곳으로 전달하는 시점에 삭제되어야 한다.**

---

# 헤더 보호기

- Connenction Header는 홉별(hop-by-hop) 헤더명을 기술하는데 이것을 헤더 보호기라고 한다.
- Connection Header에 명시된 헤더들이 전달되는것을 방지
- Connection Header에 명시되지 않았더라도 홉별 헤더인 것들도 있다.
    - Proxy-Authenticate
    - Proxy-Connection
    - Transfer-Encoding
    - Upgrade

---

# 순차적 트랜잭션 처리에 의한 지연

![스크린샷 2023-06-22 오전 9 38 15](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/ab79f751-bca4-49df-b1d9-34bbbe0bc147)

- 각 트랜잭션이 새로운 커넥션을 필요로한다면 커넥션을 맺는데 발생하는 지연이 발생한다.
- 또한, 사용자는 한번에 모든 파일이 로드되기를 바란다. 때문에 특정 브라우저의 경우 객체를 화면에 배치하려면 크기를 알아야 하기 때문에 모든 객체를 내려 받기 전까지 텅 빈 화면을 보여준다.
- **HTTP 커넥션 성능 향상을 시켜줄 수 있는 기술**
    - 병렬(Parallel) 커넥션
    - 지속(persistent) 커넥션
    - 파이프라인(pipelined) 커넥션
    - 다중(multiplexed) 커넥션

---

# 병렬 커넥션

![스크린샷 2023-06-22 오전 9 38 38](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/35e5b2ef-fd2c-4c01-826d-bab1f2021965)

![스크린샷 2023-06-22 오전 9 38 52](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/219b123c-47fc-4bf5-b3e4-4a68de7891de)

- **병렬 커넥션은 페이지를 더 빠르게 내려받는다.**
- **병렬 커넥션이 항상 더 빠르지는 않다.**
    - 다수의 커넥션은 메모리를 많이 소모하고 자체적인 성능 문제를 야기 할 수 있다.
    - 서버는 특정 클라이언트로부터 과도한 수의 커넥션이 맺어졌을 경우 그것을 임의로 끊어버릴 수 있다.
- 병렬 커넥션은 더 빠르게 **“**느껴질 수**”** 있다.
    - 병렬 커넥션이 페이지를 항상 더 빠르게 로드 하지 않는다.
    - 하지만, 동시에 로드되기 때문에 더 빠르다고 느껴질 수 있다.
- 커넥션수의 제한이 있다.

---

# 지속 커넥션

![스크린샷 2023-06-22 오전 9 39 24](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/d4dd931a-6737-4e6a-ae9f-69067f0ccec2)

- **HTTP/1.1을 지원하는 기기는 처리가 완료된 후에도 TCP커넥션을 유지하여 앞으로 있을 요청에 재사용 가능.**
- **이 처리 완료 후에도 연결되어 있는 TCP커넥션을 지속 커넥션이라고 한다.**
- 지속 커넥션은 클라이언트나 서버가 커넥션을 끊기 전까지는 트랜잭션 간에도 커넥션을 유지한다.
- 커넥션을 하는 시간을 줄일 수 있고, 튜닝된 커넥션을 재사용 할 수 있으며, 커넥션수를 줄여준다.

### Keep-Alive

- 클라이언트에서 요청한 Connection: Keep-Alive를 서버에서 사용할 경우응답헤더에 
Connection: Keep-Alive 작성하여 보내주어야 작동한다.
- **Keep-Alive의 옵션을 사용하려면 Keep-Alive 헤더에 옵션을 작성해주어야 한다.**
    - , (쉼표)를 기준으로 사용한다.
    - max
        
        → 몇개의 트랜젝션까지 지속할 건지를 명시한다.
        
    - timeout
        
        → 미사용시 유지시간
        

```bash
Connecntion: Keep-Alive
Keep-Alive: max=5, timeout=120   #최대 5개의 트랜젝션, 2분동안 유지
```

- **멍청한 Proxy**로 인해 무한 로딩상태가 발생할 수 있다.
    - **멍청한 Proxy**
        
        → Connection: Keep-Alive를 이해하지 못하는 Proxy
        
    - Front에서 보낸 Connection: Keep-Alive를 멍청한 Proxy는 인지하지 못하고 확장헤더로 간주한다.
    - Connection: Keep-Alive는 Hop-by-Hop헤더 인데 확장헤더로 간주, 그대로 Server에 전송한다.
    - Connection: Keep-Alive를 받은 Server는 연결을 유지시키고 다시 멍청한 Proxy로 전달한다.
    - 멍창한 Proxy는 다시 Connection: Keep-Alive를 확장헤더로 인식하고 Front에게 보낸다.
    - 멍청한 Proxy는 Server가 커넥션을 끊어주기를 무한 대기하면서 무한 로딩상태가 된다.

### Proxy & Hop-by-Hop Header

- Connection 헤더뿐만 아니라 Keep-Alive란 헤더도 주의해야한다.
- 또한, Conncection 헤더 값으로 명시되지 않은 다른 Hop-by-Hop 헤더들도 주의해야 한다.
- Hop-by-Hop Header
    - Connection
    - Keep-Alive
    - Proxy-Authenticate
    - Proxy-Authorization
    - TE
    - Trailers
    - Transfer-Encoding
    - Upgrade

### Proxy-Connection Header

- 클라이언트의 요청이 중개서버를 통해 이어지는 경우 모든 헤더를 무조건 전달하는 문제를 해결.
- 멍청한 Proxy는 해당 해더를 확장헤더로 인식하여 송신시 Proxy-Connection Header로 보냄.
- **영리한 Proxy는 해당 헤더를 Connection 헤더로 변형하여 송신한다.**

### HTTP/1.1의 지속 커넥션

- **HTTP/1.1에서는 keep-alive 커넥션을 지원하지 않는대신, 설계가 더 개선된 지속 커넥션을 지원한다.**
- HTTP/1.1 애플리케이션은 트랜잭션이 끝난 다음 커넥션을 끊으려면 Connection: close를 명시해야 한다.
- **keep-alive 커넥션이 선택 사항이 아니며, Connection: close를 통해 언제든지 커넥션을 끊을 수 있다.**
- HTTP/1.1 Proxy는 클라이언트와 서버 각각에 대해 별도의 지속커넥션을 맺고 관리해야 한다.
- HTTP/1.1 Proxy서버는 클라이언트가 커넥션 관련 기능에 대한 지원 범위를 알고 있지 않은 한 지속 커넥션을 맺으면 안된다. 멍청한 Proxy가 Connection 헤더를 전달하는 문제가 발생할 수 있기 때문이다.

**참고사항**

- **청크 전송 인코딩(Chunked Transfer Eccoding)**
    - Content-Length를 표시할 필요가 없다. → 크기가 가변적인 데이터 전송에 유리
    - 청크데이터의 첫번째 body row는 16진수로 데이터의 크기를 나타내준다.
    - 한 청크당 보내지는 Length가 명시되어 있다.
    - 마지막 청크가 0을 수신하면 전송이 종료된다.
    - 스트리밍전송방식에서 주로 사용(?)
- **비멱등(nonidempotent)**
    - 연산을 여러번 수행하더라도 결과가 변하지 않는 멱등(idempotent)의 반댓말
        - GET처럼 단순 조회로 데이터의 변화가 없는 것들이 멱등에 해당
    - POST처럼 회원가입, 수정과 같은 데이터의 변화가 있는 것들이 비멱등에 해당

---

# 파이프라인 커넥션

![스크린샷 2023-06-22 오전 9 39 45](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/6f652654-d565-4d2c-95c0-847a72c22d89)

- 지속 커넥션을 통해 파이파이닝을 할 수 있다.
- **요청이 도착하면 응답이 도착하기 전에 다른 트랜잭션을 요청하는 방법**
- 요청은 Queue에 쌓여 대기하게 되는데 응답이 완료되기 전에 요청을 발생하여 대기시간을 줄일 수 있다.
- 비멱등 트랜잭션을 반복적으로 보내게되면 문제가 발생할 수 있는 단점이 있다.
    
    → 따라서 클라이언트는 POST와 같이 멱등이 아닌 트랜잭션을 파이프라인 커넥션으로 전송해서는 안된다.
