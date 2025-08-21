

하드디스크가 426GB를 거의 꽉 채워져서 이미지를 빌드할 때 멈추고 카톡도 실행이 안되는 현상 발생


```bash
docker system df
```


TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          11        2         54.7GB    -4.751e+09B (-8%)
Containers      2         1         4.534MB   16.38kB (0%)
Local Volumes   0         0         0B        0B
Build Cache     227       0         15.6GB    15.6GB

사용중인 용량 확인



```
docker system prune -a
```

```
Total reclaimed space: 41.79GB
```
미사용 자원 정리




```
root@MISO-PC:/mnt/c/eGovFrameDev-4.3.0-64bit/workspace-egov/misoLLM_api# docker images
REPOSITORY            TAG                       IMAGE ID       CREATED         SIZE
llm                   latest                    ae51571002a8   43 hours ago    58.2GB
msrnd2/datascan       2.6.18.3                  3814e4bffaa7   7 days ago      2.9GB
msrnd2/dev-datascan   1.0.0                     cf6f7e28ed3d   7 days ago      2.58GB
msrnd2/datascan       2.6.18.2                  229e395606a6   7 days ago      2.89GB
datascan-api          nuitka                    c41426f7c9a8   8 days ago      2.89GB
datascan-api          pyminifier                5f93bbd55147   9 days ago      2.81GB
datascan-api          pyconcrete                76c810f39db6   9 days ago      2.85GB
datascan-api          enc                       c73a6be3ab81   9 days ago      2.87GB
msrnd2/datascan       2.6.18                    f3f574ff5703   13 days ago     2.81GB
ubuntu                latest                    7c06e91f61fa   3 weeks ago     117MB
nvidia/cuda           11.8.0-base-ubuntu22.04   f895871972c1   21 months ago   341MB
```

불필요한 이미지 삭제 



```bash
docker system df
```

```

TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          11        2         54.7GB    -4.751e+09B (-8%)
Containers      2         1         4.534MB   16.38kB (0%)
Local Volumes   0         0         0B        0B
Build Cache     227       0         15.6GB    15.6GB

```



### 도커 빌드 캐시 / 레이어 완전 정리

```bash
# 사용하지 않는 모든 빌드 캐시 제거 (-a=모든 스테이지, -f=확인 없이)
docker builder prune -af



```

```
Total:  24.52GB
```


```bash

# 혹시 남아있는 이미지 레이어/중지 컨테이너/네트워크도 재차 정리
docker system prune -af --volumes
```

```
Deleted Networks:
llm_network

Total reclaimed space: 0B
```

```bash
# 현황 확인
docker system df
```
```
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          0         0         0B        0B
Containers      0         0         0B        0B
Local Volumes   0         0         0B        0B
Build Cache     0         0         0B        0B
```


### WSL 가상디스크 용량 “압축”(진짜로 C: 여유공간 늘리는 단계)

```powershell
# 1) WSL 전체 종료
wsl --shutdown

# 2) 도커/우분투 디스크 압축 (Windows 11 최신 빌드에서 지원)
wsl --manage docker-desktop-data --compact
wsl --manage docker-desktop --compact
wsl --manage Ubuntu-22.04 --compact   # 우분투 배포판 이름은 환경에 맞게

```
