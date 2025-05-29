---
tistory: https://jn-silveryoung.tistory.com/48
---

Ubuntu 서버에 Redis를 설치하려면 다음 단계를 따르시면 됩니다.

### 1. 패키지 업데이트
먼저, 패키지 목록을 업데이트합니다.

```bash
sudo apt update
```

### 2. Redis 설치
Redis를 설치하려면 다음 명령을 사용합니다.

```bash
sudo apt install redis-server
```

### 3. Redis 설정 (옵션)
설치 후 Redis의 기본 설정을 편집할 수 있습니다. 설정 파일은 일반적으로 `/etc/redis/redis.conf`에 위치해 있습니다.

```bash
sudo nano /etc/redis/redis.conf
```

#### 예: 백그라운드 실행 설정
Redis를 백그라운드에서 실행하려면 `daemonize` 옵션을 `yes`로 설정합니다.

```plaintext
daemonize yes
```

설정을 완료했으면 저장하고 나옵니다. (`Ctrl + X`, `Y`, `Enter`)

### 4. Redis 서비스 재시작
설정을 적용하려면 Redis 서비스를 재시작합니다.

```bash
sudo systemctl restart redis-server
```

### 5. Redis 자동 시작 활성화 (서버 재부팅 시 자동 실행)
Redis를 서버 시작 시 자동으로 실행되도록 설정하려면 다음 명령을 사용합니다.

```bash
sudo systemctl enable redis-server
```

### 6. 설치 확인
Redis가 제대로 설치되었는지 확인하려면 Redis CLI를 실행하여 `ping` 명령을 입력해 봅니다.

```bash
redis-cli
```

이후에 `ping`을 입력했을 때 `PONG` 응답이 나오면 Redis가 정상적으로 동작하는 것입니다.

```plaintext
127.0.0.1:6379> ping
PONG
```