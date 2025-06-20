
### 📌 Linux 서버 시간 설정 및 변경 가이드

리눅스 서버의 시스템 시간(Time), 타임존(Timezone), RTC(Real-Time Clock) 설정 방법을 정리한 가이드입니다.

---

#### 🕓 1. 현재 시간 및 타임존 확인

```bash
timedatectl
```

예시 출력:

```
Local time: Mon 2024-10-07 15:03:41 KST
Universal time: Mon 2024-10-07 06:03:41 UTC
RTC time: Mon 2024-10-07 06:03:41
Time zone: Asia/Seoul (KST, +0900)
System clock synchronized: yes
NTP service: active
RTC in local TZ: no
```

시스템 시간만 간단히 확인하려면:

```bash
date
```

---

#### 🌏 2. 타임존 변경

```bash
# Asia/Seoul로 타임존 설정
sudo timedatectl set-timezone Asia/Seoul
```

타임존 파일 위치 확인:

```bash
ls /usr/share/zoneinfo/Asia
```

---

#### 🔁 3. /etc/localtime 심볼릭 링크 재설정

```bash
sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```

※ 일반적으로 `set-timezone` 명령어로 자동 처리되지만, 수동으로 설정할 경우 사용합니다.

---

#### ⏱ 4. RTC (Real Time Clock) 설정

- **RTC를 시스템 로컬 시간 기준으로 사용하려면:**
    
    ```bash
    sudo timedatectl set-local-rtc 1
    ```
    
- **RTC를 UTC 기준으로 변경하려면 (권장):**
    
    ```bash
    sudo timedatectl set-local-rtc 0
    ```
    

---

#### 🔗 참고 링크

- [Colt 블로그 - 시간/날짜 설정 timedatectl](https://colt357.tistory.com/entry/%EC%8B%9C%EA%B0%84%EB%82%A0%EC%A7%9C-%EC%84%A4%EC%A0%95-timedatectl)
