
https://velog.io/@onestone/%EC%98%A4%ED%94%88%EC%86%8C%EC%8A%A4-APM-Scouter-Paper-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-%EC%84%A4%EC%B9%98


### 다운로드

https://github.com/scouter-project/scouter/releases/
- Scouter 프로젝트
https://github.com/scouter-project/scouter/blob/master/scouter.document/main/Setup_kr.md
- 설치 방법


https://github.com/scouter-contrib/scouter-paper/releases
- Scouter Paper 프로젝트

![[Pasted image 20250406104522.png]]
### ubuntu 서버에 설치

OS : Linux
Scouter 버전 : 2.6.4


1. 설치파일을 다운로드할 경로로 이동

2. scouter 패키지 다운로드
```bash
wget wget https://github.com/scouter-project/scouter/releases/download/v2.20.0/scouter-all-2.20.0.tar.gz
```

3. tar 파일 압축해제
```bash
tar -xvf scouter
```

4. scouter 폴더 파일 정보

- server(collector) : Agent가 전송한 데이터 수집/처리
- agent.host : OS의 CPU, Memory, Disk 등의 성능 정보 전송
- agent.java : 실시간 서비스 성능 정보, Heap Memory, Thread 등 Java 성능 정보
- agent.batch : 배치 잡 모니터링
- webapp : rest api 독립 서버, server에 embedded로 실행 가능

```

drwxr-xr-x 7 root root 4096 May 29  2023 ./
drwxr-xr-x 4 root root 4096 Apr  5 19:09 ../
drwxr-xr-x 3 root root 4096 Apr  5 19:09 agent.batch/
drwxr-xr-x 5 root root 4096 Apr  6 01:26 agent.host/
drwxr-xr-x 6 root root 4096 Apr  5 22:00 agent.java/
drwxr-xr-x 8 root root 4096 Apr  6 01:26 server/
drwxr-xr-x 5 root root 4096 Apr  5 19:09 webapp/

root@s2it-server:/insider/apm/scouter# chmod -R 775 *

root@s2it-server:/insider/apm/scouter# ll
total 28
drwxr-xr-x 7 root root 4096 May 29  2023 ./
drwxr-xr-x 4 root root 4096 Apr  5 19:09 ../
drwxrwxr-x 3 root root 4096 Apr  5 19:09 agent.batch/
drwxrwxr-x 5 root root 4096 Apr  6 01:26 agent.host/
drwxrwxr-x 6 root root 4096 Apr  5 22:00 agent.java/
drwxrwxr-x 8 root root 4096 Apr  6 01:26 server/
drwxrwxr-x 5 root root 4096 Apr  5 19:09 webapp/

```
- 권한 부여


4. /server/startup.sh 실행
```bash
/server/startup.sh
```

5-1. `com.sun.xml.bind.v2.runtime.reflect.opt.Injector.defineClass" is null` 오류 발생

- java 9이상 모듈 시스템으로 인해 java.lang 등 내부 API 접근이 제한되면서 발생하는 현상이여서 JVM 옵션을 추가해줘야함

5-2. startup.sh 파일 수정
```bash
vim scouter/server/startup.sh
```

```bash
#!/usr/bin/env bash

nohup java \
--add-opens=java.base/java.lang=ALL-UNNAMED \
--add-exports=java.base/sun.net=ALL-UNNAMED \
-Djdk.attach.allowAttachSelf=true \
 -Xmx1024m \
 -classpath ./scouter-server-boot.jar scouter.boot.Boot ./lib > nohup.out &
sleep 1
tail -100 nohup.out
```

| 옵션                                | 설명                                       |
|-----------------------------------|------------------------------------------|
| --add-opens                       | Java 11+의 모듈 보안 정책 우회 (Script plugin 지원) |
| --add-exports                     | 내부 패키지 API 접근 허용                         |
| -Djdk.attach.allowAttachSelf=true | 자기 자신 프로세스에 attach 허용 (스레드 덤프 등)         |
| -Xmx1024m                         | 최대 힙 메모리 설정 (필요 시 조정 가능)                 |

5-3. 저장 후 다시 서버 실행

```bash
root@s2it-server:/insider/apm/scouter/server# ./startup.sh
nohup: redirecting stderr to stdout
  ____                  _
 / ___|  ___ ___  _   _| |_ ___ _ __
 \___ \ / __/   \| | | | __/ _ \ '__|
  ___) | (_| (+) | |_| | ||  __/ |
 |____/ \___\___/ \__,_|\__\___|_|
 Open Source S/W Performance Monitoring
 Scouter version 2.20.0
```

