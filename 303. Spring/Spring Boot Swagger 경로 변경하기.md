Spring Boot에서 Swagger 문서를 `/api/docs` 경로로 변경하려면, Swagger 설정을 수정해야 합니다.

Spring Boot에서는 **Springdoc OpenAPI**를 사용하여 Swagger UI를 설정할 수 있습니다.  
Spring Boot 프로젝트에서 Swagger UI를 `/api/docs` 경로에서 접근할 수 있도록 설정하는 방법을 설명할게요.

---

**🔹 `application.yml` 사용 시**

```yaml
springdoc:
  swagger-ui:
    path: /api/docs
  api-docs:
    path: /api/v3/api-docs
```

**🔹 `application.properties` 사용 시**

```properties
springdoc.swagger-ui.path=/api/docs
springdoc.api-docs.path=/api/v3/api-docs
```

