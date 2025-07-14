---
tags:
  - status/done
---
# 🛠️ Selenium ChromeDriver 버전 불일치 오류 해결 과정

## 🖥️ 시스템 환경

| 항목 | 값 |
|------|-----|
| OS | Amazon Linux 2023.7.20250609 (`kernel 6.1.140-154.222.amzn2023.x86_64`) |
| Java | 11.0.27 |
| Chrome | 137.0.7151.119 |
| ChromeDriver | 137.0.7151.119 |
| Selenium | 4.25.0 |

---

## 📌 발생한 에러 요약

```text
Could not start a new session. Response code 500.
Message: session not created: This version of ChromeDriver only supports Chrome version 132
Current browser version is 137.0.7151.119
````

- Selenium이 **Chrome 137 브라우저를 실행하려 했으나**,  
    **ChromeDriver는 132 버전까지만 지원**하는 상태였음.
    

---

## 🔍 문제 원인

Chrome과 ChromeDriver의 **메이저 버전 불일치**로 인해 Selenium이 브라우저 세션을 생성하지 못함.

---

## ✅ 해결 과정

### 1. 기존 chromedriver 위치 확인 및 버전 확인

```bash
/data001/crawl/ob/chromedriver/chromedriver --version
# 결과: ChromeDriver 132.x (불일치)
```

### 2. 기존 chromedriver 제거

```bash
sudo rm -f /data001/crawl/ob/chromedriver/chromedriver
```

### 3. Chrome 137에 맞는 ChromeDriver 다운로드 및 설치

```bash
cd /data001/crawl/ob/chromedriver/

wget https://storage.googleapis.com/chrome-for-testing-public/137.0.7151.119/linux64/chromedriver-linux64.zip
unzip chromedriver-linux64.zip

mv chromedriver-linux64/chromedriver ./chromedriver
chmod +x ./chromedriver
```

### 4. 설치된 버전 확인

```bash
./chromedriver --version
# 결과: ChromeDriver 137.0.7151.119 (정상)
```


---


- ✅ ChromeDriver 버전 137 설치 및 정상 적용
    
- ✅ Selenium을 통한 브라우저 제어 성공
    
- ⚠️ CDP 경고는 DevTools 디버깅 기능과 관련된 것으로, 크롤링에는 영향 없음
    
    ```text
    Unable to find CDP implementation matching 137
    ```
    

---

## 📝 참고

- Chrome ↔ ChromeDriver는 반드시 **메이저 버전 일치** 필요
    
- [ChromeDriver 공식 버전 다운로드](https://googlechromelabs.github.io/chrome-for-testing/)
    
- [Selenium 공식 문서 - Driver 오류 처리](https://www.selenium.dev/documentation/webdriver/troubleshooting/errors/driver_location/)
    
```