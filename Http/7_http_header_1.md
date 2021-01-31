
> 본 문서는 김영한님의 [인프런 HTTP 강의](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/)를 정리한 내용입니다.

## 1. HTTP 헤더 개요

- HTTP 전송에 필요한 모든 부가정보
- 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시관리 정보
- 표준 헤더가 너무 많음
- 필요시 임의의 헤더 추가 가능

## 2. 표현(Representation)

- Content-Type: 표현 데이터의 형식
- Content-Encoding: 표현 데이터의 압축 방식
- Content-Language: 표현 데이터의 자연 언어
- Content-Length: 표현 데이터의 길이
- 표현 헤더는 전송, 응답 둘 다 사용

### Content-Type: 표현 데이터의 형식 설명

- 미디어 타입, 문자 인코딩
- 예) text/html; charset=utf-8

### Content-Encoding: 표현 데이터 인코딩

- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서 압출 후 인코딩 헤더 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
- 예) gzip

### Content-Language: 표현 데이터의 자연 언어

- 표현 데이터의 자연 언어를 표현
- 예) ko, en, en-US

### Content-Length: 표현 데이터의 길이

- 바이트 단위
- Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨

## 3. 콘텐츠 협상

**클라이언트가 선호하는 표현 요청**

- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어
- 협상 헤더는 요청시에만 사용

### 협상과 우선순위: Quality Values(q)

- Quality Values(q) 값 사용
- **0~1, 클수록 높은 우선순위**
- 생략하면 1
- Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
- **구체적인 것이 우선함**
- Accept: text/*, text/plain, text/plain;format=flowed
    1. text/plain;format=flowed
    2. text/plain
    3. text/*
- **구체적인 것을 기준으로 미디어 타입을 맞춤**

## 4. 전송 방식

### 단순 전송

- Content-Length
- 한번에 요청하고 한번에 쭉 받음

### 압축 전송

- Content-Encoding
- 컨텐츠를 압축해서 전송해줌

### 분할 전송

- Transfer-Encoding
- 컨텐츠를 쪼개서 차례차례 보내줌
- Content-Length를 보내면 안됨

### 범위 전송

- Content-Range
- 범위를 지정해서 컨텐츠를 보내줌

## 5. 일반 정보

### From: 유저 에이전트의 이메일 정보

- 일반적으로 잘 사용되지 않음
- 검색 엔진 같은 곳에서, 주로 사용
- 요청에서 사용

### Referer: 이전 웹 페이지 주소

- 현재 요청된 페이지의 이전 웹 페이지 주소
- A → B로 이동하는 경우 B를 요청할 때 Referer:A를 포함해서 요청
- Referer를 사용해서 유입 경로 분석 가능
- 요청에서 사용

### User-Agent: 유저 에이전트 어플리케이션 정보

- 클라이언트의 어플리케이션 정보(웹 브라우저 정보 등)
- 통계 정보
- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
- 요청에서 사용

### Server: 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보

- Server: Apach/2.2.22(Debian)
- server: nginx
- 응답에서 사용

### Date: 메시지가 발생한 날짜와 시간

- 응답에서 사용

## 6. 특별한 정보

### Host: 요청한 호스트 정보(도메인)

- 요청에서 사용
- **필수!!**
- 하나의 서버가 여러 도메인을 처리해야 할 때
- 하나의 IP 주소에 여러 도메인이 적용되어 있을 때

### Location: 페이지 리다이렉션

- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, 그 위치로 자동 이동(리다이렉트)
- 응답코드 3xx에서 설명
- 201(Created): Location 값은 요청에 의해 생성된 리소스 URI
- 3xx(Redirection): Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴

### Allow: 허용 가능한 HTTP 메서드

- 405(Method Not Allowed)에서 응답에 포함해야함
- Allow: GET, HEAD, PUT

### Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

- 503(Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음

## 7. 인증

### Authorization: 클라이언트를 인증 정보를 서버에 전달

- Authorization: Basic xxxxxxxxxxxx

### WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의

- 리소스 접근시 필요한 인증 방법 정의
- 401 Unauthorized 응답과 함께 사용
- WWW-Authenticate: Newauth realm="apps", type=1, title="Login to \"apps\"", Basic realm="simple"

## 8. 쿠키

- 예) set-cookie: sessionId=abc123; expires=Sat, 26-Dec-2020; path=/; domain=.google.com; Secure
- 사용처
    - 사용자 로그인 세션 관리
    - 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송됨
    - 네트워크 트래픽 추가 유발
    - 최소한의 정보만 사용(세션 id, 인증 토큰)
    - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 참고
- 주의!
    - 보안에 민감한 데이터는 저장하면 안됨(주민번호, 카드번호 등)

### 쿠키-생명 주기

- Set-cookie: expires=xxx
    - 만료일이 되면 쿠키 삭제
- Set-cookie: max-age=3600(3600초)
    - 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

### 쿠키-도메인

- 예) domain=example.org
- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
- 생략: 현재 문서 기준 도메인에만 적용

### 쿠키-경로

- 예) path=/home
- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
- 일반적으로 path=/ 루트로 지정

### 쿠키-보안

- Secure
    - 쿠키는 http, https를 구분하지 않고 전송
    - Secure를 적용하면 https인 경우에만 전송
- HttpOnly
    - XSS 공격 방지
    - 자바스크립트에서 접근 불가
    - HTTP 전송에만 사용
- SameSite
    - XSRF 공격 방지
    - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송