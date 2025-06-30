---

---

# CORS(Cross Origin Resource Sharing)

- 브라우저의 보안과 관련하여 중요한 일들을 수행
- 브라우저가 자신이 보낸 요청 및 서버로부터 받은 응답의 데이터가 CORS정책을 지키는지 검사하여 안전한 요청을 보낸 건지 검사

**Origin**

- 프로토콜, 도메인, 포트번호를 합친 부분

ex) URL : `https://it-eldorado.com:80/posts/123456?data=789#abc`

 Origin : `https://it-eldorado.com:80`

**SOP(Same Origin Policy)**

- 동일 출처 정책
- 다른 Origin으로 요청을 보낼 수 없음

**CORS(Cross Origin Resource Sharing)**

- 교차 출처 자원 공유
- **‘브라우저’의 정책**
- 다른 Origin으로 요청하기위해 지켜야하는 정책
- SOP에 의해 막히게 될 요청을 풀어주는 정책

**자유롭게 다른 Origin으로 요청을 보낼 수 있다면?(CSRF공격)**

사용자가 A라는 웹사이트에 로그인되어 있어 브라우저 단에 인증 정보가 존재할 때, 해커가 만든 사이트에 실수로 접속하면 해커가 심어둔 JavaScript코드가 실행되면서 A웹사이트로 개인정보를 조회하는 API요청을 보낼 것것이다. 이 요청을 받으면 해당 사이트는 응답해줄 것이고 그러면 개인정보를 해커가 빼돌릴 수 있다.

**CORS 동작 원리**

- 브라우저는 다른 Origin으로 요청을 보낼 때 Origin 헤더에 자신의 Origin을 설정
- 서버로부터 응답을 받으면 응답의 Access-Control-Allow-Origin 헤더에 설정된 Origin 목록을 검사
- 요청한 Origin 헤더 값이 포함되어 있으면 요청에 응답

1. **단순 요청(Simple Request)**
- 조건이 매우 까다롭기 때문에 단순 요청은 흔한 유형의 요청은 아니다.
- 아래 조건을 만족하면 단순 요청은 안전한 요청으로 취급
- 프리플라이 요청 필요없이 단 한번의 요청만을 전송함
- **CORS의 기본적인 동작원리를 그대로 따름**
- [조건](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS#단순_요청simple_requests)
    - **method = Get, Head, Post 중 하나여야함**
    - User Agent가 자동으로 설정한 헤더를 제외하면, 아래와 같은 헤더들만 사용 가능
        - Accept
        - Accept-Language
        - Content-Language
        - Content-Type
        - DPR
        - Downlink (en-US)
        - Save-Data
        - ViewPort-Width
        - Width
    - Content-Type 헤더는 아래와 같은 값들만 설정 가능
        - application/x-www-form-unlencode
        - multipart/form-data
        - text/plan
    - …

![Untitled](https://user-images.githubusercontent.com/117332903/236159382-df69d38e-10ae-4abf-9d0d-cd1d8e246be0.png)


1. **프리플라이트 요청(Preflight Request)**
- 단순 요청의 조건에서 벗어나는(=안전하지 않은) 요청의 경우 서버에 실제 요청을 보내기 전 예비 요청을 보냄
- 보낸 요청(프리플라이트 요청)으로 실제 요청이 전송하기 전에 안전한지 확인
- 안전하다고 판단되면 그제서야 실제 요청을 서버에 보냄
- 총 2번의 요청을 전송
- **프리플라이트 요청의 특징**
    - **method = OPTIONS 사용**
    - **Origin 헤더에** 자신의 Origin을 설정
    - **Access-Control-Request-Method 헤더에** 실제 요청에 사용할 메소드를 설정
    - **Access-Control-Request-Headers 헤더에** 실제 요청에 사용할 헤더들을 설정
- **서버는 다음과 같은 특징을 가진 응답을 제공**
    - **Access-Control-Allow-Origin 헤더에** 허용되는 Origin들의 목록 or 와일드카드(*)를 설정
    - **Access-Control-Allow-Methods 헤더에** 허용되는 메소드들의 목록 or 와일드카드(*)를 설정
    - **Access-Control-Allow-Headers 헤더에** 서용되는 헤더들의 목록 or 와일드카드(*)를 설정
    - **Access-Control-Max-Age 헤더에** 해당 프리플라이트 요청이 브라우저에 캐시 될 수 있는 시간을 초단위로 설정
- 해당 응답을 받으면 브라우저는 해당 정보와 자신이 요청한 정보와 비교하여 안전성을 검사
- 안전성을 확보하면 실제 요청을 서버에 보냄
- **이 때 Access-Control-Request-XXX 형태의 헤더는 보내지 않음**

![Untitled 1](https://user-images.githubusercontent.com/117332903/236159679-7d2e8205-fdaf-48ec-a2b1-315b85f4f73f.png)


1. **인증 정보를 포함한 요청(Credentialed Request)**
- **인증 정보(쿠키 or Authorization 헤더에 설정하는 토큰 값 등)가 있는 경우 사용**
- **별도로 따라줘야 하는 CORS정책이 존재**
- 쿠키 등의 인증 정보를 보내기 위해서는 클라이언트 단에서 요청 시 별도의 설정이 필요
- Ajax 요청을 위해 어떠한 도구를 사용하느냐에 따라 달라짐
    - **XMLHttpRequest, jQuery의 ajax, axios → withCredentials옵션을 ture로 설정**
    - **fetch API → credentials옵션을 include로 설정**
- 위와 같은 인증정보를 요청에 포함 시 일반적인 CORS요청과는 다르게 대응해주어야 함
- 응답의 Access-Control-Allow-Origin 헤더가 와일드카드(*)가 아닌 **분명한 Origin으로 설정**되고
- **Access-Control-Allow-Credentials 헤더는 true로 설정되어야 함**
- 그렇지 않으면 브라우저에 의해 응답이 거부 됨

![Untitled 2](https://user-images.githubusercontent.com/117332903/236159792-2ff946b5-8fa2-4a18-9ecd-318b92d9e490.png)
