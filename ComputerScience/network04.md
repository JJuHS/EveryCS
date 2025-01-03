## 1. HTTP 개요
### HTTP란 무엇인가?
> HTTP(HyperText Transfer Protocol)이란 웹 상에서 클라이언트와 서버 간에 데이터를 주고받기 위한 프로토콜

> 주로 웹 브라우저(클라이언트)가 웹 서버에 요청을 본내고, 서버가 그에 대한 응답을 반환하는 방식으로 동작한다.

> **역할**
> > 데이터 전송 : 텍스트, 이미지, 동영상 등 다양한 형태의 데이터를 전송
>
> > 상태 관리 : 클라이언트와 서버 간의 상호작용 상태를 관리
>
> > 표준화 : 웹 통신의 표준을 제공하여 다양한 기기와 애플리케이션 간의 호환성 보장


### 클라이언트-서버 모델

> **클라이언트**
>
> 요청을 보내는 주체 (보통 웹 브라우저)

> **서버**
> 
> 요청을 받아 처리하고 응답을 보내는 주체 (웹 서버, 개발자의 백엔드서버)

> **동작 방식**
>
> 클라이언트가 특정 서버에 특정 리소스 요청
> 
> 서버가 요청을 처리하고 적절한 응답을 반환
> 
> 클라이언트가 응답을 받아 사용자에게 표시하거나 처리를 수행


### HTTP의 역사와 발전
> <strong>HTTP/0.9 (1991)</strong>
> 
> 최초의 HTTP 버전으로, 단순한 텍스트 기반 프로토콜.
> 
> 기능: 단순히 HTML 문서를 요청하고 반환.
> 
> 제한점: 헤더 정보가 없고, 단일 요청-응답만 지원.
> 
> <strong>HTTP/1.0 (1996)</strong>
> 
> 헤더 도입: 요청과 응답에 헤더가 추가되어 메타데이터 전송 가능.
> 
> 메서드 추가: GET, POST 등 기본적인 메서드 지원.
> 
> 단점: 연결을 요청할 때마다 새로운 TCP 연결을 생성해야 함.

> <strong>HTTP/1.1 (1997)</strong>
> 
> 지속 연결 (Persistent Connections): 동일한 TCP 연결을 통해 여러 요청과 응답을 주고받을 수 있어 성능 향상.
> 
> 파이프라이닝 (Pipelining): 요청을 순차적으로 보내고, 응답을 순서대로 받을 수 있음 (하지만 구현이 복잡하여 널리 사용되지는 않음).
> 
> 추가 기능: 캐싱, 호스트 헤더, 내용 인코딩 등 다양한 기능 추가.

><strong>HTTP/2 (2015)</strong>
>
>멀티플렉싱 (Multiplexing): 단일 TCP 연결에서 여러 요청과 응답을 동시에 주고받을 수 있어 지연 시간 감소.
>
>헤더 압축 (Header Compression): 헤더 정보를 압축하여 전송 효율성 향상.
>
>서버 푸시 (Server Push): 서버가 클라이언트의 요청 없이도 필요한 리소스를 미리 전송할 수 있음.
>
>바이너리 프로토콜 (Binary Protocol): 텍스트 기반에서 이진 형식으로 전환하여 처리 속도와 효율성 개선.

> <strong>HTTP/3 (2022)</strong>
>
> QUIC 프로토콜: TCP 대신 UDP 기반의 QUIC 프로토콜을 사용하여 연결 설정 및 전송 지연 시간 감소.
>
> 향상된 성능: 패킷 손실 시 빠른 복구와 더 나은 네트워크 조건에서의 성능 향상.
>
> 보안 강화: 기본적으로 TLS 1.3을 사용하여 보안성을 높임.
>
> 멀티플렉싱 개선: HTTP/2의 멀티플렉싱 문제(헤드 오브 라인 블로킹)를 해결하여 더 효율적인 데이터 전송 가능.

## 2. HTTP 요청과 응답 구조 
### HTTP 요청 구성 요소
> **요청 라인**
> > Method : 클라이언트가 서버에 요청하는 작업의 종류 (get, put, post, delete)
> >
> > URL(Uniform Resource Locator) : 요청하고자 하는 리소스의 주소
> > 
> > HTTP 버전 : 사용하고 있는 HTTP 프로토콜의 버전 명시
> 
> **요청 헤더**
> > Host : 요청을 보내는 서버의 호스트 이름과 포트
> >
> > User-Agent : 클라이언트 소프트웨어의 정보
> > 
> > Accept : 클라이언트가 처리할 수 있는 미디어 타입 명시
> > 
> > Content-Type : 요청 본문의 미디어 타입 지정
> 
> **요청 본문**
> > 서버로 전송하는 데이터의 실제 내용

