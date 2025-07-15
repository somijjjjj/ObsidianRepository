---
tags:
  - status/done
---

### 📌 문제 배경


Selenium을 활용한 크롤링 서비스를 리눅스 서버에서 장시간 운영하던 중, 주기적으로 ChromeDriver 세션을 종료하고 새로 생성함에도 불구하고 **크롬 프로세스가 계속해서 서버에 남아 쌓이는 현상**을 발견하게 되었습니다.

특히 `/opt/google/chrome/chrome_crashpad_handler`와 같은 **하위 프로세스가 부모 프로세스 ID(PPID)가 1인 상태로 계속 누적**되어 있었고, 이로 인해 메모리 점유가 높아지고 시스템 성능 저하로 이어질 수 있었습니다.

이 문제를 인지하게 된 계기는, 수집 작업이 모두 완료된 시점에서 `ps -ef` 명령어로 프로세스를 점검하던 중 **이미 종료됐어야 할 크롬 프로세스가 여러 개 살아 있는 상태**를 발견하면서였습니다.



![[크롬 프로세스.png]]

---

### 🔍 조건 점검

먼저 일반적으로 발생할 수 있는 실수를 점검해보았습니다.
1) `driver.quit()` 이 존재하는 로직을 제대로 안타고 있는지?
	➡️ 정상적으로 세션 종료 주기마다 quit()을 호출하고 드라이버의 세션ID도 초기화 되고 있었음.
2) driver 옵션에 안정성 옵션 적용이 누락되었는지?
	➡️ `--no-sandbox`, `--disable-dev-shm-usage`, `--headless` 등 안정성을 위한 필수 옵션도적용되어 있음
    
즉, 일반적으로 고려되는 종료 누락이나 옵션 미적용은 모두 피한 상황임에도 불구하고 프로세스 누적 문제가 발생하고 있었습니다.

이로부터 Selenium 내부에서 생성되는 하위 프로세스들이 driver.quit() 이후에도 완전히 종료되지 않는 현상이 문제의 원인임을 추정할 수 있었습니다.


---

## ⚙️ 기존 로직 흐름

```text
Selenium 로직 실행
  └─ driver가 null? → driver 객체 생성, 새로운 세션 ID 생성 → 크롬 프로세스 실행
      └─ 수집기 실행
          ├─ 세션 만료 시: driver.quit(), 세션 ID null 
          └─ 예외 or 수집 종료 시: driver.quit(), 세션 ID null 
          → 크롬 프로세스 종료 여부는 모름
```

- 문제점: `driver.quit()` 호출만으로는 **하위 프로세스(crashpad 등)** 가 모두 종료되지 않을 수 있음
    
- PPID가 "1" 인 프로세스가 남아있는지 확인 명령어:
    
    ```bash
    ps -ef | grep chrome | awk '$3 == 1'
    ```
    
이 명령은 부모 프로세스 ID가 1인 크롬 관련 고아 프로세스만 필터링합니다.
이로써 chrome, chromedriver 관련 프로세스들이 종료되지 않고 남아 있는 상태를 확인할 수 있었습니다.

---

## 🔧 개선된 로직 흐름

```text
Selenium 로직 실행
  └─ driver가 null? → driver 객체 생성, 세션 ID 저장
      └─ 수집기 실행
          ├─ 세션 만료 시:
          │    ├─ PPID=1 chrome 프로세스 검색 후 종료
          │    └─ driver.quit(), 세션 ID null
          └─ 예외 or 수집 종료 시:
               ├─ PPID=1 chrome 프로세스 검색 후 종료
               └─ driver.quit(), 세션 ID null
```

- driver.quit() 이전에 PPID=1인 고아 프로세스를 탐지하고 강제 종료하는 로직을 삽입
- 이를 통해 메모리 누수 없이 크롬 프로세스까지 명확하게 정리

---

## 🧪 적용된 개선 코드 (Java)

### ✅ 1. 드라이버 옵션 설정
```java
ChromeOptions options = new ChromeOptions();
options.addArguments(arguments);
options.setExperimentalOption("detach", false); // 프로세스가 백그라운드에 남지않도록
```
드라이버 실행 옵션 중 driver.quit() 호출 시 브라우저 창과 프로세스가 함께 종료되는 설정값을 지정.
해당 옵션을 명시하지 않더라도 기본값이 false이지만, 명확히 적는것이 로직상 안전하므로 설정값을 추가함.

### ✅ 2. 고아 프로세스 정리 메서드

