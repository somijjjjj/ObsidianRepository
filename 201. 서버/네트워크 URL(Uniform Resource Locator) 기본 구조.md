



우리가 접속하는 **URL(Uniform Resource Locator)** 에는 일정한 **약속된 규칙**이 있다. 
URL은 **웹 상에서 특정 리소스를 식별**하는 주소이며, 다음과 같은 형식으로 구성된다.

- [[네트워크 호스트(Host)]]
- [[네트워크 도메인 구조]]

---

## 📌 **URL 기본 구조**

```
프로토콜://호스트(IP주소 또는 도메인):포트/경로(리소스)?쿼리스트링#프래그먼트
```

예제:

```
https://www.example.com:8080/path/to/resource?key=value#section1
```

---

## 🔹 **각 부분 설명**

1. **프로토콜 (Protocol)**
    - 데이터를 주고받는 통신 방식
    - **예제:** `http://`, `https://`, `ftp://`, `ws://`
    - 일반적으로 웹에서는 `http`(비보안) 또는 `https`(보안) 사용
2. **호스트 또는 도메인 (Domain)**
    - 서버의 주소 (IP 주소 또는 도메인 이름)
    - **예제:** `www.example.com`, `127.0.0.1`
    - 브라우저에서 `example.com`을 입력하면 DNS가 이를 실제 IP로 변환함.
3. **포트번호 (Port Number)** _(생략 가능)_
    - 서버에서 특정 애플리케이션이 동작하는 네트워크 엔드포인트
    - **예제:** `:80`, `:443`, `:8080`
    - 기본 포트인 경우 생략 가능
        - `http://example.com` → `http://example.com:80`
        - `https://example.com` → `https://example.com:443`
4. **경로 (Path) 또는 리소스 위치**
    - 서버 내 특정 파일이나 리소스를 가리킴
    - **예제:** `/products`, `/images/photo.jpg`
    - `/`로 계층 구조를 나타냄 (`/path/to/resource`)
5. **쿼리 스트링 (Query String)** _(옵션)_
    - `?` 뒤에 `key=value` 형태로 여러 개의 값을 전달
    - 여러 개의 값은 `&`로 구분
    - **예제:** `?id=123&name=john`
    - RESTful API에서는 URL 경로로 자원을 구별하는 것이 일반적이지만, 검색이나 필터링에는 쿼리 스트링을 사용
6. **프래그먼트 (Fragment, 해시, 앵커)** _(옵션)_
    - `#` 뒤에 특정 페이지의 위치를 가리킴 (클라이언트 측에서만 사용됨)
    - **예제:** `#section1` (HTML 문서 내 특정 위치 이동)
    - 서버에 전달되지 않고, 브라우저가 처리

---

## ✅ **URL 예제 분석**

|URL|설명|
|---|---|
|`https://www.google.com`|HTTPS 프로토콜 사용, 기본 포트(443)|
|`http://localhost:8080/api/users`|로컬 서버, 8080 포트에서 API 호출|
|`https://example.com/products?category=shoes&page=2`|`products` 페이지에서 `category=shoes`, `page=2` 전달|
|`https://docs.example.com#chapter3`|`docs.example.com`에서 `#chapter3`로 이동|
