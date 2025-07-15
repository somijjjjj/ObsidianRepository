
`pkill -f {프로세스명}` 명령어는 **실행 중인 프로세스 중에서 커맨드 라인에 `"프로세스명"`이 포함된 모든 프로세스를 강제 종료**하는 명령입니다. 

---

### ✅ `pkill` 이란?

- `kill`은 특정 PID를 지정해서 종료하는 명령이고,
    
- `pkill`은 **프로세스 이름이나 패턴으로 종료할 수 있는 명령**입니다.
    

---

### ✅ 옵션 설명: `-f`

- `-f` 옵션은 **"전체 커맨드 라인(명령어 전체 문자열)"에 대해 검색**합니다.
    
- 기본적으로 `pkill chrome`은 **실행 파일명만** 검사합니다 (예: `chrome`).
    
- 하지만 `pkill -f chrome`은 아래처럼 전체 명령어를 다 확인합니다:
    
    ```bash
    /opt/google/chrome/chrome --headless --disable-gpu ...
    ```
    
    위처럼 실행된 프로세스도 `-f` 덕분에 감지됩니다.
    

---

### ✅ 예시

```bash
$ ps -ef | grep chrome
user    1234   1  /opt/google/chrome/chrome --headless --disable-gpu ...
user    5678   1  chromedriver --port=9515
```

```bash
$ pkill -f chrome
```

→ 위의 두 프로세스 중 `"chrome"`이 포함된 프로세스는 전부 종료됩니다.

---

### ✅ 주의 사항

- **필터가 모호하면 과도하게 종료될 수 있습니다.**
    
    - 예: `pkill -f java` → 자바 기반 모든 프로세스를 죽일 수 있음
        
- 크롬 관련된 것만 죽이고 싶다면:
    
    ```bash
    pkill -f "/opt/google/chrome/chrome"
    ```
    
