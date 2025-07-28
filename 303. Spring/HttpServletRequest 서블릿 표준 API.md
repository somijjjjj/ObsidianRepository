### 개념
- **서블릿 표준 API** (`javax.servlet.http.HttpServletRequest`)
- HTTP 요청과 관련된 모든 정보를 담고 있는 객체  
  (URL, 헤더, 파라미터, 세션 등)
- Spring MVC뿐만 아니라 **모든 서블릿 기반 웹 애플리케이션**에서 사용 가능
- 즉, 톰캣, Jetty 같은 서블릿 컨테이너 위라면 어디서든 제공
### 주요 특징
- 표준 자바 EE (서블릿) 인터페이스
- GET/POST 요청 파라미터, 헤더, 세션 정보 접근 가능
- 파일 업로드(`multipart/form-data`)는 직접 처리하지 않음

### 사용 예제
```java
@RequestMapping("/example")
public String example(HttpServletRequest request) {
    String name = request.getParameter("name");
    return "hello " + name;
}
```


---

### 관련 내용

- [[MultipartHttpServletRequest HttpServletRequest 확장 인터페이스 multipart form-data 처리]]