
포트번호는 서버에서 특정 애플리케이션이 동작하는 네트워크 엔드포인트를 나타낸다.

### 대표적인 기본 포트 번호

- HTTP 프로토콜 `80`
- HTTPS 프로토콜 `443`

### 1999년 6월 RFC2616

> [RFC2616](https://www.rfc-editor.org/rfc/rfc2616.txt) 3.2.2 절 http URL
> - If the port is empty or not given, port 80 is assumed. 만약 포트가 비어 있거나 없으면 포트 80으로 간주한다.
- Hypertext Transfer Protocol - HTTP/1.1에 명시된 내용에 따르면, 80포트가 기본 포트로 명시되어 있다. 

### 2015년 5월 RFC7540

> [RFC7540](https://datatracker.ietf.org/doc/html/rfc7540) 3절 Starting HTTP/2
> - HTTP/2 shares the same default port numbers: 80 for "http" URIs and 443 for "https" URIs.  HTTP/2는 동일한 기본 포트 넘버를 공유한다: "http" URI는 80, "https" URI는 443.

- RFC 1700 이전까지는 빈 포트 번호였다.
- Kipp E.B. Hickman의 요청으로 1994년 10월에 RFC 1700 문서에 `443`이 추가되었다.
- `443`인 이유는 빈 칸에 순서대로 배정하다 보니 그렇게 된 것 같다.
---

> 참고링크
> - https://johngrib.github.io/wiki/why-http-80-https-443/