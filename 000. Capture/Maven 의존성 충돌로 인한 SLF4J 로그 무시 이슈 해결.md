---
tistory: https://jn-silveryoung.tistory.com/58
tags:
  - publish/done
---
### 개요

프로젝트에서 SLF4J를 사용해 로그를 남기고자 했지만, 콘솔에 로그가 출력되지 않는 문제가 발생했습니다. 관련 경고 메시지를 분석하고, Maven 의존성 충돌을 해결한 과정을 정리합니다.

### 1\. 발생 문제

-   SLF4J 로그가 출력되지 않음.
-   로그를 출력하려고 했으나, 콘솔에 아래와 같은 경고 메시지가 출력됨.

```
SLF4J: No SLF4J providers were found.
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#noProviders for further details.
SLF4J: Class path contains SLF4J bindings targeting slf4j-api versions prior to 1.8.
SLF4J: Ignoring binding found at [jar:file:/C:/eGovFrameDev-3.10.0-64bit/workspace/.metadata/.plugins/org.eclipse.wst.server.core/tmp9/wtpwebapps/dataScan_web/WEB-INF/lib/log4j-slf4j-impl-2.23.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#ignoredBindings for an explanation.
Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdbc.Driver'. The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
```

-   SLF4J가 NOPLogger로 동작하여 실제 로그가 출력되지 않음
-   `log4j-slf4j-impl` 의존성이 구버전의 `slf4j-api`를 끌어와 버전 충돌 발생

### 2\. 원인 분석

-   `log4j-slf4j-impl`이 **구버전의 `slf4j-api`**를 포함하고 있어서, SLF4J 로그 구현이 제대로 동작하지 않음.
-   SLF4J 관련 경고 메시지가 출력되고, 로그가 제대로 찍히지 않음.

### 3\. 해결 방법

-   `log4j-slf4j-impl` 의존성에서 **`slf4j-api`**를 **제외(exclude)**하여 중복된 버전 충돌을 해결.
-   `pom.xml`에 다음과 같이 수정:

```
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>${org.apache.logging.log4j.version}</version>
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>  <!-- slf4j-api 중복 제거 -->
        </exclusion>
    </exclusions>
</dependency>
```

위에만 추가하면 해결될 줄 알았으나 콘솔 출력이 똑같이 안 나왔음  
그래서 의존성 tree 확인하여 SLF4J 1.7이하 버전을 의존하는 디펜던시마다 exclusions 추가해줌

#### maven 의존성 tree 확인

-   \[\[Maven Build\]\]

1.  Eclipse에서 `Run As > Maven build...`
2.  Goals에 `dependency:tree -Dincludes=org.slf4j` 입력
3.  충돌하는 SLF4J 의존성 목록 확인 후, 필요한 exclusions 적용

```
<dependency>
            <groupId>com.zaxxer</groupId>
            <artifactId>HikariCP</artifactId>
            <version>4.0.3</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-api</artifactId>
                </exclusion>
            </exclusions>
        </dependency>


        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-slf4j-impl</artifactId>
            <version>${org.apache.logging.log4j.version}</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-api</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.egovframe.rte</groupId>
            <artifactId>org.egovframe.rte.fdl.logging</artifactId>
            <version>${org.egovframe.rte.version}</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>jcl-over-slf4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>log4j-over-slf4j</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

### 4\. 결과

-   위와 같이 `pom.xml`을 수정한 후, SLF4J 로그가 정상적으로 출력되기 시작함.
-   **버전 충돌**이 해결되어, 로그 출력 문제가 해결됨.
    
    ```
    정보: Initializing Spring root WebApplicationContext
    2025-04-21 13:34:27,009 INFO [jdbc.sqlonly] SELECT ...
    ```
    

### 5\. 추가 팁

-   **드라이버 클래스 로딩**: `com.mysql.jdbc.Driver`는 deprecated이므로, `com.mysql.cj.jdbc.Driver` 사용 권장. 대부분의 JDBC 드라이버는 SPI를 통해 자동 등록되므로 수동 로딩은 불필요합니다.
-   **로깅 설정 파일**: `log4j2.xml` 또는 `logback.xml`의 위치 및 설정 확인
-   **SLF4J 코드 참조**: 경고 메시지 링크에서 자세한 원인 및 해결책 확인 가능:
    -   [http://www.slf4j.org/codes.html#noProviders](http://www.slf4j.org/codes.html#noProviders)
    -   [http://www.slf4j.org/codes.html#ignoredBindings](http://www.slf4j.org/codes.html#ignoredBindings)

