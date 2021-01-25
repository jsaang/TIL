
> 본 문서는 김영한님의 [인프런 HTTP 강의](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/)를 정리한 내용입니다.

## 1. HTTP 메서드를 만들어보자

회원 정보 관리 API를 만들기 위해, 아래와 같이 API URI를 설계함
과연 이는 좋은 설계일까?

### Before
- 회원 목록 조회 /read-member-list
- 회원 조회 /read-member-by-id
- 회원 등록 /create-member
- 회원 수정 /update-member
- 회원 삭제 /delete-member

가장 중요한 것은 **리소스 식별**!!!

- 리소스의 의미는?
  - 회원을 등록하고 수정하고 조회하는게 리소스가 아님
  - 회원이라는 개념 자체가 바로 리소스임
- 리소스를 어떻게 식별하면 좋을까?
  - 회원을 등록하고 수정하고 조회하는 것을 모두 배제
  - 회원이라는 리소스만 식별하면 됨 -> 회원 리소스를 URI에 매핑함

### After
- 회원 목록 조회 /members
- 회원 조회 /members/{id}
- 회원 등록 /members/{id}
- 회원 수정 /members/{id}
- 회원 삭제 /members/{id}

그럼 이제 어떻게 구분하지??

### 리소스와 행위를 분리
- URI는 리소스만 식별!
- 리소스와 행위를 분리함
  - 리소스: 회원
  - 행위: 조회, 등록, 삭제, 변경
- 그렇다면 행위(메서드)는 어떻게 구분? -> GET, POST, PUT, PATCH, DELETE

## 2. HTTP 메서드 - GET, POST

### 주요 메서드
- GET: 리소스 조회
- POST: 요청 데이터 처리, 주로 등록에 사용
- PUT: 리소스를 대체, 해당 리소스가 없으면 생성
- PATCH: 리소스 부분 변경
- DELETE: 리소스 삭제

### GET
```
GET /search?q=hello&hl=ko HTTP/1.1
Host: www.google.com
```
- 리소스 조회
- 서버에 전달하고 싶은 데이터는 쿼리 파라미터를 통해서 전달
- 메시지 바디를 사용해서 데이터 전달할 수 있지만, 권장하지 않음

### POST
```
POST /members HTTP/1.1
Contetn-Type: application/json

{
    "username": "hello",
    "age": 20
}
```
- 요청 데이터 처리
- 메시지 바디를 통해 서버로 요청 데이터 전달
- 서버는 요청 데이터를 처리
  - 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행함
- 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용

## 3. HTTP 메서드 - PUT, PATCH, DELETE

### PUT
```
PUT /members/100 HTTP/1.1
Content-Type: application/json

{
    "username": "hello",
    "age": 20
}
```
- 리소스를 대체
  - 리소스가 있으면 대체
  - 리소스가 없으면 생성
  - 즉, 덮어쓰기
- 중요: **클라이언트가 리소스를 식별!!**
  - 클라이언트가 리소스 위치를 알고 URI 지정
  - POST와의 차이점
  - 리소스를 완전히 대체해버림

### PATCH
- 리소스 부분 변경

### DELETE
- 리소스 제거

## 4. HTTP 메서드의 속성

### 안전(Safe)
- 호출해도 리소스를 변경하지 않음

### 멱등(Idempotent)
- 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 같음
- 멱등인 메서드: GET, PUT, DELETE, **POST(X)**
- 활용
  - 자동 복구 메커니즘
  - 서버가 TIMEOUT 등으로 정상 응답을 못했을 때, 클라이언트가 같은 요청을 다시 해도 되는가?의 근거

### 캐시가능(Cacheable)
- 응답 결과 리소스를 캐시해서 사용해도 되는가?
- 캐시 가능: GET, HEAD, POST, PATCH