### HTTP 응답 구성 요소
> **상태 라인**
> > HTTP 버전 : 사용된 HTTP버전
> > 
> > 상태 코드 : 요청의 결과 (200, 403, 404 등)
> > 
> > 상태 메시지 : 상태 코드에 대한 간단한 설명
>
> **응답 헤더**
> > Content-Type : 응답 본문의 미디어 타입 지정
> >
> > Content-Length : 응답 본문의 길이 (바이트 단위)
> >
> > Set-Cookie : 클라이언트에 쿠키 설정
> > 
> > Cache-Control : 응답의 캐시 정책 지정
> 
> **응답 본문**
> > 서버가 클라이언트에게 전송하는 실제 데이터

> 크롬의 개발자도구 (F12) 에서 네트워크 탭으로 이동하면 요청에 대한 정보를 확인할 수 있다.


## 3. HTTP 메서드와 상태 코드
> **HTTP 메서드 종류** : GET, POST, PUT, DELETE, PATCH, OPTIONS, HEAD
> > **GET 메소드**
> >
> > 서버로부터 데이터를 요청할 때 사용한다.
> >
> > 서버의 상태를 변경하지 않으며, 데이터의 크기에 제한이 있다.
> >
> > 페이지 로드에 필요한 요소나 기타 정보를 받는 상황에서 사용한다.
> 
> > **POST 메소드**
> > 
> > 서버에 데이터를 제출할 때 사용한다.
> > 
> > 주로 리소스를 생성하거나 업데이트 할 때 사용한다.
> >  
> > 서버의 상태를 변경할 수 있으며, 큰 데이터의 전송이 가능하다. 
> 
> > **PUT 메소드**
> >  
> > 서버에 특정 리소스를 생성하거나 업데이트 할 때 사용한다.
> >  
> > 멱등성(Idempotent) : 같은 요청을 여러 번 보내더라도 결과가 동일하다.
> 
> > **DELETE 메소드** 
> >  
> > 서버에 특정 리소스를 삭제할 때 사용한다.
> >  
> > 멱등성(Idempotent) : 같은 요청을 여러 번 보내더라도 결과가 동일하다.

> **상태 코드** 
> > **200 OK**
> > 
> > 클라이언트가 존재하는 리소스를 성공적으로 요청한 경우
> > 
> > 보통 GET 요청의 반환값
> 
> > **201 CREATED**
> > 
> > 클라이언트가 새로운 리소스를 성공적으로 생성한 경우
> > 
> > 보통 POST 요청의 반환값
> 
> > **400 BAD REQUEST**
> > 
> > 클라이언트가 잘못된 형식의 요청을 보냈을 때
> > 
> > 필수 필드가 누락된 POST, PUT 등의 요청
> 
> > **401 Unauthorized**
> > 
> > 인증이 필요한 리소스에 인증 없이 접근 한 경우
> 
> > **404 NOT Found**
> > 
> > 클라이언트가 존재하지 않는 리소스를 요청한 경우
> 
> > **500 Internal Server Error**
> > 
> > 서버 내부에서의 오류가 발생한 경우

## 4. HTTP 헤더와 쿠키 
> **HTTP 헤더의 역할과 종류**
> > HTTP 헤더는 클라이언트와 서버 간에 추가적인 정보를 전달하는데 사용된다. 요청, 응답 모두에 포함될 수 있다.
> 
> > **역할** : 메타데이터 전달, 컨텐츠 협상, 인증 및 보안, 캐싱 제어
> 
> > **종류** : 일반 헤더, 요청 헤더, 응답 헤더, 엔티티 헤더
> 
> > **예시**
> > > Content-Type(전송되는 데이터의 미디어 타입 지정): application/json
> > 
> > > Accept(클라이언트가 처리할 수 있는 미디어 타입): text/html,application/xhtml+xml,application/xml;q=0.9
> > 
> > > Authorization(인증 정보를 전송): Bearer token
> > 
> > > Cache-Control(리소스의 캐싱 정책을 정의): no-cache
> > 
> > > Set-Cookie(서버가 클라이언트에 쿠키를 설정할 때 사용): sessionId=abc123; HttpOnly; Secure