- config 파일 수정
```bash
vim server/conf/scouter.conf
```
```bash

# Server
server_id=inssider-collector-server

# Agent Control and Service Port(Default : TCP 6100)
net_tcp_listen_port=6100

# UDP Receive Port(Default : 6100)
net_udp_listen_port=6100

# DB directory(Default : ./database)
db_dir=/insider/apm/scouter/server/database

# Log directory(Default : ./logs)
log_dir=/insider/apm/scouter/server/logs


# Log
 log_rotation_enabled=true

 # Compress
 compress_xlog_enabled=true
 compress_profile_enabled=true

 # Manager
 mgr_purge_enabled=false
~
~
~

```

|설정 항목|설명|현재 설정|문제 없음|
|---|---|---|---|
|`net_tcp_listen_port`|Agent와의 TCP 통신 포트|`6100`|✅|
|`net_udp_listen_port`|UDP 포트 (일부 메트릭에 사용)|`6100`|✅|
|`db_dir`|수집된 데이터 저장 디렉토리|`./database`|✅|
|`log_dir`|로그 저장 디렉토리|`./logs`|✅|


- 6100 포트 허용
```
sudo ufw allow 6100/tcp    # TCP 연결 허용 (주로 이게 중요)
sudo ufw allow 6100/udp    # UDP 연결 허용 (Scouter는 일부 기능에서 사용 가능)
```

###  호스트 에이전트

해당 호스트(이 Agent가 설치된 운영체제 환경)에서의 메모리 사용량 등을 조회한다.

- 호스트 에이전트 conf 파일 수정
```bash
vim /scouter/agent.host/conf/scouter.conf
```
```bash
### scouter host configruation sample
obj_name=HOST-01
net_collector_ip=144.24.82.57
net_collector_udp_port=6100
net_collector_tcp_port=6100

# Alert
disk_alert_enabled=true
disk_warning_pct=80
disk_fatal_pct=90

cpu_alert_enabled=true
cpu_warning_pct=80
cpu_fatal_pct=90

mem_alert_enabled=true
mem_warning_pct=80
mem_fatal_pct=90

cpu_warning_pct=80
cpu_fatal_pct=85
cpu_check_period_ms=60000
cpu_fatal_history=3
cpu_alert_interval_ms=300000

disk_warning_pct=88
disk_fatal_pct=92

```

- 호스트 에이전트 시작
```bash
cd ./scouter/agent.host
./host.sh
```

- 호스트 에이전트 bat으로 실행하니까 실행됏음.
```

root@s2it-server:/insider/apm/scouter/agent.host# ./host.bat
  ____                  _
 / ___|  ___ ___  _   _| |_ ___ _ __
 \___ \ / __/   \| | | | __/ _ \ '__|
  ___) | (_| (+) | |_| | ||  __/ |
 |____/ \___\___/ \__,_|\__\___|_|
 Open Source S/W Performance Monitoring
 Scouter version 2.20.0

SLF4J: No SLF4J providers were found.
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See https://www.slf4j.org/codes.html#noProviders for further details.
Configure -Dscouter.config=./conf/scouter.conf
Scouter Host Agent Version 2.20.0 2023-05-29 05:14 GMT
System JRE version : 17.0.14

```
### 자바 에이전트

Host Agent와 별개로, java API를 사용하여 해당 자바를 사용하는(주로 Tomcat) 프로세스의 수치를 파악할 수 있다.

- 에이전트 파일 수정
```bash
vim /scouter/agent.java/conf/scouter.conf
```

```bash
### scouter java agent configuration sample
obj_name=WAS-01
obj_host_name=inssider-api-server
net_collector_ip=144.24.82.57
net_collector_udp_port=6100
net_collector_tcp_port=6100
hook_method_patterns=sample.mybiz.*Biz.*,sample.service.*Service.*
trace_http_client_ip_header_key=X-Forwarded-For
profile_spring_controller_method_parameter_enabled=false
hook_exception_class_patterns=my.exception.TypedException
profile_fullstack_hooked_exception_enabled=true
hook_exception_handler_method_patterns=my.AbstractAPIController.fallbackHandler,my.ApiExceptionLoggingFilter.handleNotFoundErrorResponse
hook_exception_hanlder_exclude_class_patterns=exception.BizException


```

/insider/apm/scouter/agent.java/logs/

```bash
mkdir -p /insider/apm/scouter/agent.java/logs
chmod 755 /insider/apm/scouter/agent.java/logs
```

❗ Agent는 로그 디렉토리를 직접 생성하지 않기 때문에 미리 만들어줘야 해요.


