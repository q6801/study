# OAuth
- [용어 정리](#용어-정리)
- [OAuth란?](#oauth란)
- [OAuth 1.0](#oauth-10)
- [OAuth 2.0](#oauth-20)
- [참고](#참고)
</br>

# 용어 정리

- authentication
    - 인증, 접근 자격이 있는지 **검증하는 단계** (회원가입, 로그인)
- authorization
    - 인가, **자원에 접근할 권한을 부여**하는 것
</br>

# OAuth란?

- 웹 사이트 계정 인증에 타 서비스의 계정을 사용하는 인증 방식을 **OAuth** 인증 방식이라고 한다.
    - 웹사이트 로그인을 네이버, 구글, 카카오를 이용하는 것을 뜻한다.
- **OAuth2**(Open Authorization, Open Authentication 2)는 **인증을 위한 표준 프로토콜**입니다.
- 유저가 네이버, 구글 카카오 등에 있는 자신의 정보를 다른 서비스에 건네주는 것을 허가하는 개념이다.
- OAuth는 Authentication뿐만 아니라 **Authorization도 포함**하고 있는 개념이다.
- OAuth도 인증 과정이 있지만 근본 목적은 기존 서비스(구글, 카카오)등에서 **API를 호출할 수 있는 권한**이 있는 사용자인지 확인하는 것에 있다.
</br>

# OAuth 1.0

### 단점

- 절차가 복잡하여 OAuth 구현 라이브러리를 제작하기 어려웠다.
- HMAC을 통해 따로 암호화하는 과정이 필요했다.
- access token를 만료시키지 않았다.

### 결론

- 그래서 OAuth 2.0이 탄생하게 되었다.
- oAuth 2.0은 HTTPS를 사용하여 따로 암호화가 필요 없고 Access Token의 만료 시간을 넣었다.
</br>

# OAuth 2.0

### 용어

- `Resource Server` : OAuth2.0 서비스를 제공하고 자원을 관리하는 서버 (구글 등)
- `Authorization Server` : Client가 Resource Server의 서비스를 사용할 수 있게 인증하는 서버 (구글 등)
- `Resource Owner` : Resource Server의 계정을 소유하고 있는 사용자 (유저)
- `Client` : Resource Server의 API를 사용하여 데이터를 가져오려하는 사이트 (application)
- `Access Token` : 리소스 서버에게서 **리소스 소유자의 보호된 자원을 획득할 때** 사용되는 만료 기간이 있는 Token
- `Refresh Token` : **Access Token 만료시 이를 갱신**하기 위한 용도로 사용하는 Token

</br>

### OAuth2.0 인증 방식의 종류

- `Authorization Code Grant`
    - client가 다른 사용자 대신 특정 리소스에 접근을 요청할 때 사용
    - resource 접근을 위해, Authorization server에서 받은 권한 코드로 리소스에 대한 액세스 토큰을 받는 방식
    - 다른 인증 절차에 비해 보안성이 높아서 주로 사용된다.
    - refresh token의 사용이 가능한 방법이다.

![Authorization Code Grant](https://raw.githubusercontent.com/q6801/study/main/CS/Network/img/OAuth/1.png)

---

- `Implicit Code Grant`
    - Authorization Code Grant와 다르게 권한 승인 코드 없이 access token이 발급된다.
    - Access Token을 즉시 반환 받아서 이를 인증에 사용하며 refresh token의 사용이 불가능하다.
    - **자격증명을 안전하게 보관할 수 없는 클라이언트(javascript 등)** 에게 최적화된 방법이다.
        - 자격 증명 : client에서 oAuth를 사용하기 위해 (구글, 카카오 등에서) 미리 얻어놓은 client_id, redirect_url등
        - 자격 증명이 안전하지 못하여 client_secret은 사용하지 않는다.

![Implicit Code Grant](https://raw.githubusercontent.com/q6801/study/main/CS/Network/img/OAuth/2.png)

---

- `Resource Owner Password Credentials Grant`
    - username, password를 user에게 받아서 access token을 client가 받는 방식이다.
    - client, resource server authorization server가 같은 시스템에 속할 때만 사용할 수 있다.
    - 그렇지 않다면 client에게 user의 pw가 바로 넘어가니 위험하다.

![Resource Owner Password Credentials Grant](https://raw.githubusercontent.com/q6801/study/main/CS/Network/img/OAuth/3.png)

---

- `Client Credentials Grant`
    - OAuth 2.0 권한 부여 방식 중 가장 간단한 방식
    - client의 자격 증명만으로 access token을 발급받는 방법으로 리소스 서버에서 해당 클라이언트를 위한 제한된 리소스 접근 권한이 설정되어야 한다.
    - 자격 증명이 안전하게 보관될 수 있는 클라이언트에서만 사용되고 refresh token 사용 불가

![Client Credentials Grant](https://raw.githubusercontent.com/q6801/study/main/CS/Network/img/OAuth/4.png)
</br>

# 참고
[한컴인텔리전스 블로그 : 네이버 블로그](https://blog.naver.com/mds_datasecurity/222182943542)

[Resource Owner의 승인 - 생활코딩](https://opentutorials.org/course/3405/22006)

[NAVER D2](https://d2.naver.com/helloworld/24942)
