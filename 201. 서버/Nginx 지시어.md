---
tags:
  - publish/draft
---

- `location / {}` : 클라이언트가 `/` 경로로 요청하면 블록 내 설정을 실행한다.
- `proxy_pass`: 클라이언트 요청을 전달할 백엔드 서버 주소를 지정합니다. 
	- 클라이언트에서 `example.com/` 요청 시 내부의 `http://backend_server`로 전달됨
- `proxy_set_header`: 요청 헤더 정보를 백엔드 서버에 전달합니다
	- `Host $host;` : 클라이언트가 요청한 host 헤더를 유지
	- `X-Real-IP $remote_addr;` : 클라이언트의 실제 IP주소를 전달
		- 기본적으로 Nginx가 프록시 역할을 하면 백엔드에서 클라이언트의 IP를 알 수 없지만, 해당 설정을 하면 백엔드에서도 요청한 클라이언트의 IP를 알 수 있음
	- `X-Forwarded-For $proxy_add_x_forwarded_for;` : 클라이언트의 IP를 여러 개 저장하여 추적 가능하도록 함
		- 프록시 서버를 거칠 때마다 클라이언트 IP를 계속 추가함
	- `X-Forwarded-Proto $scheme;` : 클라이언트의 요청이 http인지, https인지 전달
- `proxy_redirect` : 백엔드에서 리다이렉트할 때 포트번호를 노출하지 않도록 변경함
	- 예를 들어 `example.com/` 요청 시 프록시 서버에서 `HTTP/1.1 302 Found \n Location: http://localhost:8080/login` 응답을 하였을 때,
	- Nginx가 이를 `http:example.com/login`으로 변경해서 클라이언트에게 전달



---

> 참고링크
> - [[Nginx] 엔진엑스 프록시 모듈](https://12bme.tistory.com/367)