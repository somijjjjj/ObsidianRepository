좋습니다 👍 이런 이슈는 WSL2 + Docker Desktop 혼용 환경에서 자주 발생하는 대표적인 문제라, 정리해두면 재발했을 때 바로 해결할 수 있어요.  
옵시디언 노트로 바로 붙여넣을 수 있게 마크다운 형식으로 정리해드릴게요.

## 📌 증상

```bash
docker images
# 결과:
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

- Docker Desktop을 켜면 `docker images`가 비어 있음
    
- 예전 WSL 내부 도커 엔진에 있던 이미지가 안 보임
    

---

## 🧐 원인

- **도커 엔진(데몬)은 둘 이상 존재 가능**
    
    1. **WSL 내부 엔진 (dockerd)** → `/var/lib/docker`에 이미지 저장
        
    2. **Docker Desktop 엔진 (desktop-linux)** → Windows Docker Desktop이 관리
        

👉 `docker` CLI는 현재 연결된 엔진(context)에 따라 다른 이미지 목록을 보여줌.  
즉, 이미지가 "사라진 것"이 아니라 **다른 엔진에 저장**되어 있는 것.

---

## ✅ 해결 방법

### 1) 현재 어떤 엔진에 연결됐는지 확인

```bash
docker context ls
docker info | grep -E 'Context|Operating System|Docker Root Dir'
```

- `Operating System: Docker Desktop` → Desktop 엔진
    
- `Operating System: Ubuntu` 또는 Linux → WSL 로컬 엔진
    

---

### 2) WSL 로컬 엔진으로 전환

1. Docker Desktop **종료** 또는 **WSL Integration 해제**
    
2. `/etc/wsl.conf` 설정 (systemd 활성화)

```ini
[boot]
systemd=true
```
    
적용:

```powershell
wsl --shutdown
```
    
3. WSL 재실행 후 도커 기동
    
    ```bash
    sudo systemctl start docker
    docker context use default
    docker images
    ```
    

---

### 3) Docker Desktop 엔진으로 전환

```bash
docker context use desktop-linux
docker images
```

---

### 4) 엔진 간 이미지 이전

WSL → Desktop으로 옮기기:

```bash
# WSL 엔진에서
docker save repo:tag -o /mnt/c/tmp/repo_tag.tar

# Desktop 엔진에서
docker load -i /mnt/c/tmp/repo_tag.tar
```

---

## ⚠️ 자주 발생하는 이슈 & 해결

- **`Cannot connect to the Docker daemon`**  
    → `sudo systemctl start docker` 또는 `sudo service docker start`
    
- **계속 Docker Desktop 엔진으로 붙음**  
    → Docker Desktop 종료 or Integration 해제 필요
    
- **권한 문제** (`Got permission denied`)
    
    ```bash
    sudo usermod -aG docker $USER
    newgrp docker
    ```
    

---

## 🔑 기억 포인트

- `docker images` 결과가 다른 이유는 **데몬이 다르기 때문**
    
- 항상 `docker info`에서 **Operating System**을 확인할 것
    
- 필요하면 `save/load`로 엔진 간 이미지 옮기기
    
