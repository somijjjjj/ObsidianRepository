

![[Pasted image 20250201203550.png]]

### 8080포트 프로세스 확인
```
netstat -ano | findstr :8080
```

### PID로 사용중인 프로세스 정보 확인
```
tasklist /FI "PID eq 4740"
```

### 프로세스 종료
```
taskkill /PID 4740 /F
```