```java	
	/* 서버 실행 시 크롬 고아 프로세스 정리 */
	public void killOrphanByChrome() {
        try {
            // PPID가 1인 프로세스 전체 조회
            String command = "ps -ef | awk '$3 == 1'";
            Process process = Runtime.getRuntime().exec(new String[] {"bash", "-c", command});
            BufferedReader reader = new BufferedReader(new InputStreamReader(process.getInputStream()));
            String line;

            while ((line = reader.readLine()) != null) {
                if (line.contains("chrome")) {
                    String[] parts = line.trim().split("\\s+");
                    if (parts.length > 1) {
                        String pid = parts[1]; // PID는 두 번째 열
                        LOGGER.info("종료 대상 프로세스 : " + String.join(" | ", parts) );
                        Runtime.getRuntime().exec("kill -9 " + pid);
                    }
                }
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

고아 프로세스를 정리하는 메서드는  driver.quit() 를 호출하기 전 실행하도록 추가함
부모 PID가 1인 크롬 프로세스를 검색하고, 프로세스를 죽이고, 드라이버  quit로 세션을 종료하도록 하여 안전하게 프로세스와 드라이버 객체 초기화까지 할 수 있음

- ps -ef 명령어로 현재 시스템의 모든 프로세스를 조회
- 그중 부모 PID가 1인(고아 상태) 프로세스만 필터링
- 해당 프로세스의 명령어에 chrome이 포함되어 있는지 확인
- 발견된 경우, kill -9 명령으로 강제 종료

즉, 드라이버 세션을 종료하기 전에, 크롬이 남긴 프로세스를 먼저 청소하는 방식입니다.

> 왜 driver.quit() 이후가 아니라 직전에 해야 할까?

driver.quit() 이후에는 Selenium에서 관리하던 브라우저 세션이 사라지므로, 크롬과의 연결이 완전히 끊어집니다.
이후에 남아 있는 프로세스를 추적하더라도 어떤 드라이버가 만들었는지 관계없이 모든 프로세스를 죽게됩니다.
반대로 driver.quit() 직전에 상태를 점검하면 명확하게 해당 수집기에서 발생한 프로세스만 타겟팅할 수 있음
이후 driver.quit()으로 세션 정리를 마무리하면 메모리 누수가 없는 완전 종료가 됨


### ✅ 개선 이후 로그 확인

```shell
[2025-07-15 16:29:51]  INFO (runner.Class - tagetStoreSearch:342) -             크롬 세션 연결 30분 지남.. 세션 재 연결
[2025-07-15 16:29:52]  INFO (runner.Class - killOrphanByChrome:2692) - 종료 대상 프로세스 : root | 1563762 | 1 | 0 | 16:23 | ? | 00:00:00 | /opt/google/chrome/chrome_crashpad_handler | --monitor-self | --monitor-self-annotation=ptype=crashpad-handler | 
[2025-07-15 16:29:52]  INFO (runner.Class - killOrphanByChrome:2692) - 종료 대상 프로세스 : root | 1563764 | 1 | 0 | 16:23 | ? | 00:00:00 | /opt/google/chrome/chrome_crashpad_handler | 
[2025-07-15 16:29:52]  INFO (runner.Class - getReConnectSeleniumWebDriver:2657) - ✅종료 전 WebDriver 세션 ID: a88799802c90d734379f9a55e9150a3a
[2025-07-15 16:29:52]  INFO (runner.Class - getReConnectSeleniumWebDriver:2660) - 기존 WebDriver 세션 종료
[2025-07-15 16:29:52]  INFO (runner.Class - getReConnectSeleniumWebDriver:2663) - ✅종료 후 기존 driver.getSessionId(): null
[2025-07-15 16:30:14]  INFO (runner.Class - getConnectSeleniumWebDriver:2635) - ✅ChromeDriver 생성됨 - 세션 ID: f3079cfed4724a64c7b6f099ee2b05d5

```



`driver.quit()` 호출만으로는 완전한 프로세스 정리가 보장되지 않는 경우가 있습니다. 특히 크롬의 구조상 여러 하위 프로세스가 분리되어 실행되기 때문에, 고아 상태로 남은 프로세스를 주기적으로 감시하고 정리 해야 서버 메모리 누수 및 성능 저하를 예방할 수 있습니다.




`#selenium` `#chromedriver` `#리눅스프로세스` `#크롬고아프로세스` `#크롤링서버운영` `#프로세스정리` `#driver.quit` `#크롬메모리누수`