> **쿠키와 세션 관리**
>
> 쿠키는 클라이언트와 서버 간에 상태 정보를 저장하고 관리하는 데 사용되는 작은 데이터 조각이다.
> 
> 쿠키는 사용자의 브라우징 경험을 개인화하고, 인증 상태를 유지하며, 사용자 선허도를 저장하는 등 다양한 용도로 활용된다.
>
> **쿠키의 구조**
> 
> 쿠키는 여러 속성으로 구성되며, 주요 속성은 다음과 같다.
> > Name=Value: 쿠키의 이름과 값.
> > > `sessionId=abc123`
> >
> > Expires: 쿠키의 만료 날짜와 시간
> > > `Expires=Wed, 09 Jun 2024 10:18:14 GMT`
> > 
> > Max-Age: 쿠키의 유효 기간을 초 단위로 지정
> > > `Max-Age=3600`
> > 
> > Domain: 쿠키가 유효한 도메인
> > > `Domain=example.com`
> > 
> > Path: 쿠키가 유효한 경로
> > > `Path=/account`
> > 
> > Secure: HTTPS 연결을 통해서만 쿠키를 전송.
> > > `Secure`
> > 
> > HttpOnly: 자바스크립트를 통해 쿠키에 접근할 수 없도록 설정
> > > `HttpOnly` 
> > 
> > SameSite: 크로스 사이트 요청 시 쿠키 전송을 제한
> > > `SameSite=Strict`
>  
> **쿠키의 사용 사례**
> > 세션 관리
> > 
> > 사용자가 로그인 상태를 유지하도록 한다.
> > 
> > 서버는 sessionId 쿠키를 설정하여 사용자의 세션을 추적한다.
> 
> > 개인화
> > 
> > 사용자 선호도, 설정을 저장한다.
> 
> > 추적 및 분성
> > 
> > 사용자의 행동을 추적하여 분석 데이터를 수집한다.

> **세션 관리의 기본 개념**
> > 세션 : 사용자와 서버 간의 지속적인 상호작용을 의미한다.
> 
> > 세션 ID : 고유한 ID 를 통해 사용자의 세션을 식별하고 추적한다.
> 
> > 서버 측 세션 : 세션 데이터를 서버에 저장하고, 클라이언트는 세션 ID만 전달한다.
> 
> > 클라이언트 측 세션 : 쿠키 등에 세션 데이터를 직접 저장한다.

## 5. RESTful API와 HTTP 
> **REST의 기본 원칙**
>
> REST(Representational State Transfer)는 웹 기반의 아키텍처 스타일로, 클라이언트와 서버 간의 상호작용을 정의하는 일련의 원칙을 포함한다.
> > **자원(Resource)**: RESTful API에서 모든 것은 자원으로 표현된다. 각 자원은 URI(Uniform Resource Identifier)로 식별된다.
>
> > **표현(Representation)**: 자원은 다양한 형식(예: JSON, XML 등)으로 표현될 수 있다. 클라이언트는 원하는 표현 형식을 요청할 수 있다.
>
> > **상태 전이(State Transfer)**: 클라이언트는 서버에서 자원 상태를 전이하는 방식으로 상호작용한다. 요청은 자원에 대한 상태 변경을 포함할 수 있다.

> **HTTP와 REST의 관계**
> > REST는 HTTP 프로토콜을 기반으로 하며, HTTP 메서드를 사용하여 자원에 대한 CRUD(Create, Read, Update, Delete) 작업을 수행한다. RESTful API는 HTTP의 다양한 메서드(GET, POST, PUT, DELETE 등)를 통해 자원에 대한 접근을 정의한다.

> **RESTful한 설계에서의 HTTP 메서드 활용**
> > **GET**: 자원 조회 (예: 사용자 목록 요청)
>
> > **POST**: 자원 생성 (예: 새 사용자 등록)
>
> > **PUT**: 자원 업데이트 (예: 사용자 정보 수정)
>
> > **DELETE**: 자원 삭제 (예: 특정 사용자 삭제)

## 6. HTTP 보안 (HTTPS)
> **HTTPS란 무엇인가?**
> 
> HTTPS(HyperText Transfer Protocol Secure)는 HTTP에 SSL/TLS 암호화를 추가하여 웹 통신의 보안을 강화한 프로토콜이다.
>
> HTTPS를 사용하면 데이터가 클라이언트와 서버 간에 안전하게 전송된다.


> **SSL/TLS의 역할**
>
> SSL(Secure Sockets Layer)과 TLS(Transport Layer Security)는 HTTPS의 기반이 되는 프로토콜로, 데이터 암호화, 인증, 무결성을 보장한다.
>
> SSL은 더 이상 사용되지 않으며, 현재 TLS가 표준으로 자리 잡고 있다.

> **HTTPS의 중요성**
> 
> > 데이터 암호화: 전송되는 데이터가 암호화되어 외부의 공격자로부터 보호된다.
> >
> > 클라이언트와 서버 간의 통신이 암호화되어 도청이나 변조를 방지.
>
> > 인증: 웹사이트의 신뢰성을 확인하여 사용자가 안전하게 접속할 수 있도록 한다.
> >
> > SSL/TLS 인증서를 통해 사용자는 웹사이트가 진짜임을 확인할 수 있다.
>
> > 무결성: 데이터가 전송 중에 변경되지 않았음을 보장하여 중간자 공격을 방지한다.
> > 
> > 전송 중 데이터가 변경되지 않았음을 확인할 수 있는 메커니즘 제공.
