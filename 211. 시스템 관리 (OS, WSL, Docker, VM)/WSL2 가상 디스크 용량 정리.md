

[[2025-08-21]]

![AS-IS](image.png)

AS-IS

![TO-BE](image%201.png)

TO-BE

- 디스크 압축을 통해 173.1GB 용량을 확보한 과정입니다.

### 1. WSL ext4.vhdx 파일 위치 찾기

```bash
C:\Users\{사용자}\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc\LocalState
```

- 개인마다 위치는 다를 수 있으나 보통 해당 위치에 존재함

### 2. WSL 종료

1. PowerShell 관리자 권한으로 실행
2. Docker Desktop 종료
3. wsl 실행 현황

```bash
wsl -l -v
```

```bash
  NAME              STATE           VERSION
* docker-desktop    Stopped         2
  Ubuntu-22.04      Running         2
```

1. Ubuntu 종료

```bash
wsl -t Ubuntu-22.04
```

```bash
PS C:\WINDOWS\system32> wsl -l -v
  NAME              STATE           VERSION
* docker-desktop    Stopped         2
  Ubuntu-22.04      Stopped         2
```

### 3. diskpart 실행

1. PowerShell에서 diskpart를 실행한다.

```bash
diskpart
```

```bash
Microsoft DiskPart 버전 10.0.26100.1150

Copyright (C) Microsoft Corporation.
컴퓨터: MISO-PC
```

1. 커맨드 순서대로 입력

```bash
select vdisk file="C:\Users\{사용자}\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc\LocalState\ext4.vhdx"
```

- ext4.vhdx의 경로 입력해주면 됨

```bash
DiskPart가 가상 디스크 파일을 선택했습니다.
```

1. 디스크 파일 연결

```bash
attach vdisk readonly
```

```bash

  100 퍼센트 완료

DiskPart가 가상 디스크 파일을 연결했습니다.
```

1. 가상 디스크 파일 압축

```bash
compact vdisk
```

```bash
  100 퍼센트 완료

DiskPart가 가상 디스크 파일을 압축했습니다.
```

1. 가상 디스크 파일 분리 

```bash
detach vdisk
```

```bash

DiskPart가 가상 디스크 파일을 분리했습니다.
```

디스크 압축이라는게 결국은 현재 안쓰는 공간을 압축하는 거기 때문에 만약 모든 공간이 실제로 사용중이라면 디스크 용량이 다이나믹하게 줄어들진 않는다.
wsl은 한번 늘어난 디스크 용량을 자동으로 감소시키지 않기 때문에 wsl 서버에 접속 후 먼저 쓰지 않는 파일들을 직접 정리해주고 압축을 실행한다면 용량이 확 줄어드는 걸 볼 수 있다.

- 출처: [https://uutopia.tistory.com/70](https://uutopia.tistory.com/70) [하루, 하나, IT지식:티스토리]