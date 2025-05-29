---
tistory: https://jn-silveryoung.tistory.com/49
---

**Nginx를 사용해 애플리케이션을 배포하는 방법**은 Nginx를 **리버스 프록시**로 설정하여 외부 트래픽을 애플리케이션 서버(Node.js 등)로 전달하는 방식입니다. 이를 통해 사용자는 표준 HTTP 포트(80번) 또는 HTTPS 포트(443번)를 통해 애플리케이션에 접근할 수 있습니다.

---

## **1. Nginx 설치**

1. **Nginx 설치**:
   Ubuntu 기준으로 Nginx를 설치합니다:
   ```bash
   sudo apt update
   sudo apt install nginx
   ```

2. **Nginx 서비스 시작 및 활성화**:
   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

3. **Nginx 상태 확인**:
   ```bash
   sudo systemctl status nginx
   ```

4. **80번 포트 허용**:
   방화벽에서 80번 포트를 허용합니다:
   ```bash
   sudo ufw allow 'Nginx Full'
   ```

- ufw 로 허용해도 접속이 안될경우 iptables 명령어로 포트 허용할 것
    - error: port 80 after 1 ms No route to host 발생

```bash
root@was-instance:/home/ubuntu# curl http://150.230.252.242
curl: (7) Failed to connect to 150.230.252.242 port 80 after 0 ms: No route to host
root@was-instance:/home/ubuntu# sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
```


- 설정 저장
```
root@was-instance:/home/ubuntu# netfilter-persistent save
run-parts: executing /usr/share/netfilter-persistent/plugins.d/15-ip4tables save
run-parts: executing /usr/share/netfilter-persistent/plugins.d/25-ip6tables save


root@was-instance:/home/ubuntu# curl http://150.230.252.242
{"timestamp":"2025-01-18T05:09:58.711+00:00","status":404,"error":"Not Found","path":"/"}
```
---

## **2. React와 Node.js 애플리케이션 배포 준비**

### **React 앱 빌드**
React 프로젝트에서 정적 파일을 생성합니다:
```bash
npm run build
```
- 생성된 `build/` 폴더가 배포에 사용됩니다.

---

## **3. Node.js 서버 준비**

Node.js 서버를 통해 React 빌드 파일을 제공하는 `server.js`를 작성합니다. 예를 들어:

```javascript
const express = require('express');
const path = require('path');
const app = express();

const PORT = process.env.PORT || 3000;

// 정적 파일 제공
app.use(express.static(path.join(__dirname, 'dist')));

// SPA를 위해 모든 요청을 index.html로 리다이렉트
app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, 'dist', 'index.html'));
});

app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

---

## **4. PM2로 Node.js 서버 실행**

1. PM2로 서버 실행:
   ```bash
   pm2 start server.js --name insider
   ```

2. PM2 상태 확인:
   ```bash
   pm2 list
   ```

3. PM2 설정 저장(서버 재부팅 후에도 실행 유지):
   ```bash
   pm2 save
   pm2 startup
   ```

---

## **5. Nginx 리버스 프록시 설정**

### **Nginx 설정 파일 수정**
1. Nginx 설정 파일 열기:
   ```bash
   sudo nano /etc/nginx/sites-available/default
   ```

2. 아래 내용을 추가:
   ```nginx
   server {
       listen 80;
       server_name your-domain.com; # 또는 서버 IP

       location / {
           proxy_pass http://localhost:3000; # Node.js 서버
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

3. 설정 저장 후 Nginx 테스트:
   ```bash
   sudo nginx -t
   ```

4. Nginx 재시작:
   ```bash
   sudo systemctl restart nginx
   ```

---

## **6. HTTPS 설정 (선택 사항)**

1. **Certbot 설치**:
   ```bash
   sudo apt install certbot python3-certbot-nginx
   ```

2. **SSL 인증서 발급**:
   ```bash
   sudo certbot --nginx -d inssider.kro.kr 
   ```



3. **자동 갱신 설정 확인**:
   ```bash
   sudo certbot renew --dry-run
   ```

---

## **7. 결과 확인**

- 브라우저에서 `http://your-domain.com` 또는 `http://<서버 IP>`로 접속.
- HTTPS 설정을 완료했다면 `https://your-domain.com`으로 접속.

---

## **요약**

1. Nginx 설치 및 방화벽 설정.
2. React 앱 빌드 → Node.js 서버로 제공.
3. PM2로 Node.js 서버 실행.
4. Nginx 설정 파일에서 리버스 프록시 설정.
5. (선택) HTTPS 적용.

이 과정을 통해 Nginx를 사용한 안정적이고 효율적인 배포를 완료할 수 있습니다. 😊


---

### 다른 이슈사항

node.js 버전 문제

- ES Module 방식에서 **CommonJS 스타일의 모듈**을 가져오려면 호환성을 고려해야 합니다.
- ES Module을 사용하는 경우, Node.js 버전이 최소 14 이상이어야 하며, 16 이상을 권장합니다.  
    버전을 확인하려면: `node -v`