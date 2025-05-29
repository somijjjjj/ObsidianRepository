

#### `/etc/nginx/sites-available/default`설정

```
server {

    listen 80;
    listen [::]:80;

    server_name inssider.kro.kr;


    location / {
            root /insider/web/dist;
            index index.html;
            try_files $uri $uri/ /index.html;
    }

    location /api/ {
           proxy_pass http://localhost:8080;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
    }


    location /api-docs {
           proxy_pass http://localhost:8080/;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
           proxy_redirect off;
    }

    location /swagger-ui/ {
        proxy_pass http://localhost:8080/swagger-ui/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        location ~ ^/swagger-ui/(.+\.html)$ {
                proxy_pass http://localhost:8080/swagger-ui/$1;
        }

        location ~ ^/swagger-ui/(.+\.css)$ {
                proxy_pass http://localhost:8080/swagger-ui/$1;
        }

        location ~ ^/swagger-ui/(.+\.js)$ {
                proxy_pass http://localhost:8080/swagger-ui/$1;
        }

        location ~ ^/swagger-ui/(.+\.png)$ {
                proxy_pass http://localhost:8080/swagger-ui/$1;
        }
    }

    location /v3/api-docs/{
        proxy_pass http://localhost:8080/v3/api-docs/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_redirect off;

        location ~ ^/v3/api-docs/*$ {
                proxy_pass http://localhost:8080/v3/api-docs/$1;
                proxy_redirect off;
        }
    }

}
```



#### 흐름도

다음은 Nginx 설정을 기반으로 한 요청 흐름도입니다.

##### **설정 요약**

- **80 포트**에서 요청을 수신함.
- `/` → 정적 파일(`index.html`)을 제공.
- `/api/` → 백엔드(Spring Boot, `localhost:8080`)로 프록시.
- `/api-docs`, `/swagger-ui/`, `/v3/api-docs/` → Swagger 문서를 위한 프록시.

---

##### **요청 흐름도**

```plaintext
사용자 요청
     │
     ├── `/`  → 정적 파일 제공 (/insider/web/dist/index.html)
     │
     ├── `/api/`  → 백엔드 서버 (http://localhost:8080)로 프록시
     │
     ├── `/api-docs`  → 백엔드 서버 (http://localhost:8080)로 프록시
     │
     ├── `/swagger-ui/`  
     │       ├── HTML 파일 (swagger-ui/*.html)
     │       ├── CSS 파일 (swagger-ui/*.css)
     │       ├── JS 파일 (swagger-ui/*.js)
     │       ├── PNG 파일 (swagger-ui/*.png)
     │
     ├── `/v3/api-docs/`  → 백엔드 서버 (http://localhost:8080/v3/api-docs/)로 프록시
     │       ├── API 문서 JSON 반환
     │       ├── proxy_redirect off 설정 (경로 유지)
```

---

##### **비주얼 흐름도 (다이어그램)**

```plaintext
+-------------+               +---------------------+
|   Client    |               |    Nginx Server    |
+-------------+               +---------------------+
        |                                 |
        |   1. HTTP 요청                  |
        |-------------------------------->|
        |                                 |
        |   2. Nginx에서 라우팅 처리        |
        |   ├── `/` → 정적 파일 제공        |
        |   ├── `/api/` → 백엔드 프록시     |
        |   ├── `/api-docs` → 백엔드 프록시 |
        |   ├── `/swagger-ui/` → 프록시    |
        |   ├── `/v3/api-docs/` → 프록시   |
        |                                 |
        |   3. 백엔드(Spring Boot) 응답    |
        |<--------------------------------|
```

---

**설명**

- 클라이언트가 요청을 보냄.
- Nginx가 요청 경로에 따라 처리:
    - 정적 파일 (`/`)은 `root` 디렉토리에서 제공.
    - `/api/`는 백엔드(Spring Boot)로 전달.
    - Swagger 관련 요청(`/swagger-ui/`, `/api-docs`, `/v3/api-docs/`)도 백엔드로 전달.
- Spring Boot가 응답을 보내고, Nginx가 이를 사용자에게 반환.



---
### 네트워크 요청 분석

![[Pasted image 20250202204936.png]]


#### [springdoc 공식문서](https://springdoc.org/#how-can-i-deploy-springdoc-openapi-starter-webmvc-ui-behind-a-reverse-proxy)

#### **SpringDoc OpenAPI를 리버스 프록시 뒤에서 배포하는 방법**

애플리케이션이 **리버스 프록시, 로드 밸런서, 또는 클라우드 환경에서 실행**될 경우, **호스트(host), 포트(port), 스키마(scheme)** 등의 요청 정보가 변경될 수 있습니다.

