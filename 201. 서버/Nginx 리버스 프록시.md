

Nginx 리버스 프록시는 클라이언트의 요청을 대신 받아 내부 서버에 전달하고, 내부 서버의 응답을 클라이언트에게 반환하는 역할을 합니다. 이를 통해 보안 강화, 부하 분산, 요청 처리 최적화 등의 이점을 제공합니다.

## 주요 개념

![[Pasted image 20250202152037.png]]

- **리버스 프록시란?**  
  클라이언트 요청을 직접 처리하지 않고, 중간에서 요청을 받아 내부 서버로 전달하는 서버입니다. 클라이언트는 내부 서버의 세부 정보를 알 필요 없이 리버스 프록시를 통해 간접적으로 통신합니다
- **Nginx와 리버스 프록시**  
  Nginx는 리버스 프록시로 자주 사용되는 웹 서버로, 로드 밸런싱, 캐싱, SSL 처리 등의 기능을 제공합니다. 이를 통해 성능과 보안을 동시에 향상시킬 수 있습니다

## 리버스 프록시의 주요 기능

1. **로드 밸런싱**  
   클라이언트 요청을 여러 서버에 분산하여 처리함으로써 성능과 확장성을 높입니다

2. **캐싱**  
   자주 요청되는 데이터를 캐싱하여 서버 부하를 줄이고 응답 속도를 개선합니다

3. **SSL 터미네이션**  
   클라이언트와의 SSL 연결을 처리하여 내부 서버의 부담을 줄이고 보안을 강화합니다

4. **보안 강화**  
   내부 서버의 IP와 포트를 숨기고, DDoS 공격 완화 및 요청 제한 기능을 제공합니다

5. **압축 및 최적화**  
   클라이언트로 전송하기 전 데이터를 압축하여 네트워크 효율성을 높입니다

## 설정 예시

Nginx를 리버스 프록시로 설정하려면 다음과 같은 설정 파일이 필요합니다:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend_server;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_redirect http://localhost:8080/ /;
    }
}
```

Nginx가 요청을 프록시하면 요청을 지정된 프록시 서버로 보내고 응답을 가져와 클라이언트로 다시 보냅니다. 지정된 프로토콜을 사용하여 HTTP 서버 또는 비 HTTP 서버에 요청을 프록시 할 수 있습니다.
[[Nginx 지시어]]

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

## 활용 사례

- API 게이트웨이 역할: 특정 URI에 따라 요청을 다른 백엔드 서비스로 라우팅.
- 웹 애플리케이션 보호: 내부 서버를 외부로부터 숨김으로써 보안 강화.
- 다중 서버 환경에서 부하 분산 및 고가용성 지원.

Nginx 리버스 프록시는 이러한 기능들을 통해 안정적이고 효율적인 웹 서비스 운영을 지원합니다.

---

> 참고링크
> - [Nginx를 사용한 리버시 프록시는 왜 필요할까?](https://kangmanjoo.tistory.com/69)
> - [Nginx를 리버스 프록시로 설정하고 구성하는 방법](https://blog.containerize.com/ko/how-to-setup-and-configure-nginx-as-reverse-proxy/)


---

> 활용
> - [[Nginx 리버스 프록시로 스프링부트 Swagger 문서 제공하기]]
>