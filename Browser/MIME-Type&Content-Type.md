---

---

# MIME-Type / Content-Type

# **MIME(**Multipurpose Internet Mail Extensions)**이란?**

- MIME는 이메일과 함께 동봉할 파일을 텍스트 문자로 전환해서 이메일 시스템을 통해 전달하기 위해 개발되었다.
- 현재는 웹을 통해서 여러형태의 파일 전달하는데 쓰이고 있다.

### **MIME 사용 이유**

- MIME을 사용하기전에는 UUEncode에는 치명적인 단점이 있는데 이를 보강하기 위해 등장하게 되었다.
- 예전에는 텍스트파일을 주고받는데 ASCII로 공통된 표준에 따르기만하면 문제가 없었다.
하지만 네트워크를 통해 ASCII 파일이 아닌 **바이너리 파일**을 보내는 경우가 생기게 되었다.
    - **바이너리파일**
        
        → 음악파일, 무비파일, 워드파일 등등
        
- ASCII만으로는 전송이 불가능하여 이러한 바이너리 파일들을 기존의 시스템에서 문제 없이 전달하기 위해 텍스트파일로 변환이 필요했다.
- **텍스트파일로 변환을 인코딩(Encoding)**이라고하며, **텍스트 파일을 바이너리 파일로 변환하는 과정을 디코딩(Decoding)**이라고 한다.
- MIME으로 인코딩 한 파일은 Content-type 정보를 파일의 앞부분에 담게 되며, Content-type은 여러가지의 타입이 있다.
- 특정 Content-Type은 파일을 웹서버로 부터 전달 받아 웹브라우저에서 열수 있으며, 혹은 열수 없으며, 혹은 열수 있다는 것이다.
- 또한 바이너리 파일(음악파일, 무비파일, 워드파일 등등)도 마찬가지 이다. 주로 쓰고 있는 대부분의 포맷인 .gif .jpg .mov 등등의 파일들은 웹브라저에서 무리 없게 열리게 되는데 브라우저에서 지원하지 못하는 유형은 따로 지정을 해야 한다.

# **Content-Type**

우리가 클라이언트 브라우저로 어떤 자원을 보낼때(어떤 형태의 파일이나 문서 등), 웹 서버는 일련의 HTTP 헤더로 파일이나 자원을 포함하는 바이트의 Stream을 앞에 보낸다. 이런 헤더는 클라이언트에게 웹 서버와 커뮤니케이션 세부사항을 묘사한다. 예를 들어, 헤더는 사용되고 있는 웹 서버의 소프트웨어의 타입, 서버의 날짜와 시간, HTTP 프로토콜, 사용중인 커넥션 타입등을 지정한다. 헤더는 또한 클라이언트가 이런 가상 패스나 도메인에 대해서 저장해야 할 쿠키를 포함한다.

이와 관련해서 가장 한 것은 헤더는 또한 보내지는 자원의 content 타입이 포함되는 것이다. 이것은 Content-Type 헤더에 의해 지정되는데, 이 값은 표준 MIME-Type의 하나이다. MIME-Type을 살펴봄으로써 브라우져는 데이터를 나타내는데 어떤 종류의 파일 Stream인지를 알고 있다. HTML 페이지에서, 표준MIME type는"text/html"이다. 그리고 텍스트 파일이나 텍스트 stream은 "text/text"이다. 일반적인 MIME-Type과 파일 확장명의 매칭 리스트는 아래에 기술 하였다.

## **Content-Type의 종류**

**1) Multipart Related MIME 타입**

- Content-Type: Multipart/related <-- 기본형태

- Content-Type: Application/X-FixedRecord

**2) XML Media의 타입**

- Content-Type: text/xml

- Content-Type: Application/xml

- Content-Type: Application/xml-external-parsed-entity

- Content-Type: Application/xml-dtd

- Content-Type: Application/mathtml+xml

- Content-Type: Application/xslt+xml

**3) Application의 타입**

- Content-Type: Application/EDI-X12 <--  Defined in RFC 1767

- Content-Type: Application/EDIFACT <--  Defined in RFC 1767

- **Content-Type: Application/javascript** <-- Defined in RFC 4329

- Content-Type: Application/octet-stream  : <-- 디폴트 미디어 타입은 운영체제 종종 실행파일, 다운로드를 의미

- Content-Type: Application/ogg <-- Defined in RFC 3534

- Content-Type: Application/x-shockwave-flash <-- Adobe Flash files

**- Content-Type: Application/json** <-- JavaScript Object Notation JSON; Defined in RFC 4627

- Content-Type: Application/x-www-form-urlencode <-- HTML Form 형태

- **x-www-form-urlencode와 multipart/form-data은 둘다 폼 형태이지만 x-www-form-urlencode은 대용량 바이너리 테이터를 전송하기에 비능률적이기 때문에 대부분 첨부파일은 multipart/form-data를 사용하게 된다.**

**4) 오디오 타입**

- Content-Type: audio/mpeg <-- MP3 or other MPEG audio
- Content-Type: audio/x-ms-wma <-- Windows Media Audio;
- Content-Type: audio/vnd.rn-realaudio <-- RealAudio; 등등

**5) Multipart 타입**

- Content-Type: multipart/mixed: MIME E-mail;
- Content-Type: multipart/alternative: MIME E-mail;
- Content-Type: multipart/related: MIME E-mail <-- Defined in RFC 2387 and used by MHTML(HTML mail)
- Content-Type: multipart/formed-data <-- 파일 첨부

**6) TEXT 타입**

- Content-Type: text/css
- Content-Type: text/html
- Content-Type: text/javascript
- Content-Type: text/plain
- **Content-Type: text/xml**

**7) file 타입**

- **Content-Type: application/msword <-- doc**
- **Content-Type: application/pdf <-- pdf**
- **Content-Type: application/vnd.ms-excel <-- xls**
- **Content-Type: application/x-javascript <-- js**
- **Content-Type: application/zip <-- zip**
- **Content-Type: image/jpeg <-- jpeg, jpg, jpe**
- **Content-Type: text/css <-- css**
- **Content-Type: text/html <-- html, htm**
- **Content-Type: text/plain <-- txt**
- **Content-Type: text/xml <-- xml**
- **Content-Type: text/xsl <-- xsl**