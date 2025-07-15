
---

## ✅ `ps -ef` 출력 컬럼 설명

예시 출력:

```bash
UID        PID   PPID  C STIME     TTY          TIME CMD
root     12345      1  0 10:35     ?        00:00:01 /opt/google/chrome/chrome --headless ...
```

|컬럼명|의미|
|---|---|
|`UID`|프로세스를 실행한 사용자|
|`PID`|프로세스 ID|
|`PPID`|부모 프로세스 ID|
|`C`|CPU 사용률|
|`STIME`|**프로세스 시작 시간** (`HH:MM` 또는 `MMMDD` 형식)|
|`TTY`|연결된 터미널 (없으면 `?`)|
|`TIME`|누적 CPU 사용 시간|
|`CMD`|실행 명령어 전체|

---

## 📌 `STIME` 값의 표시 형식

- **당일 시작된 프로세스**: `HH:MM` (예: `10:35`)
    
- **이전 날짜에 시작된 프로세스**: `MMMdd` (예: `Jul14`)
    

> 즉, 오래된 프로세스는 날짜로, 당일 것은 시간으로 표시됩니다.

---

## ✅ 오래된 Chrome 프로세스만 필터링하는 팁

예: 오늘보다 오래된 프로세스를 걸러내고 싶다면 `ps` 출력의 `STIME`을 활용해 **스크립트로 파싱**할 수 있습니다. 다만 `ps`의 `etime`, `lstart` 옵션을 쓰면 더 정밀하게 시간 조건을 줄 수 있습니다.

---

## ✅ 정밀 시간 확인 (초 단위로 보기)

```bash
ps -eo pid,lstart,cmd | grep chrome
```

출력 예:

```
12345 Mon Jul 15 10:35:02 2025 /opt/google/chrome/chrome --headless ...
```

| `lstart` | 프로세스 정확한 시작 날짜+시간 (초 단위) |

---

필요하면 `시작된 지 30분이 지난 크롬 프로세스만 종료`하는 스크립트도 도와드릴 수 있습니다.