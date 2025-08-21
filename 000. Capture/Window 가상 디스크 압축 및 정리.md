
# Ubuntu VHD 크기(GB) 확인

```

$UbuntuVhd = "C:\Users\misouser\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc\LocalState\ext4.vhdx"
"{0:N2} GB" -f ((Get-Item $UbuntuVhd).Length/1GB)

```

> 57.62 GB

```powershell
$UbuntuVhd = "C:\Users\misouser\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc\LocalState\ext4.vhdx"

Mount-VHD -Path $UbuntuVhd -ReadOnly -NoDriveLetter
Optimize-VHD -Path $UbuntuVhd -Mode Full -Verbose
Dismount-VHD -Path $UbuntuVhd

```

좋습니다 👍 지금 확인된 값이 **57.62GB**라는 건, Ubuntu-22.04 WSL 가상디스크(`ext4.vhdx`) 자체 크기가 여전히 꽤 크다는 뜻이에요.

여기서 중요한 포인트는:  
👉 **Optimize-VHD는 “빈 공간”만 줄여줍니다.**  
즉, Ubuntu 내부에 실제 파일/패키지/도커 데이터가 남아있으면 아무리 압축해도 크기가 그대로예요.



### 1) Ubuntu 내부 청소

WSL(우분투 터미널)에서:
```
# APT 캐시 삭제
sudo apt-get clean

# 오래된 패키지 제거
sudo apt-get autoremove -y

# 도커 이미지/컨테이너/캐시 완전 제거
docker system prune -af --volumes
docker builder prune -af
docker buildx prune -af

# 불필요한 로그 정리 (큰 파일 확인)
sudo du -ah /var/log | sort -hr | head

```