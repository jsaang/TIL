
> 본 문서는 김영한님의 [인프런 HTTP 강의](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/)를 정리한 내용입니다.

## 1. 클라이언트에서 서버로 데이터 전송

데이터 전달 방식은 크게 두 가지
- 쿼리 파라미터를 통한 전송
  - GET
  - 주로 정렬 필터(검색어)
- 메시지 바디를 이용한 전송
  - POST, PUT, PATCH
  - 회원가입, 상품주문, 리소스 등록, 리소스 변경

### 정적 데이터 조회
1. 쿼리 파라미터 미사용
   - 이미지, 정적 텍스트 문서
   - 조회는 Get 사용
   - 일반적으로 쿼리 파라미터 없이 리소스 경로로 조회 가능

2. 쿼리 파라미터 사용
   - 주로 검색, 게시판 목록에서 정렬 필터(검색어)
   - 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
   - 조회는 GET 사용
   - GET은 쿼리 파라미터 사용해서 데이터를 전달

### HTML Form 데이터 전송
 - HTML Form submit시 POST 전송
   - 예) 회원가입, 상품주문, 데이터 변경
 - Content-Type: application/x-www-form-urlencoded 사용
   - form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
   - 전송 데이터를 url encoding 처리
 - HTML Form은 GET 전송도 가능
 - Content-Type: multipart/form-data
   - 파일 업로드 같은 바이너리 데이터 전송시 사용
   - 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능(그래서 이름이 multipart)
 - HTML Form 전송은 **GET, POST만 가능**

### HTTP API 데이터 전송
- 서버 to 서버
- 웹 클라이언트
  - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
  - 예) React, VueJs 같은 웹 클라이언트와 API 통신
- POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
- GET: 조회, 쿼리 파라미터로 데이터 전달
- Content-Type: application/json을 주로 사용(사실상 표준)
  - TEXT, XML, JSON 등등

## 2. HTTP API 설계 예시

### HTTP API - 컬렉션
- POST 기반 등록
- 예) 회원 관리 API 제공

- 회원 목록 /members -> GET
- 회원 등록 /members **-> POST**
- 회원 조회 /members/{id} -> GET
- 회원 수정 /members/{id} -> PATCH, PUT, POST
- 회원 삭제 /members/{id} -> DELETE

#### POST - 신규 자원 등록 특징
- 클라이언트는 등록될 리소스의 URI를 모름
  - 회원 등록 /members -> POST
  - POST /members
- 서버가 새로 등록된 리소스 URI를 생성함
  - HTTP/1.1 201 Created
    Location: **/members/100**
- 컬렉션(Collection)
  - 서버가 관리하는 리소스 디렉토리
  - 서버가 리소스의 URI를 생성하고 관리
  - 여기서 컬렉션은 /members

### HTTP API - 스토어
- PUT 기반 등록
- 예) 정적 컨텐츠 관리, 원격 파일 관리

- 파일 목록 /files -> GET
- 파일 조회 /files/{filename} -> GET
- 파일 등록 /files/{filename} **-> PUT**
- 파일 삭제 /files/{filename} -> DELETE
- 파일 대량 등록 /files -> POST

#### PUT - 신규 자원 등록 특징
- 클라이언트가 리소스 URI를 알고 있음
  - 파일 등록 /files/{filename} -> PUT
  - PUT **/files/star.jpg**
- 클라이언트가 직접 리소스의 URI를 지정함
- 스토어(Store)
  - 클라이언트가 관리하는 리소스 저장소
  - 클라이언트가 리소스의 URI를 알고 관리
  - 여기서 스토어는 /files

### HTML FORM 사용
- HTML Form은 GET, POST만 지원
- AJAX 같은 기술을 사용해서 해결 가능
- GET, POST만 지원하므로 제약 있음
