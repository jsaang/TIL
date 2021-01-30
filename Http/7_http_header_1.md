
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