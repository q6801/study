# CORS
- [CORS](#cors)
- [Origin](#origin)
- [SOP (Same Origin Policy)](#sop-same-origin-policy)
- [CORS (Cross Origin Resource Sharing)](#cors-cross-origin-resource-sharing)
- [CORS 해결 방안](#cors-해결-방안)
- [CORS 동작 방법 3가지](#cors-동작-방법-3가지)
- [참고](#참고)
</br>

# Origin

- URL 구조
    - Protocol + Host + Port + Path + query String
- 출처(origin)은 URL 구조의 Protocol + Host + Port를 합친 것
</br>

# SOP (Same Origin Policy)

- 하나의 출처에서 로드된 자원이 출처가 다른 자원과 상호작용하지 못하도록 요청 발생을 제한하는 것.
- 즉, 동일 출처에서만 접근이 가능한 정책이다.
- 다른 출처에서의 접근을 막았기 때문에 보안적으로 좋아지지만 실제로 웹페이지는 다른 출처의 리소스를 사용해야하는 경우가 꽤 있다.
- 그래서 외부 리소스를 사용하기 위해 생긴 것이 CORS다.
</br>

# CORS (Cross Origin Resource Sharing)

- 서로 다른 도메인간에 자원을 공유하는 것을 뜻한다.
- 대부분 브라우저에서 이를 기본적으로 차단하며, 서버측에서 헤더를 통해서 사용가능한 자원을 알려준다.
- 다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS 헤더를 포함한 응답을 반환해야한다.
- CORS는 자바스크립트를 이용해서 XHR(XMLHttpRequest)를 다른 도메인으로 발생시킬 수 있게 해준다.
    - XHR 기반 cross origin HTTP 요청을 이용하여 자원을 공유해야하는 브라우저와 서버 간의 안전한 교차 출처 요청 및 데이터 전송을 지원한다.
- 출처를 비교하는 로직은 서버가 아닌 **브라우저에 구현되어 있는 스펙이다.**
    - CORS 정책을 위반하는 리소스 요청을 하더라도 서버에서 같은 출처만 받는 로직을 지니고 있지 않다면 서버는 정상적인 응답을 한다.
    - 하지만 브라우저가 응답을 분석하여 CORS 정책 위반이라 판단되면 그 응답을 버린다.
    - CORS 브라우저의 구현 스펙에 포함되는 정책이기 때문에, 서버간 통신할 떄는 이 정책이 적용되지 않는다.
</br>

# CORS 해결 방안

Access-Control-Allow-Origin 세팅

- 서버에서 Access-Control-Allow-Origin 헤더에 알맞은 값을 세팅해 주면 된다.
- 이때 와일드 카드인 * 을 사용하면 모든 출처에서 오는 요청을 받겠다는 의미이다.

Webpack Dev Server로 리버스 프록싱

- **로컬에서** 프론트엔드가 개발하는 경우에 발생하는 문제 해결 방법이다.
- webpack dev server에서 제공해주는 프록시 기능을 사용하면 쉽게 CORS 정책을 우회할 수 있다.
- 로컬(locahost:8000)에서의 요청을 웹팩이 프록싱하여 다른 url(예: [https://dremean.com)](https://dremean.com)으로)로 변경해줘서 CORS를 지킨 것처럼 자유로운 통신이 가능하다.
</br>

# CORS 동작 방법 3가지

simple request

- 클라이언트가 요청을 보낼 때 헤더 orgin에 출처를 써서 보낸다.
- 서버가 응답할 때 응답 헤더의 Access-Control-Allow-Origin에 접근 허용 출처를 써서 응답한다.
- 이 응답을 받은 브라우저는 origin과 access를 비교해본 후 유효한지 확인한다.
- 이 simple request를 하려면 특정 조건을 만족시켜야한다.
    
    ```java
    1. 요청의 메소드는 GET, HEAD, POST 중 하나여야 한다.
    2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안된다.
    3. 만약 Content-Type를 사용하는 경우에는 application/x-www-form-urlencoded, multipart/form-data, text/plain만 허용된다.
    ```
    

preflight request

- 브라우저가 본 요청을 보내기 전에 예비 요청을 보내는 것이다. (http 메소드 OPTIONS가 사용됨)
- 자바스크립트의 xhr이나 fetch api를 사용하여 리소스를 받아오라는 명령을 내리면 브라우저는 서버에 예비 요청을 보내서 가능 여부를 따진다.
- 자신의 origin과 허용 정책을 비교한 후 요청을 보내는 것이 안전하다고 판된되면 다시 본 요청을 보낸다.
- 일반적으로 웹 어플리케이션을 개발할 때 가장 마주치는 시나리오이다.

Credentialed Request

- 다른 출처 간 통신에서 좀 더 보안을 강화하고 싶을 때 사용하는 방법이다.
- 원래 XHR, Fetch API는 브라우저의 쿠키 정보나 인증 관련 헤더를 함부로 요청에 담지 않는다.
    - 이 때 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 바로 credentials이다.
- credential 옵션
    
    
    | 옵션 값 | 설명 |
    | --- | --- |
    | same-origin (기본값) | 같은 출처 간 요청에만 인증 정보를 담을 수 있다 |
    | include | 모든 요청에 인증 정보를 담을 수 있다 |
    | omit | 모든 요청에 인증 정보를 담지 않는다 |
- 이 옵션을 사용하면 브라우저는 Access-Control-Allow-Origin만 보고 허락하는 것이 아닌 다른 검사 조건들을 추가하게 된다. (보안적으로 더 좋아진다.)
</br>

# 참고

[CORS는 왜 이렇게 우리를 힘들게 하는걸까?](https://evan-moon.github.io/2020/05/21/about-cors/)