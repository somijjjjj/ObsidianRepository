


# 1) 도구 설치
sudo apt-get update && sudo apt-get install -y dos2unix

# 2) 현재 디렉토리의 모든 .sh 파일: CRLF → LF 변환
find . -type f -name "*.sh" -print0 | xargs -0 dos2unix

# 3) (중요) 파일 맨 앞 BOM 제거(있을 경우)
find . -type f -name "*.sh" -exec sed -i '1s/^\xEF\xBB\xBF//' {} +

# 4) 실행권한 재확인
find . -type f -name "*.sh" -exec chmod +x {} +

# 5) 확인
file -b ./entrypoint.sh
# => "UTF-8 Unicode text executable" (CRLF 언급 없어야 정상)



```
root@MISO-PC:/mnt/c/enc_builder# file -b ./entrypoint.sh 
Bourne-Again shell script, Unicode text, UTF-8 text executable, with CRLF line terminators
```
```
root@MISO-PC:/mnt/c/enc_builder# file -b ./enc.sh 
Bourne-Again shell script, Unicode text, UTF-8 text executable
root@MISO-PC:/mnt/c/enc_builder# file -b ./entrypoint.sh 
Bourne-Again shell script, Unicode text, UTF-8 text executable
```