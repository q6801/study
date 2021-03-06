# HTTP (HyperText Transfer Protocol)
- [HTTP란?](#http란)
- [HTTP 역사](#http-역사)
- [HTTP 특징](#http-특징)
- [HTTP의 문제점](#http의-문제점)
- [HTTP 메서드 종류](#http-메서드-종류)
- [HTTP 상태코드](#http-상태코드)
- [참고](#참고)

</br>

# HTTP란?

- HTTP(Hyper Text Transfer Protocol)는 html을 전송하는 프로토콜로 처음 시작됐는데 지금은 모든 것을 전송할 수 있다.
- HTML, TEXT, IMAGE, 음성, 영상, 파일, JSON, XML (API) 등
- 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용
</br>

# HTTP 역사

- HTTP/0.9 1991년: GET 메서드만 지원, HTTP 헤더X
- HTTP/1.0 1996년: 메서드, 헤더 추가
- HTTP/1.1 1997년: 가장 많이 사용, 우리에게 가장 중요한 버전RFC2068 (1997) -> RFC2616 (1999) -> RFC7230~7235 (2014)
- HTTP/2 2015년: 성능 개선
- HTTP/3 진행중: TCP 대신에 UDP 사용, 성능 개선

### 기반 프로토콜

- TCP: HTTP/1.1, HTTP/2
- UDP: HTTP/3
- 현재 HTTP/1.1 주로 사용
- HTTP/2, HTTP/3도 점점 증가
</br>

# HTTP 특징

- 클라이언트 서버 구조로 동작한다
- 무상태 프로토콜(Stateless) 지향, 비연결성(conectionless)
- HTTP 메시지를 통해서 통신한다
- 단순하고 확장 가능하다.

### 무상태 프로토콜(stateless)

- 서버가 클라이언트의 상태를 보존X
- 장점: 서버 확장성 높음 (스케일 아웃)
    - 서버가 client의 상태를 보존하지 않으니 새로 서버를 증설하기 쉽다.
- 단점
    - 모든 것을 무상태로 설계할 수 없는 경우도 있다. ex) 로그인을 하면 유저의 상태 유지를 해줘야한다.
    - 하지만 http는 stateless이기 때문에 상태를 유지하기 위해 브라우저 쿠키와 서버 세션 등을 이용한다.

### 비 연결성(connectionless)

- 클라이언트와 서버는 요청을 주고 받을때만 연결을 유지해서 최소한의 자원을 사용한다.
- HTTP는 기본이 연결을 유지하지 않는 모델
    - ex) 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
    - == 서버 자원을 매우 효율적으로 사용할 수 있음
- 하지만 지속적인 연결이 더 효율적인 상황또한 존재
    - 나머지 자세한 내용은 **persistent connection 참고**
</br>

# HTTP의 문제점

- HTTP는 평문 통신이기 떄문에 도청이 가능하다.
- 통신 상대를 확인하지 않기 떄문에 위장이 가능하다.
    - SSL로 극복 가능. SSL은 상대를 확인하는 수단으로 증명서를 제공한다. 증명서는 제 3자 기관에 의해 발행됨
    - 이 증명서를 이용해서 통신 상대가 내가 통신하고자하는 서버임을 나타냄
- 완전성을 증명할 수 없어서 변조가 가능하다.
    - 공격자가 req, res를 빼앗아 변조하는 man in the middle 공격에 취약함
    - 방지를 위해 HTTPS 써야됨

### TCP/IP는 도청 가능한 네트워크이다.

- TCP/IP 구조의 통신은 전부 통신 경로 상에서 엿볼 수 있다.
    - 패킷을 수집하는 것만으로도 도청 가능
- 보완 방법
    - 통신 자체를 암호화(SSL, TLS) 프로토콜과 조합함으로 HTTP 통신을 암호화할 수 있다.
</br>

# HTTP 메서드 종류

주요 메서드

- GET: 리소스 조회
    - 일반적으로 Request Body는 입력하지 않는 것이 일반적이며, 레거시 시스템의 경우 요청을 받아들이지 않을 수 있다
        - 요청하는 데이터는 query로 보냄
    - 캐싱을 수행하기 때문에 캐싱되지 않는 요청은 GET 요청이 맞지 않을 수 있습니다.
        - 즉, post로 할 일을 get으로 하는 경우 기존의 데이터가 응답될 수 있음
- POST: 요청 데이터 처리, 주로 등록에 사용
    - POST 방식의 request 는 HTTP Request Message의 Body 부분에 데이터가 담겨서 전송된다.
    - POST 요청은 서버의 상태를 변경시키기 때문에 멱등성이 유지되지 않습니다.
- PUT: 리소스를 대체, 해당 리소스가 없으면 생성
- PATCH: 리소스 부분 변경
- DELETE: 리소스 삭제, 존재하지 않아도 동일하게 동작
</br>

기타 메서드

- HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
    - 응답내용이 필요없이 정상 호출 여부를 확인할 떄 사용
- OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
- CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정
- TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

![Resource Owner Password Credentials Grant](https://raw.githubusercontent.com/q6801/study/main/CS/Network/img/HTTP/1.png)
</br>

### HTTP 메서드의 속성

- 안전(Safe Methods)
- 멱등(Idempotent Methods) (연산을 여러 번 적용해도 결과가 달라지지 않는 성질) 보장
    - GET: 한 번 조회하든, 두 번 조회하든 같은 결과가 조회된다.
    - PUT: 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
    - DELETE: 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 똑같다.
    - POST: 멱등이 아니다! 두 번 호출하면 같은 결제가 중복해서 발생할 수 있다.
- 캐시가능(Cacheable Methods)
    - 응답 결과 리소스를 캐시해서 사용해도 되는가?
    - GET, HEAD, POST, PATCH 캐시 가능
    - 실제로는 GET, HEAD 정도만 캐시로 사용
    - POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음
</br>

# HTTP 상태코드

### HTTP 상태코드 소개

**상태 코드클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능**

- 1xx (Information): 요청이 수신되어 처리중
- 2xx (Successful): 요청 정상 처리
- 3xx (Redirection): 요청을 완료하려면 추가 행동이 필요
- 4xx (Client Error): 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함
</br>

**리다이렉션이란?**

- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)

**영구 리다이렉션301, 308**

- 리소스의 URI가 영구적으로 이동
- 원래의 URL을 사용X, 검색 엔진 등에서도 변경 인지
- 301 Moved Permanently
    - 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)
- 308 Permanent Redirect
    - 301과 기능은 같음
    - 리다이렉트시 요청 메서드와 본문 유지(처음 POST를 보내면 리다이렉트 유지)

**HTTP 헤더용도**

- HTTP 전송에 필요한 모든 부가정보
- 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보...
- 표준 헤더가 너무 많음
- 필요시 임의의 헤더 추가 가능
    - helloworld: hihi
</br>

# 참고
[모든 개발자를 위한 HTTP 웹 기본 지식 - 인프런 김영한님 강의 정리](https://velog.io/@chss3339/httplecture)