400번대 HTTP 오류 코드는 **클라이언트 측(Client-side) 오류**를 나타냅니다. 요청이 잘못되었거나, 인증이 필요하거나, 권한이 없는 경우 등에 발생합니다.

---

## 🔹 **주요 400번대 HTTP 오류 코드**

|상태 코드|설명|
|---|---|
|**400 Bad Request**|잘못된 요청 (유효하지 않은 파라미터, 형식 오류 등)|
|**401 Unauthorized**|인증 필요 (로그인하지 않음)|
|**402 Payment Required**|결제 필요 (대부분 사용되지 않음)|
|**403 Forbidden**|접근 권한 없음 (인가 실패)|
|**404 Not Found**|요청한 리소스를 찾을 수 없음|
|**405 Method Not Allowed**|허용되지 않은 HTTP 메서드 사용 (예: `GET`만 가능한데 `POST` 요청)|
|**406 Not Acceptable**|클라이언트가 요청한 응답 형식을 제공할 수 없음|
|**407 Proxy Authentication Required**|프록시 인증 필요|
|**408 Request Timeout**|클라이언트가 요청을 너무 오래 걸려서 서버가 연결 종료|
|**409 Conflict**|리소스 충돌 (예: 중복된 데이터 요청)|
|**410 Gone**|요청한 리소스가 더 이상 존재하지 않음 (404와 유사하지만 영구적 삭제)|
|**411 Length Required**|`Content-Length` 헤더가 필요하지만 없음|
|**412 Precondition Failed**|요청이 서버의 사전 조건을 충족하지 않음|
|**413 Payload Too Large**|요청 본문 크기가 서버가 허용하는 한도를 초과|
|**414 URI Too Long**|요청 URL 길이가 너무 길어서 처리할 수 없음|
|**415 Unsupported Media Type**|서버가 지원하지 않는 요청 데이터 형식 (예: `application/xml`만 지원하는데 `application/json` 요청)|
|**416 Range Not Satisfiable**|클라이언트가 요청한 `Range`(부분 데이터 요청)가 범위를 벗어남|
|**417 Expectation Failed**|`Expect` 헤더의 기대 조건을 충족하지 못함|
|**418 I'm a teapot** ☕|(농담 코드) HTCPCP 프로토콜에서 사용됨 😂|
|**421 Misdirected Request**|서버가 해당 요청을 처리할 수 없음|
|**422 Unprocessable Entity**|요청 형식은 올바르지만, 의미적으로 처리할 수 없음 (예: JSON 필드 값이 잘못됨)|
|**423 Locked**|리소스가 잠겨 있어 요청을 처리할 수 없음|
|**424 Failed Dependency**|요청이 다른 요청에 의존하는데, 그 요청이 실패함|
|**425 Too Early**|요청을 너무 일찍 보냈음 (보안 문제 방지)|
|**426 Upgrade Required**|클라이언트가 다른 프로토콜로 업그레이드해야 함|
|**428 Precondition Required**|요청 전에 특정 조건이 필요함|
|**429 Too Many Requests**|너무 많은 요청을 보냄 (Rate Limit 초과)|
|**431 Request Header Fields Too Large**|요청 헤더가 너무 큼|
|**451 Unavailable For Legal Reasons**|법적 문제로 인해 리소스 접근 불가|

---

## 🔥 **자주 발생하는 400번대 오류 예시**

1. **400 Bad Request**
    
    - JSON 형식 오류 (`{ "name": "John", "age": "twenty" }` → `age`는 숫자여야 함)
    - 필수 파라미터 누락 (`/api/user?id=` → `id` 값이 없음)
2. **401 Unauthorized**
    
    - 로그인하지 않은 사용자가 보호된 API 요청
3. **403 Forbidden**
    
    - 일반 사용자가 관리자 페이지 접근 시 (`/admin`)
4. **404 Not Found**
    
    - 존재하지 않는 페이지 요청 (`/api/product/9999` → 없는 상품)
5. **409 Conflict**
    
    - 회원가입 시 이미 사용 중인 이메일 입력
6. **429 Too Many Requests**
    
    - 짧은 시간 내 너무 많은 API 요청 (Rate Limiting)

---

### ✅ **정리**

- **400~499**: 클라이언트의 요청이 잘못되었거나, 인증/권한 문제가 있음.
- **400, 401, 403, 404**가 가장 자주 발생.
- **422, 429**도 API 개발에서 많이 사용됨.

**어떤 오류를 처리해야 하는지에 따라 적절한 상태 코드를 반환하면 좋습니다!** 🚀