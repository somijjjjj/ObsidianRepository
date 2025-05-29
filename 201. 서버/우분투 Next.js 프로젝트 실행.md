
1. git 원격 저장소의 변경 사항 가져오기

```bash
git pull
```
- username : 계정 이름 입력
- password : 계정 토큰 입력


2. 프로젝트 패키지 설치
```
yarn install --force
```



3. 프로젝트 빌드 

```bash
yarn build
```
    
`yarn build`가 성공적으로 완료되면, `.next` 폴더에 빌드된 파일이 생성됨


4. PM2 프로세스 확인 (백그라운드 실행)

```bash
pm2 list
```
![[Pasted image 20250216223731.png]]
- web 프로세스 존재 시 **재시작**
    
```bash
pm2 restart nextjs-app
```

- web 프로세스가 존재하지 않으면 프로세스 실행 

```bash
pm2 start yarn --name "web" -- start
```


--- 
**추가 관리 명령어**

- PM2 로그 확인 
    ```bash
    pm2 logs web
    ```
    이 명령어로 Next.js 앱의 **실행 로그**를 실시간으로 확인할 수 있습니다.
    

- 프로세스 종료 
    ```bash
    pm2 stop web
    ```
    
---