- 기존 실행하던 jar 실행 옵션 수정
```bash
AGENT_PATH=/insider/apm/scouter/agent.java

JAR_FILE=$(ls -t insider-0.0.1-SNAPSHOT.jar | head -n 1)

# 1번째 수정 -> agent.java 에 log 안생김
nohup java \
-javaagent:$AGENT_PATH/scouter.agent.jar \
-Dscouter.config=$AGENT_PATH/conf/scouter.conf \
-Djasypt.encryptor.password=JASYPT_INSIDER \
-jar "$JAR_FILE" > nohup.out 2>&1 &

# 2번째 수정
nohup java \
--add-opens java.base/java.lang=ALL-UNNAMED \
--add-exports java.base/sun.net=ALL-UNNAMED \
-Djdk.attach.allowAttachSelf=true \
-javaagent:$AGENT_HOME/scouter.agent.jar \
-Dscouter.config=$AGENT_HOME/conf/scouter.conf \
-Djasypt.encryptor.password=JASYPT_INSIDER \
-jar insider-0.0.1-SNAPSHOT.jar > nohup.out 2>&1 &
```

| 옵션                              | 설명                            |
|---------------------------------|-------------------------------|
| -javaagent=...                  | Scouter Java Agent를 JVM에 붙임   |
| -Dscouter.config=...            | 설정파일 경로 직접 지정 (생략 시 기본 경로 탐색) |
| -Djasypt.encryptor.password=... | 원래 쓰시던 JASYPT 설정 유지           |
| nohup ... &                     | 백그라운드에서 실행, 로그는 nohup.out에 저장 |

- 서버 로그 확인
```bash
 vim server-20250405.log
```

 ✅ 핵심 확인 포인트

```bash
20250405 19:49:48 [S104] New OBJECT  objType=linux objHash=z1s5d7rj objName=/s2it-server ...
```


✔️ **Host Agent**가 정상적으로 Scouter Server에 연결됨  
→ `objName=/s2it-server` 라고 등록된 리눅스 호스트가 Scouter Client에서 보일 거예요.

---

 ⚠️ 이건 약간의 경고지만 무시 가능

```bash
20250405 19:49:51 [A1601] error on toClass with javassist. try to fallback for java8 below. err:...
```


✔️ 이건 특정 **플러그인 클래스 로딩** 과정에서 Java 11 이상 버전의 **모듈 시스템 제약** 때문에 생긴 경고예요.  
→ **기능엔 큰 문제 없음**, fallback 처리되어서 계속 실행됩니다.

※ 이건 아래 옵션을 Scouter Server의 `startup.sh`에 JVM 옵션으로 추가하면 없어질 수 있어요:
```bash
--add-opens java.base/java.lang=ALL-UNNAMED \
--add-exports java.base/sun.net=ALL-UNNAMED \
-Djdk.attach.allowAttachSelf=true
```

---

## ✅ Plugin들도 정상적으로 로딩 완료

```bash
PLUG-IN : scouter.server.plugin.IAlert loaded PLUG-IN : scouter.server.plugin.ICounter loaded ...
```


✔️ Scouter Server에 내장된 플러그인들이 전부 정상적으로 로딩됨  
→ 알림, XLog, Summary 등 모든 기능이 준비 완료된 상태입니다.


| 항목                       | 상태                           |
| ------------------------ | ---------------------------- |
| ✅ Scouter Server 설치 및 실행 | 완료                           |
| ✅ Host Agent 연결          | 완료 (`/s2it-server` 이름으로 확인됨) |
| ❓ Java Agent 연결          | 아직 로그에 안 보임                  |
|                          |                              |

### 클라이언트

- windows 버전으로 클라이언트 다운로드 파일 설치
- `scouter.ini` 파일 내 vm jvm 경로 추가해주어야함

```
-startup
plugins/org.eclipse.equinox.launcher_1.6.400.v20210924-0641.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.win32.win32.x86_64_1.2.400.v20211117-0650
-data
@user.home/scouter
-vm
C:\Program Files\Java\openlogic-openjdk-11.0.21+9-windows-x64\bin\javaw.exe
-vmargs
-Xms128m
-Xmx1024m
-XX:+UseG1GC
-Dosgi.requiredJavaVersion=11

```

- 실행파일 실행
- 초기 계정 admin/admin 

2. scouter 패키지 다운로드
```bash
wget https://github.com/scouter-contrib/scouter-paper/archive/refs/tags/2.6.4.tar.gz
```

3. tar파일 압축 해제
```bash
 tar -xvf 2.6.4.tar.gz
```

![[Pasted image 20250406105338.png]]
```
 cp -r * /insider/apm/scouter/server/extweb/

```
- 해당 경로로 복사

```
/insider/apm/scouter/webapp/conf/scouter.conf

```
- conf파일 수정

