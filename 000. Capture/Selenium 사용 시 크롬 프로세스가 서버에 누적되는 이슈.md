
### 🗓️ 문제 발견 

Selenium을 활용한 크롤링 서비스를 리눅스 서버에서 장시간 운영하던 중, 주기적으로 ChromeDriver 세션을 종료하고 새로 생성함에도 불구하고 **크롬 프로세스가 계속해서 서버에 남아 쌓이는 현상**을 발견하게 되었습니다.

서버의 메모리 사용률이 비정상적으로 증가했고, `ps -ef`로 확인해보니 크롬 및 크롬드라이버 프로세스가 수십 개 이상 살아 있는 상태였습니다.

![[크롬 프로세스.png]]

---

### 🔍 기본 조건 점검

먼저 일반적으로 발생할 수 있는 실수를 점검해보았습니다.
1) `driver.quit()` 이 존재하는 로직을 제대로 안타고 있는지?
	➡️ 정상적으로 세션 종료 주기마다 quit()을 호출하고 드라이버의 세션ID도 초기화 됨
2) driver 옵션에 안정성 옵션 적용이 누락되었는지?
	➡️ `--no-sandbox`, `--disable-dev-shm-usage`, `--headless` 등 안정성 옵션 적용됨
    

하지만 이 조건들이 모두 충족된 상태에서도 **프로세스 누적 현상**이 발생하고 있었습니다.

---

### ⚠️ 원인 분석

문제의 원인을 보다 정밀하게 파악하기 위해 프로세스 상태를 조사했습니다.

```bash
ps -ef | grep chrome | awk '$3 == 1'
```

해당 명령어는 **부모 프로세스 ID(PPID)가 1**인, 즉 **init(systemd)의 자식 프로세스로 남아 있는 고아 프로세스**를 필터링합니다.  
이를 통해 `chrome`, `chromedriver`와 관련된 **정상적으로 종료되지 않은 프로세스들이 고아 상태로 남아 있다는 사실**을 확인할 수 있었습니다.

이는 Selenium이 내부적으로 생성하는 여러 하위 프로세스(e.g., crashpad_handler, zygote, utility 등)가 `driver.quit()` 이후에도 **정상적으로 종료되지 않거나**, ChromeDriver의 **세션 종료 타이밍 이슈**로 인해 발생하는 것으로 추정됩니다.

---

### 🛠 해결 방법

#### 1. 고아 프로세스 수동 정리 명령어

```bash
ps -ef | grep chrome | grep -v grep | awk '$3 == 1 {print $2}' | xargs kill -9
```

해당 명령어는:

- `chrome`이 포함된 프로세스 중
    
- 부모 PID가 1인 고아 프로세스만 필터링한 후
    
- `kill -9`로 강제 종료합니다.
    

#### 2. Java에서 자동화 처리

```java
public void killOrphanChromeProcesses(String keyword) {
    try {
        String cmd = "ps -ef | grep " + keyword + " | grep -v grep | awk '$3 == 1 {print $2}' | xargs kill -9";
        Runtime.getRuntime().exec(new String[]{"bash", "-c", cmd});
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

```java
// 예시 사용
killOrphanChromeProcesses("chrome");
killOrphanChromeProcesses("chromedriver");
```

#### 3. 주기적인 자동 실행 (cron 추천)

```bash
*/30 * * * * /usr/bin/bash -c 'ps -ef | grep chrome | grep -v grep | awk "$3 == 1 {print \$2}" | xargs kill -9'
```

→ 30분마다 고아 상태의 크롬 프로세스를 정리하여 메모리 누수를 방지합니다.

---

### 🧩 추가 점검 사항

- ChromeDriver와 실제 설치된 Chrome 버전이 정확히 일치하는지 확인
    
    ```bash
    google-chrome --version
    chromedriver --version
    ```
    
- 필요 시 로그로 남은 PID를 기록하고 모니터링 도입 (Prometheus, Grafana 등)
    

---

### ✅ 마무리

`driver.quit()` 호출만으로는 완전한 프로세스 정리가 보장되지 않는 경우가 있습니다. 특히 크롬의 구조상 여러 하위 프로세스가 분리되어 실행되기 때문에, **고아 상태로 남은 프로세스를 주기적으로 감시하고 정리하는 루틴이 반드시 필요**합니다.

> Selenium을 사용하는 모든 리눅스 기반 서비스 운영자라면, 고아 프로세스 정리 여부를 점검하고, 자동화된 클린업 정책을 마련하는 것을 강력히 추천드립니다.

---

### 📌 관련 키워드

`#selenium` `#chromedriver` `#리눅스프로세스` `#크롬고아프로세스` `#크롤링서버운영` `#프로세스정리` `#driver.quit` `#크롬메모리누수`

---

필요하시면 블로그 스타일에 맞춰 제목, 태그, 코드 강조 등을 조정해드릴 수 있어요. `Notion`, `Velog`, `Tistory`, `GitHub Blog`용 포맷으로도 변환 가능합니다.