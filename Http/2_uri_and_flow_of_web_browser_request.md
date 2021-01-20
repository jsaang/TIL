
> 본 문서는 김영한님의 [인프런 HTTP 강의](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/)를 정리한 내용입니다.


## 1. URI(Unifrom Resource Identifier)

- URI? URL? URN?
- URI = URL(Resource Locator) + URN(Resource Name)
    - **U**niform: 리소스를 식별하는 통일된 방식
    - **R**esouce: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
    - **I**dentifier: 다른 항목과 구분하는데 필요한 정보
- URL : 리소스가 있는 위치를 지정
- URN: 리소스에 이름을 부여
- 거의 URL만 사용함 → 앞으론 URI라함은 URL과 같은 의미로 설명함

### URL Structure

- scheme://[userinfo@]host[:port][/path][?query][#fragment]
- https://www.google.com:443/search?q=hello&hl=ko
1. scheme
    - 주로 프로토콜 사용
2. userinfo
    - URL에 사용자 정보를 포함하여 인증
    - 거의 사용하지 않음(보안 때문이겠지?)
3. host
    - 호스트명
    - 도메인이나 IP 주소 사용 가능
4. port
5. path
    - 리소스 경로, 계층적 구조
6. query
    - 웹서버에 제공하는 파라미터(문자 형태)
    - key=value 형태
    - ?로 시작하고 &로 추가 가능

## 2. 웹 브라우저 요청 흐름

1. 웹 브라우저가 HTTP 메시지 생성
2. SOCKET 라이브러리를 통해 전달
    - A: TCP/IP 연결(IP, PORT)
    - B: 데이터 전달
3. TCP/IP 패킷 생성, HTTP 메시지 포함
4. 서버가 HTTP 응답 메시지 답신