예를 들어, **애플리케이션이 내부적으로 `10.10.10.10:8080`에서 실행되더라도,** 클라이언트는 `example.org`에서 서비스가 제공되는 것처럼 보여야 합니다.

---

#### **1. Forwarded 헤더 설정 필요**

[RFC7239 "Forwarded Headers"](https://datatracker.ietf.org/doc/html/rfc7239)는 **리버스 프록시가 원본 요청 정보를 전달하는 표준 헤더**를 정의합니다.

프록시는 다음과 같은 **비표준 헤더**도 추가할 수 있습니다.

- `X-Forwarded-Host`
- `X-Forwarded-Port`
- `X-Forwarded-Proto`
- `X-Forwarded-Ssl`
- `X-Forwarded-Prefix`

이 중 **`X-Forwarded-For`, `X-Forwarded-Proto`** 헤더가 추가되면, **Spring Boot 설정에서 `server.forward-headers-strategy=NATIVE`** 옵션을 사용하면 자동으로 적용됩니다.

---

#### **2. 리버스 프록시에서 `X-Forwarded-Prefix` 헤더 추가**

Swagger UI를 정상적으로 표시하려면 리버스 프록시에서 **`X-Forwarded-Prefix`** 헤더를 설정해야 합니다.

**Apache 2 예제**

```apache
RequestHeader set X-Forwarded-Prefix "/custom-path"
```

**Nginx 예제**

```nginx
proxy_set_header X-Forwarded-Prefix "/custom-path";
```

---

#### **3. Spring Boot에서 헤더 처리 방법**

리버스 프록시를 사용할 때, `X-Forwarded-For` 헤더를 처리하는 방법은 **두 가지**가 있습니다.

##### **① `server.use-forward-headers=true` 설정**

`application.properties` 또는 `application.yml`에서 아래 설정 추가:

```properties
server.use-forward-headers=true
```

##### **② `server.forward-headers-strategy=framework` 설정**

Spring Boot 2.2 이상에서는 **`server.forward-headers-strategy` 옵션을 `framework`로 설정**할 수도 있습니다.

```properties
server.forward-headers-strategy=framework
```

이 옵션을 사용하면, **Spring이 직접 `ForwardedHeaderFilter`를 통해 헤더를 처리**합니다.

---

#### **4. `ForwardedHeaderFilter`를 직접 등록하는 방법**

필요한 경우, `ForwardedHeaderFilter`를 직접 Bean으로 등록할 수도 있습니다.

```java
@Bean
ForwardedHeaderFilter forwardedHeaderFilter() {
   return new ForwardedHeaderFilter();
}
```

---

#### **5. Swagger UI의 Base URL 직접 조정하는 방법**

리버스 프록시 환경에서는 **Swagger UI가 잘못된 URL을 표시할 수 있음**.  
이 경우, `ServerBaseUrlCustomizer` 인터페이스를 구현하여 Swagger UI의 Base URL을 수정할 수 있습니다.

```java
@Bean
public class CustomServerBaseUrlCustomizer implements ServerBaseUrlCustomizer {
    @Override
    public String customize(String serverBaseUrl) {
        try {
            URL url = new URL(serverBaseUrl);
            if (url.getHost().contains(".com")) {
                serverBaseUrl = new URL(url.getProtocol(), url.getHost(), url.getFile()).toString();
            }
        } catch (MalformedURLException ex) {
            // 예외 처리 불가능한 경우
        }

        return serverBaseUrl;
    }
}
```

**이 코드의 역할**

- URL의 호스트 이름이 `.com`을 포함하고 있으면 **포트 번호를 제거**한 URL로 변경.

---

#### **📌 결론**

1. **Nginx 또는 Apache에서 `X-Forwarded-Prefix` 헤더 설정**
2. **Spring Boot에서 `server.forward-headers-strategy=framework` 또는 `server.use-forward-headers=true` 설정**
3. **Swagger UI에서 잘못된 URL이 표시될 경우 `ServerBaseUrlCustomizer` 사용**

이렇게 하면 **Spring Boot + SpringDoc OpenAPI + 리버스 프록시 환경에서 Swagger UI를 정상적으로 표시할 수 있음**! 🚀




---

> 참고링크
> - [[Swagger] Reverse Proxy 환경의 Swagger3 적용 (MIME, CSS, JS 404 Error)](https://velog.io/@hanqjun2660/Swagger-Reverse-Proxy-%ED%99%98%EA%B2%BD%EC%9D%98-Swagger3-%EC%A0%81%EC%9A%A9-xvw3o8cw)
> - [[Spring-Boot] Swagger(OpenAPI) - Failed to load remote configuration (Reverse-Proxy)](https://ssnotebook.tistory.com/entry/Spring-Boot-SwaggerOpenAPI-Failed-to-load-remote-configuration-Reverse-Proxy)

